# `package.json` vs `package-lock.json`

## Definition

- `package.json`: a manifest file that contains `metadata`, `scripts`, `dependencies` + `devdependencies` about a project.
  - `metadata`: project information (name, version, description, author, license, etc.)
  - `scripts`: custom commands to run (e.g., `npm start`, `npm test`)
  - `dependencies`: `runtime` dependencies required for the project to function
  - `devdependencies`: dependencies required `only for development` (e.g., linters, build tools)

- `package-lock.json`: a lock file that contains the `exact versions` of dependencies installed in a project (Version map ~).
  - Never manually edit `package-lock.json` file
  - Contains complete `dependency tree` structure
  - Has integrity checksums (`for security`)

- Commit both files for version control

```bash
# Adds dependency and updates both files
npm install package-name

# Installs exact versions from lock file (recommended for CI)
npm ci

# Updates lock file after modifying package.json
npm install
```
