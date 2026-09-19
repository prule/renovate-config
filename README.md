# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for `prule` projects, so dependency policy lives in one place instead of drifting across repos.

## Usage

A Node/TypeScript project:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>prule/renovate-config:node"]
}
```

A Java/Gradle project:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>prule/renovate-config:java"]
}
```

Anything else can extend the baseline directly with `github>prule/renovate-config`.

Repo-specific rules still belong in that repo's own `renovate.json` — add them to `packageRules` after the `extends`, and they take precedence over the preset.

## What's here

| Preset | Extends | Adds |
| --- | --- | --- |
| `default.json` | `config:recommended`, `config:best-practices` | UTC weekend schedule, vulnerability alerts at any time, devcontainer/Dockerfile digest automerge, GitHub Actions grouping (minor/patch automerged, majors held) |
| `node.json` | `default`, `group:vite` | `rangeStrategy: pin`, devDependencies minor/patch automerge, Serenity/JS, WebdriverIO and testing-utility groups |
| `java.json` | `default` | PR limits, Spring grouping, test-dependency automerge, Gradle wrapper automerge, Mockito/Byte Buddy and JDK majors behind dashboard approval |

## Notes

React and Vite grouping come from Renovate's own presets rather than hand-written rules: `group:react` and `monorepo:react` are already pulled in by `config:recommended`, and `group:vite` is added explicitly by `node.json` because it is *not* part of `group:recommended`. Prefer an upstream preset over a custom `matchPackageNames` rule where one exists.

## Validating a change

```bash
npx --package renovate renovate-config-validator
```

Run it with no arguments from a directory containing the file as `renovate.json` — passing a filename validates it as *global* config, which is a different and weaker schema.
