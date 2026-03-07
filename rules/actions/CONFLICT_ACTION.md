# Conflict Detection Action

## ConflictAction

- [CNFLACT-001.0/D] IS a GitHub Action that detects conflicts and inconsistencies across target repositories.
- [CNFLACT-002.0/D] MUST support scheduled execution via cron.
- [CNFLACT-003.0/D] MUST support manual dispatch via `workflow_dispatch`.
- [CNFLACT-004.0/D] MUST scan all target repositories for `CLAUDE.md` and `.claude/` tree contents.
- [CNFLACT-005.0/D] MUST produce a ConflictReport as output.
- [CNFLACT-006.0/D] MUST create GitHub issues in the central repository for conflicts at or above the configured severity threshold.
- [CNFLACT-007.0/D] MUST NOT create duplicate issues for the same conflict; existing open issues MUST be updated instead.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Inputs

- [CNFLACT-008.0/D] HAS an input `config-path` (string, default: "steerspec-sync.yml") to specify an alternate config file path.
- [CNFLACT-009.0/D] HAS an input `target-filter` (string, optional) to limit scanning to repositories matching a glob pattern.
- [CNFLACT-010.0/D] HAS an input `tiers` (string, optional) to override the configured detection tiers (e.g., "1,2,3").

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Outputs

- [CNFLACT-011.0/D] HAS an output `conflicts-found` (integer) counting total conflicts detected.
- [CNFLACT-012.0/D] HAS an output `critical-count` (integer) counting critical-severity conflicts.
- [CNFLACT-013.0/D] HAS an output `warning-count` (integer) counting warning-severity conflicts.
- [CNFLACT-014.0/D] HAS an output `info-count` (integer) counting info-severity conflicts.
- [CNFLACT-015.0/D] HAS an output `report` (JSON string) containing the full ConflictReport.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
