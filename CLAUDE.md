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

<!-- STEERSPEC:BEGIN:org-standards -->
## SteerSpec org standards

Managed by [SteerSpec/.claude](https://github.com/SteerSpec/.claude) through
[strspc-sync](https://github.com/SteerSpec/strspc-sync). Change
`templates/CLAUDE.md.mustache` there; edits made here are overwritten by the
next sync.

- **Tiers.** strspc-rules (rule definitions) → strspc-manager (Go engine) →
  strspc-CLI. Dependencies point downward only. Release and tag upstream before
  bumping a downstream `go.mod` or core pin.
- **Pull requests only.** Org rulesets reject direct pushes to the default
  branch and require verified commit signatures; Copilot review and
  pr-auto-approve gate the merge. Agents open PRs and never merge them.
- **Conventional commits** (`type(scope): subject`). release-please derives
  versions and changelogs from them.
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