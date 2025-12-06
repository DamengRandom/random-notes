# tRPC: Introduction and Example

`tRPC` is an open source Remote Procedure Call (RPC) framework for building fully type-safe APIs in TypeScript.

## Why tRPC
- End-to-end type safety with zero schema or codegen.
- Strong input validation with `zod`.
- Simple mental model: routers and procedures (queries/mutations).
- Works with Node, Express/Fastify, Next.js, Vite, and more.
- Efficient networking via HTTP batch links and optional websockets.

## Core Concepts
- Router: A collection of procedures.
- Procedure: A unit of work; can be a `query` (read) or `mutation` (write).
- Input validation: Typically via `zod`, ensuring runtime safety that matches TypeScript types.
- Context: Shared per-request data (e.g., auth info).
- Type inference: Export your `AppRouter` type from the server and use it on the client for automatic typing.

## Quick Start Example (Node + Express)

Install dependencies:
```bash
npm i @trpc/server @trpc/client zod express
```

Server:
```ts
import { initTRPC } from '@trpc/server'
import { z } from 'zod'
import express from 'express'
import { createExpressMiddleware } from '@trpc/server/adapters/express'

type Context = {}
const createContext = (): Context => ({})

const t = initTRPC.context<Context>().create()

const appRouter = t.router({
  greet: t.procedure
    .input(z.object({ name: z.string() }))
    .query(({ input }) => `Hello, ${input.name}`),

  getPost: t.procedure
    .input(z.object({ id: z.number().int().positive() }))
    .query(({ input }) => ({ id: input.id, title: 'Example', body: '...' })),

  createPost: t.procedure
    .input(z.object({ title: z.string().min(1), body: z.string().min(1) }))
    .mutation(({ input }) => ({ id: 1, ...input })),
})

export type AppRouter = typeof appRouter

const app = express()

app.use(
  '/trpc',
  createExpressMiddleware({
    router: appRouter,
    createContext,
  }),
)

app.listen(4000)
```

Client:
```ts
import { createTRPCProxyClient, httpBatchLink } from '@trpc/client'
import type { AppRouter } from './server'

const client = createTRPCProxyClient<AppRouter>({
  links: [
    httpBatchLink({ url: 'http://localhost:4000/trpc' }),
  ],
})

async function main() {
  const greeting = await client.greet.query({ name: 'World' })
  const post = await client.getPost.query({ id: 1 })
  const created = await client.createPost.mutate({ title: 'Hello', body: 'tRPC' })
  console.log({ greeting, post, created })
}

main()
```

## How It Works
- Input shapes are defined once, validated at runtime with `zod`, and inferred at compile time on the client.
- The server exports `AppRouter`, which the client uses strictly as a type to infer available procedures and their input/output shapes.
- Queries return data without side effects; mutations change state and may return updated data.

## Next Steps
- Add authentication via context and middlewares.
- Split routers by domain and merge them.
- Switch to HTTP batching or websockets as needed.
- Integrate with React using `@trpc/react-query` for declarative data fetching.

## My own understanding

- Its a way to write API (from backend to frontend with type-safety feature)
- define a api from router (backend service side)
- frontend get call by query/mutation
- procedure is a request (eg: a query, mutation)

## Middleware

### Example: using auth middleware to secure the tRPC routes / procedures

```ts
import { initTRPC } from '@trpc/server';
import superjson from 'superjson';
import { type CreateNextContextOptions } from '@trpc/server/adapters/next';
import { verifyAuth } from '@/lib/auth';

export const createTRPCContext = async (opts: CreateNextContextOptions): Promise<Context> => {
  const { req, res } = opts;
  let user: UserPayload | null = null;

  try {
    // Extract token from various sources
    const token = extractTokenFromRequest(req);
    
    if (token) {
      user = await verifyAccessToken(token);
    }
  } catch (error) {
    // Token verification failed, user stays null
    console.debug('Auth failed:', error);
  }

  return {
    req,
    res,
    user,
  };
};


const t = initTRPC.context<Awaited<ReturnType<typeof createTRPCContext>>>().create({
  transformer: superjson,
  errorFormatter({ shape, error }) {
    // Log unauthorized errors differently
    if (error.code === 'UNAUTHORIZED') {
      console.warn('Unauthorized access attempt');
    }
    
    return {
      ...shape,
      data: {
        ...shape.data,
        // Add custom error data if needed
      }
    };
  }
});

// create middleware now for checking user, if user exists, means jwt authorization process passed ✅ ~
const isAuthed = t.middleware(async ({ ctx, next }) => {
  if (!ctx.user) {
    throw new TRPCError({ 
      code: 'UNAUTHORIZED', 
      message: 'You must be logged in to access this resource' 
    });
  }

  return next({
    ctx: {
      ...ctx,
      // Type-safe: JWT authorization passed, can return user info
      user: ctx.user,
    },
  });
});

export const createTRPCRouter = t.router;
export const privateProcedure = t.procedure.use(isAuthed); // private procedure with routes
export const publicProcedure = t.procedure;
```


```ts
// Just love this part of code, which seems like a good practice here ~
import { SignJWT, jwtVerify } from 'jose';
import { nanoid } from 'nanoid';

export interface UserPayload {
  userId: string;
  email: string;
  role: string;
}

export interface TokenPayload extends jwt.JWTPayload {
  sub: string;
  email: string;
  role: string;
  type: 'access' | 'refresh';
  jti: string;
}

function extractTokenFromRequest(req: NextApiRequest): string | null {
  // Choice 1. Check Authorization header
  const authHeader = req.headers.authorization;

  if (authHeader?.startsWith('Bearer ')) {
    return authHeader.substring(7);
  }

  // Choice 2. Check query parameter (less secure, for dev/testing)
  if (req.query.token && typeof req.query.token === 'string') {
    return req.query.token;
  }

  return null;
}

export async function createAccessToken(payload: Omit<UserPayload, 'role'> & { role?: string }): Promise<string> {
  const secret = getJwtSecret();
  
  return await new SignJWT({
    sub: payload.userId,
    email: payload.email,
    role: payload.role || 'user',
    type: 'access',
  })
    .setProtectedHeader({ alg: 'HS256' })
    .setJti(nanoid())
    .setIssuedAt()
    .setExpirationTime('15m')
    .sign(secret);
}

export async function verifyAccessToken(token: string): Promise<UserPayload | null> {
  try {
    const secret = getJwtSecret();
    const { payload } = await jwtVerify<TokenPayload>(token, secret);
    
    // Validate required claims
    if (!payload.sub || !payload.email || payload.type !== 'access') {
      return null;
    }
    
    // Check expiration (jwtVerify does this, but we double-check)
    if (payload.exp && payload.exp < Math.floor(Date.now() / 1000)) {
      return null;
    }
    
    return {
      userId: payload.sub,
      email: payload.email,
      role: payload.role || 'user',
    };
  } catch (error) {
    // Invalid signature, malformed token, etc.
    return null;
  }
}

// Helper to get JWT secret
export function getJwtSecret(): Uint8Array {
  const secret = process.env.JWT_SECRET_KEY;
  if (!secret || secret.length < 32) {
    throw new Error('JWT_SECRET_KEY must be at least 32 characters');
  }
  return new TextEncoder().encode(secret);
}
```