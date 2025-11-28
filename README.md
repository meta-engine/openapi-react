# MetaEngine OpenAPI React

[![npm version](https://img.shields.io/npm/v/@metaengine/openapi-react.svg)](https://www.npmjs.com/package/@metaengine/openapi-react)
[![npm downloads](https://img.shields.io/npm/dm/@metaengine/openapi-react.svg)](https://www.npmjs.com/package/@metaengine/openapi-react)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Generate TypeScript models and React hooks from OpenAPI/Swagger specifications.**

Perfect for API-first development with TanStack Query integration and native Fetch API.

---

## Quick Links

- **NPM Package**: [@metaengine/openapi-react](https://www.npmjs.com/package/@metaengine/openapi-react)
- **NuGet Package**: [MetaEngine.TypeScript.OpenApi.React](https://www.nuget.org/packages/MetaEngine.TypeScript.OpenApi.React)
- **Website**: [metaengine.eu/packages/openapi-react](https://www.metaengine.eu/packages/openapi-react)

---

## Features

- ✅ **React 18+** - Modern React with hooks
- ✅ **TypeScript** - Fully typed API clients and models
- ✅ **TanStack Query** - Optional integration with useQuery/useMutation
- ✅ **Fetch API** - No dependencies, uses native fetch
- ✅ **Monorepo Friendly** - Configurable environment variables per API
- ✅ **Framework Agnostic** - Works with Vite, Next.js, Create React App
- ✅ **Tree-shakeable** - Optimized bundle size

---

## Installation

```bash
npm install --save-dev @metaengine/openapi-react
```

Or use directly with npx:

```bash
npx @metaengine/openapi-react <input> <output>
```

---

## Requirements

- Node.js 18.0 or later
- .NET 8.0 or later runtime ([Download](https://dotnet.microsoft.com/download))
- React 18.0 or later (for generated code)

---

## Quick Start

### Recommended Setup

```bash
npx @metaengine/openapi-react api.yaml ./src/api \
  --tanstack-query \
  --documentation
```

### With npm scripts

Add to your `package.json`:

```json
{
  "scripts": {
    "generate:api": "metaengine-openapi-react api.yaml ./src/api --tanstack-query --documentation"
  }
}
```

Then run:
```bash
npm run generate:api
```

### Framework-Specific Examples

**Vite:**
```bash
npx @metaengine/openapi-react api.yaml ./src/api \
  --base-url-env VITE_API_URL \
  --tanstack-query \
  --documentation
```

**Next.js:**
```bash
npx @metaengine/openapi-react api.yaml ./src/api \
  --base-url-env NEXT_PUBLIC_API_URL \
  --tanstack-query \
  --documentation
```

**Create React App:**
```bash
npx @metaengine/openapi-react api.yaml ./src/api \
  --base-url-env REACT_APP_API_BASE_URL \
  --tanstack-query \
  --documentation
```

### More Examples

```bash
# From URL
npx @metaengine/openapi-react https://api.example.com/openapi.json ./src/api

# Filter by OpenAPI tags
npx @metaengine/openapi-react api.yaml ./src/api --include-tags users,auth

# Monorepo with multiple APIs
npx @metaengine/openapi-react users-api.yaml ./src/api/users --base-url-env VITE_USERS_API_URL
npx @metaengine/openapi-react orders-api.yaml ./src/api/orders --base-url-env VITE_ORDERS_API_URL
```

---

## CLI Options

| Option | Description | Default |
|--------|-------------|---------|
| `--include-tags <tags>` | Filter by OpenAPI tags (comma-separated, case-insensitive) | - |
| `--base-url-env <name>` | Environment variable name for base URL | `REACT_APP_API_BASE_URL` |
| `--service-suffix <suffix>` | Service naming suffix | `Api` |
| `--options-threshold <n>` | Parameter count for options object | `4` |
| `--documentation` | Generate JSDoc comments | `false` |
| `--tanstack-query` | Enable TanStack Query integration | `false` |
| `--strict-validation` | Enable strict OpenAPI validation | `false` |
| `--verbose` | Enable verbose logging | `false` |
| `--help, -h` | Show help message | - |

---

## Environment Variables

Configure your base URL via environment variables. The generator creates an `api-config.ts` file that reads from the appropriate variable:

**Vite** (prefix: `VITE_`):
```bash
# .env
VITE_API_URL=http://localhost:3000/api
```

**Next.js** (prefix: `NEXT_PUBLIC_`):
```bash
# .env
NEXT_PUBLIC_API_URL=http://localhost:3000/api
```

**Create React App** (prefix: `REACT_APP_`):
```bash
# .env
REACT_APP_API_BASE_URL=http://localhost:3000/api
```

---

## Usage Examples

### Basic API Calls

```typescript
import { usersApi } from './api/users-api';

// Fetch users
const users = await usersApi.getUsers();

// Create a user
const newUser = await usersApi.createUser({
  name: 'John',
  email: 'john@example.com'
});
```

### With TanStack Query Hooks

```typescript
import { useGetUsers, useCreateUser } from './api/users-api';

function UserList() {
  const { data: users, isLoading, error } = useGetUsers();
  const createUser = useCreateUser();

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      {users?.map(u => <div key={u.id}>{u.name}</div>)}
      <button onClick={() => createUser.mutate({
        name: 'Jane',
        email: 'jane@example.com'
      })}>
        Add User
      </button>
    </div>
  );
}
```

### Monorepo Setup

```bash
# Users microservice
npx @metaengine/openapi-react users-api.yaml ./src/api/users \
  --base-url-env VITE_USERS_API_URL \
  --tanstack-query

# Orders microservice
npx @metaengine/openapi-react orders-api.yaml ./src/api/orders \
  --base-url-env VITE_ORDERS_API_URL \
  --tanstack-query
```

Then configure in `.env`:
```bash
VITE_USERS_API_URL=http://localhost:3001/api
VITE_ORDERS_API_URL=http://localhost:3002/api
```

---

## Generated Code Structure

```
output/
  ├── models/                    # One file per model
  │   ├── user.ts               # export interface User { ... }
  │   ├── product.ts
  │   └── ...
  ├── users-api.ts              # Users API with hooks
  ├── products-api.ts           # Products API with hooks
  ├── api-config.ts             # Base URL configuration
  ├── alias-types.ts            # Type aliases
  ├── dictionary-types.ts       # Dictionary/map types
  └── union-types.ts            # Union types
```

---

## TanStack Query Integration

When using `--tanstack-query`, the generator creates:
- `useQuery` hooks for GET operations
- `useMutation` hooks for POST/PUT/PATCH/DELETE operations
- Properly typed query keys
- Automatic cache invalidation

**Note:** You must install `@tanstack/react-query` separately:
```bash
npm install @tanstack/react-query
```

---

## Programmatic Usage

The NuGet package allows programmatic use in .NET projects. See the [website documentation](https://www.metaengine.eu/packages/openapi-react) for full C# API reference.

---

## Support

- **Issues**: [GitHub Issues](https://github.com/meta-engine/openapi-react/issues)
- **Email**: info@metaengine.eu
- **Website**: [metaengine.eu](https://www.metaengine.eu)

---

## License

MIT License - see [LICENSE](./LICENSE) file for details.

---

## About This Repository

This is the **documentation and issue tracking repository** for MetaEngine OpenAPI React. The compiled NPM package is available at [@metaengine/openapi-react](https://www.npmjs.com/package/@metaengine/openapi-react).

Source code is proprietary, but the package is free to use under MIT license.
