# servora-web

Shared frontend libraries for [Servora-Kit](https://github.com/Servora-Kit) web applications.

## Packages

| Package | npm | Description |
|---------|-----|-------------|
| [`@servora/web-pkg`](./packages/pkg/) | [![npm](https://img.shields.io/npm/v/@servora/web-pkg)](https://www.npmjs.com/package/@servora/web-pkg) | HTTP request handler, token management, Kratos error parsing |

## Installation

```bash
npm install @servora/web-pkg
# or
pnpm add @servora/web-pkg
```

## Usage

```typescript
import { createRequestHandler } from '@servora/web-pkg/request'
import { parseKratosError, kratosMessage } from '@servora/web-pkg/errors'
```

## Local Development

This repo is part of the [servora-kit](https://github.com/Servora-Kit) workspace. For local development:

```bash
# Clone the workspace
git clone git@github.com:Servora-Kit/servora-web.git

# In the servora-kit workspace root
pnpm install
```

In the kit workspace, pnpm links the local `servora-web/packages/pkg` (current version in its `package.json`). On npm, install with `^0.0.2` after tag `pkg/v0.0.2` is published; until then the latest public release may still be `0.0.1`. In the local workspace, `linkWorkspacePackages: true` automatically symlinks to the source — equivalent to Go's `go.work` replace directive.

## License

[MIT](./LICENSE)
