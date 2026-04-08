# Changelog

All notable changes to `@metaengine/openapi-react` will be documented in this file.

## [1.0.3] - 2026-04-08

### Features

- **New: `--type-mapping <slug=target>` flag** — Opt-in override for the TypeScript type emitted for a given OpenAPI `(type, format)` pair. Repeatable. Unknown slugs or targets are hard errors — no silent fallbacks.

  | Slug | OpenAPI `(type, format)` | Default | Override |
  |------|--------------------------|---------|----------|
  | `int64` | `(integer, int64)` | `number` | `int64=bigint` |
  | `decimal` | `(number, decimal)` | `number` | `decimal=string` |
  | `date-time` | `(string, date-time)` | `Date` | `date-time=string` |
  | `date` | `(string, date)` | `Date` | `date=string` |

  ```bash
  npx @metaengine/openapi-react api.yaml ./src/app/api \
    --type-mapping int64=bigint \
    --type-mapping date-time=string
  ```

## [1.0.2] - 2026-03-30

### Features

- **TanStack Query integration** — full `useQuery`/`useMutation` hook generation with proper destructuring, options pattern for mutations with 2+ params, and hook name collision avoidance
- **Configurable base URL** — replace hardcoded base URL with configurable environment variable name via `--base-url-env`
- **Blob/binary response support** — deterministic binary detection with explicit `IsBinaryResponse` flag and conditional `apiCall` generation in http-utils
- **Response date transformation** — automatic conversion of date strings to `Date` objects in responses via `--date-transformation`
- **Custom headers, bearer auth, and timeout support** — unified on base HTTP shared options
- **Server streaming support** — `apiCallStream` import resolution and TanStack Query streaming exclusion
- **Documentation generation** — opt-in JSDoc generation via `--documentation`
- **Naming transformations** — custom file type name placeholders and file name transformations support

### Bug Fixes

- Fix optional chaining inconsistency in query params and headers
- Fix HTTP timeout generation and empty header filtering
- Fix TanStack Query hook name collision with `useQuery`/`useMutation` imports
- Fix `apiCallStream` import resolution
- Fix mutation hook parameter order destructuring
- Fix environment variable check in `api-config`
- Fix quality-check findings: env var wiring, error dedup, queryKey comma
- Wrap header values with `String()` for type safety
- Remove non-null assertion operators from HTTP call generator
- Fix naming conventions in React hooks

### Code Quality

- Align file naming with export naming (`.api.ts` instead of `.service.ts`)
- ESLint compliance improvements in generated files
- Remove unused `ServiceSuffix`, `ExportAsDefault`, `UseAsyncAwait` options

## [0.9.7] - Initial Release

Initial public release. See the [npm releases page](https://www.npmjs.com/package/@metaengine/openapi-react?activeTab=versions) for details.
