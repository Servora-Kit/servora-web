# servora-web

Shared frontend libraries for [Servora-Kit](https://github.com/Servora-Kit) web applications.

## Packages

| Package | npm | Description |
|---------|-----|-------------|
| [`@servora/client`](./packages/client/) | [![npm](https://img.shields.io/npm/v/@servora/client)](https://www.npmjs.com/package/@servora/client) | HTTP request handler, token management, Kratos error parsing |
| [`@servora/proto-utils`](./packages/proto-utils/) | [![npm](https://img.shields.io/npm/v/@servora/proto-utils)](https://www.npmjs.com/package/@servora/proto-utils) | Proto/Kratos API query utilities: filter, order_by, FieldMask |

## Installation

```bash
pnpm add @servora/client
pnpm add @servora/proto-utils
```

## Usage

```typescript
import { createRequestHandler } from '@servora/client/request'
import { parseKratosError, kratosMessage } from '@servora/client/errors'
import { makeFilter, makeOrderBy, makeUpdateMask } from '@servora/proto-utils/query'
```

## Local Development

This repo is part of the [servora-kit](https://github.com/Servora-Kit) workspace. For local development:

```bash
# Clone the workspace
git clone git@github.com:Servora-Kit/servora-web.git

# In the servora-kit workspace root
pnpm install
```

In the kit workspace, pnpm links the local `servora-web/packages/client` (current version in its `package.json`). On npm, install with `^0.0.2` after tag `pkg/v0.0.2` is published; until then the latest public release may still be `0.0.1`. In the local workspace, `linkWorkspacePackages: true` automatically symlinks to the source — equivalent to Go's `go.work` replace directive.

## License

[MIT](./LICENSE)
