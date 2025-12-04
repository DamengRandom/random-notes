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
