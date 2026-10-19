# CLAUDE.md

Guidance for Claude Code in this repository.

## Project

strspc-spec is the `dev.steerspec.platform` realm: the cross-cutting rules of the SteerSpec
platform (tier dependency direction, release order, realm ownership, CI validation, rule
immutability, where logic lives). Component rules live in each component's own repository:
`strspc-rules/rules/core` (`dev.steerspec.core`), `strspc-manager/rules`, `strspc-CLI/rules`,
`strspc-sync/rules`.

## Layout

- `rules/realm.json` — manifest; `realm.version` is the specification version
- `rules/STRSPC.json` — the single entity, `STRSPC`
- `rules/_schema/` — schemas fetched from steerspec.dev by `strspc realm init`

## Authoring

Edit entity files only through the CLI so versions and hashes stay consistent:

```bash
strspc rule add STRSPC --dir rules --body "..." --added-by @you
strspc rule promote STRSPC-001 --dir rules
strspc note add ...
strspc realm validate rules --recursive --strict
strspc lint rules --strict
strspc diff rules --base origin/main
```

Rules start Draft (`D`) and move D → P → I one step per pull request; Published and Implemented
rules are never edited, they are superseded.

## CI

`.github/workflows/ci.yml` calls the org reusable `strspc-lint` workflow (SteerSpec/.github),
which installs the released CLI and runs realm validation, entity lint and the lifecycle diff.
Commits use Conventional Commits.
