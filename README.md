# @yauseyea/oxlint-config

Shared [oxlint](https://oxc.rs) configuration for TypeScript and Node.js projects.

One config, one place to maintain, consistent linting rules across every project.

## Installation

```bash
npm install --save-dev @yauseyea/oxlint-config oxlint
```

## Usage

Create (or update) `.oxlintrc.json` in your project root:

```json
{
  "$schema": "./node_modules/oxlint/configuration_schema.json",
  "extends": ["./node_modules/@yauseyea/oxlint-config/.oxlintrc.json"]
}
```

That's it for projects that need nothing beyond the shared baseline. If a project needs
something extra, add it alongside `extends` to your local settings are merged on top of
the shared config, they don't replace it.

**Node.js backend project:**

```json
{
  "$schema": "./node_modules/oxlint/configuration_schema.json",
  "extends": ["./node_modules/@yauseyea/oxlint-config/.oxlintrc.json"],
  "env": {
    "node": true
  }
}
```

**Angular frontend project:**

```json
{
  "$schema": "./node_modules/oxlint/configuration_schema.json",
  "extends": ["./node_modules/@yauseyea/oxlint-config/.oxlintrc.json"],
  "env": {
    "browser": true
  },
  "ignorePatterns": [".angular/**", "e2e/**"]
}
```

### How merging works

- `env` / `globals`: additive. Local entries add to the shared ones, they don't remove them.
- `rules`: merged key by key. A rule you set locally overrides only that rule; everything
  else from the shared config is untouched.
- `overrides`: arrays append. Your shared TS/test overrides still apply alongside any
  local ones you add.
- `plugins`: the one exception. Setting `plugins` locally **overwrites** the full list
  rather than merging, so only redeclare it if you deliberately want to change which
  plugins are active (and then list all of them, not just the new one).

## What's included

- TypeScript-aware correctness rules, with core ESLint rules disabled where the
  TypeScript compiler already covers the same ground (e.g. `no-undef`, `constructor-super`).
- `unicorn` and `import` plugins enabled.
- Sensible defaults for tests (`src/__tests__/**`): real bugs still get caught, but
  noisy rules like `no-console` and `no-explicit-any` are relaxed.

See [`.oxlintrc.json`](./.oxlintrc.json) for the full rule set.

## Releasing a new version

Releases are manual and deliberate, nothing is published automatically on every commit.

```bash
npm version patch   # or minor / major
git push --follow-tags
```

Pushing the tag triggers the CI pipeline, which publishes the new version to npm.
Ordinary branch pushes and merge requests do not trigger any pipeline.

## License

MIT — see [LICENSE](./LICENSE)