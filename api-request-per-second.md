# How to handle large amount of API Requests

## Example: how to handle 1000 requests per second

Answer:

1. Using async / non-blocking operations to handle more requests simultaneously
2. Avoid slow DB calls in critical paths
3. Using Fastify (10k RPS) instead of Express (2 - 3k RPS)
4. Enable NodeJS cluster module to utilize multi-core systems

```javascript
// Point 4 example
const cluster = require('cluster');
const { cpus } = require('os');
const { createServer } = require('http');

const numCPUs = cpus().length;

if (cluster.isPrimary) {
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  createServer((req, res) => {
    res.end('Ok');
  }).listen(3000);
}
```

5. DB optimization:
  - Ensure indexes on query fields
  - Avoid N + 1 queries
  - Using cache mechanisms if possible (eg: Redis)

6. Using HTTP2/3
  - Reduce number of TCP connections creation
  - Use HTTP/2 multiplexing

7. Using Load Balancer
  - Put NGINX in front of API servers
  - Distributed load into multiple backend instances (scale horizontally)
