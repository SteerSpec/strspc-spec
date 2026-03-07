# SteerSpec Sync

**Keeping AI configuration files consistent across your GitHub organization.**

## Problem

Teams using Claude Code maintain `CLAUDE.md` files, `.claude/agents/`, `.claude/skills/`, and other AI configuration across many repositories. As organizations grow, these files drift apart: best practices aren't propagated, conflicting instructions appear, and new repos miss critical setup. Manual synchronization doesn't scale.

## How It Works

SteerSpec Sync formalizes a pattern for managing AI configuration files across a GitHub organization:

1. **Central Source of Truth** -- A dedicated repository holds canonical templates for all managed AI configuration files (`CLAUDE.md`, `.claude/agents/*.md`, `.claude/skills/*.md`, `.claude/settings/`).

2. **Template Distribution** -- A sync action renders templates with per-repo variables and opens pull requests in target repositories. PRs are updated in place and stale version PRs are automatically closed.

3. **Monitoring** -- A scheduled action verifies that target repositories are running the expected versions, creating issues in the central repo when drift is detected.

4. **Conflict Detection** -- A scheduled action scans target repositories for version drift, manual overrides of managed sections, broken cross-references, duplicate skills, and contradictory instructions.

## Sync Scope

SteerSpec Sync manages the full Claude Code configuration surface:

- `CLAUDE.md` (project root and nested)
- `.claude/agents/*.md`
- `.claude/skills/*.md`
- `.claude/settings/`
- Any custom AI configuration files

## Configuration

Configuration is defined in `steerspec-sync.yml` at the root of the central repository. See [rules/config/CONFIG.md](rules/config/CONFIG.md) for the full schema.

## Specification

The full specification is written using the SteerSpec rule format (`[ENTITY-###.VERSION/STATE]`). See [rules/README.md](rules/README.md) for the domain index and entity summary.

### Rule Format Reference

This specification is self-contained and includes its own copies of the core SteerSpec rule format definitions:

- [rules/core/KEYWORDS.md](rules/core/KEYWORDS.md) -- RFC-2119 keywords
- [rules/core/RULE_FORMAT.md](rules/core/RULE_FORMAT.md) -- Rule identifier format, entities, states

### Domain-Specific Rules

- [rules/core/SYNC_CORE.md](rules/core/SYNC_CORE.md) -- Core sync entities
- [rules/config/CONFIG.md](rules/config/CONFIG.md) -- Configuration schema
- [rules/versioning/VERSIONING.md](rules/versioning/VERSIONING.md) -- Versioning strategy
- [rules/actions/SYNC_ACTION.md](rules/actions/SYNC_ACTION.md) -- Sync GitHub Action
- [rules/actions/MONITOR_ACTION.md](rules/actions/MONITOR_ACTION.md) -- Monitor GitHub Action
- [rules/actions/CONFLICT_ACTION.md](rules/actions/CONFLICT_ACTION.md) -- Conflict detection GitHub Action
- [rules/conflicts/CONFLICTS.md](rules/conflicts/CONFLICTS.md) -- Conflict detection entities

## Reference Implementation

A reference implementation is RECOMMENDED in Go, packaged as a composite GitHub Action. See the action specs under [rules/actions/](rules/actions/) for input/output contracts.

## Version

Current specification version: `0.1.0-draft`

## License

This specification is open source. See [LICENSE](LICENSE) for details.
