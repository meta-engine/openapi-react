# Changelog

All notable changes to `@metaengine/openapi-react` will be documented in this file.

## [1.1.0] - 2026-05-04

### Changed

- **Generated client now exports a `createClient()` factory** — services no longer pull config from a `http-utils` module. Each generated service method takes an `apiClient: ApiClient` as its first argument, obtained from `createClient(config)`. The previous `http-utils.ts` and `api-config.ts` files are no longer emitted; consumers need to migrate their imports and call sites to the new factory shape
- **`--tanstack-query` now wires through a React Context** — the generated `client.ts` exports `ApiClientContext`, `ApiClientProvider`, and `useApiClient()`. Generated hooks resolve their client via `useApiClient()`, so consumers must wrap their app once with `<ApiClientProvider value={createClient({ ... })}>`
- **Inline enums consolidated by default** — duplicate inline enum definitions are deduplicated and emitted as a single named type instead of repeating identical literal unions at each usage site
- **Inline array-of-object items emitted as named types** — array properties whose items are inline object schemas now produce a named interface, e.g. `users: User[]` instead of `users: { id: number; ... }[]`
- **Discriminator mapping literals pinned on union subtypes** — each subtype carries its discriminator value as a literal type so TypeScript narrows correctly when switching on the discriminator
- **`@deprecated` JSDoc tags emitted regardless of `--documentation`** — deprecated types and properties always get the `@deprecated` tag so IDE tooling can warn even when JSDoc generation is otherwise off

### Bug Fixes

- **Network errors during fetch wrapped as `HttpError(0, ...)`** — failed fetches no longer bubble a raw `TypeError` to consumers; all network failures normalize into the package's `HttpError` shape with status code `0`
- **Fixed: `--service-suffix` doubles the `Api` suffix in barrel aliases** — the generated `index.ts` no longer produces aliases like `UsersApiApi` when `--service-suffix Api` is in effect
- **Fixed: blank line after JSDoc `*/` in service files** — the trailing blank line between a comment block and the function it documents is removed
- **Fixed: multi-line JSDoc continuation prefixes** — wrapped JSDoc lines correctly start with ` * `, eliminating stray indentation
- **Fixed: inline synthetic type names capped at 100 characters** — deeply nested inline schemas no longer produce overly long generated type names
- **Fixed: array-branch FormData object serialization** — when a multipart field is an array of objects, each entry is serialized correctly instead of being collapsed
- **Fixed: discriminated union members dropping wire annotations** — inline `oneOf` members combined with a `discriminator` keep their wire-format annotations in the generated TypeScript

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
