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
strspc diff rules --base origin/develop
```

Rules start Draft (`D`) and move D → P → I one step per pull request; Published and Implemented
rules are never edited, they are superseded.

## CI

`.github/workflows/ci.yml` calls the org reusable `strspc-lint` workflow (SteerSpec/.github),
which installs the released CLI and runs realm validation, entity lint and the lifecycle diff.
It also calls `strspc-check`, which resolves the realm's dependencies and verifies cross-realm
references using `.strspc/config.yaml` (provider null, so no AI evaluation).
Commits use Conventional Commits.

## Branching and releases

`develop` is the default branch. Feature branches go to `develop` by PR. A
`chore: promote develop to main` PR, merged with a merge commit, carries the bundle to `main`.
release-please then opens a release PR that sets `realm.version` and `CHANGELOG.md`; merging it
tags and publishes, and `.github/workflows/release-please.yml` attaches the rules as archives.

Never bump `realm.version` by hand. Commit types set it: `feat(rules):` for adding or promoting a
rule (minor), `fix(rules):` for correcting a Draft rule (patch), `feat(rules)!:` for a breaking
change. `docs:`, `ci:` and `chore:` do not release. CI lints commits with commitlint, and exempts
release PRs and pushes to `main` from the lifecycle diff: they aggregate steps already validated on
the way into `develop`.

## SteerSpec org standards

Managed by [SteerSpec/.claude](https://github.com/SteerSpec/.claude) through
[strspc-sync](https://github.com/SteerSpec/strspc-sync). Change
`templates/CLAUDE.md.mustache` there; edits made here are overwritten by the
next sync.

- **Tiers.** strspc-rules (rule definitions) → strspc-manager (Go engine) →
  strspc-CLI and the other consumers of strspc-manager. Dependencies point
  downward only. Release and tag upstream before bumping a downstream `go.mod`
  or core pin.
- **Pull requests only.** Org rulesets reject direct pushes to the default
  branch and require verified commit signatures; Copilot review and
  pr-auto-approve gate the merge. Agents open PRs and never merge them.
- **Conventional commits** (`type(scope): subject`). Where a repo uses
  release-please, it derives versions and changelogs from them; other repos
  follow the release process in their own README or AGENTS.md.
- **Rules live next to the code.** Each repo owns a realm under `rules/` that
  depends on `dev.steerspec.core`. Author rules through `strspc rule …` so the
  content hashes stay valid; never hand-edit rule JSON.
- **Rule states.** Draft text may still change. Published text is final while
  the code may still violate it. Implemented is enforced in CI (`fail_on: [I]`);
  promote to Implemented only once the code complies.
- **Work tracking.** Multi-session work is tracked in beads (`bd`), not in
  markdown todo files.
- **Org agents.** Architect, Agile PM, Code Review and Release come from the
  `strspc-agents` plugin: `/plugin marketplace add SteerSpec/.claude`, then
  `/plugin install strspc-agents@steerspec`.
<!-- STEERSPEC:END:org-standards -->