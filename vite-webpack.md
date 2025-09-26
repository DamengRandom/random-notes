# Vite & Webpack

## Vite is `faster` than webpack

- Reason 1:

`Webopack` traditionally relies on `bundling all modules` before serving them to the browser, but `Vite` is serves files without requring full bundling step (more based `on demand`)

- Reason 2:

Vite uses `esbuild` which is written by `Go` programming language which has better performance. Webpack is using JavaScript-based bundler which is slower than using Go lang.

**The major explanation is because of `JavaScript` is a `interpreted language` which means it has to `parse` and `execute` the code `on the fly` which is slower than `compiled language` like `Go`, especially for the computationally internsive tasks, like building & transpilation parts.**

- Reason 3:

Vite has HMR (Hot Module Replacement) which is faster than webpack's HMR, because Vite only invalidate the affacted modules and its dependencies


Conclusion, Vite is faster than Webpack in terms of `development speed` and `HMR (Hot Module Replacement)`.