# Monitor Action

## MonitorAction

- [MNRACT-001.0/D] IS a GitHub Action that monitors target repositories for synchronization drift.
- [MNRACT-002.0/D] MUST support scheduled execution via cron.
- [MNRACT-003.0/D] MUST support manual dispatch via `workflow_dispatch`.
- [MNRACT-004.0/D] MUST read the DeploymentState to determine expected template versions per repository.
- [MNRACT-005.0/D] MUST verify actual file content in target repositories against expected versions using Blake3 hash comparison.
- [MNRACT-006.0/D] MUST create a GitHub issue in the central repository for each out-of-sync target repository.
- [MNRACT-007.0/D] MUST link created issues to existing SyncPullRequests when applicable.
- [MNRACT-008.0/D] MUST NOT create duplicate issues for the same drift; an existing open issue for the same repo/template combination MUST be updated instead.
- [MNRACT-009.0/D] MUST auto-close drift issues when the drift is resolved (configurable via `auto-close`).
- [MNRACT-010.0/D] Each drift issue MUST include: repository name, template identifier, expected version, actual version (or hash), link to related PR, and time since drift was first detected.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
	- [MNRACT-005/001] Hash comparison is preferred over version string comparison because it detects manual modifications to managed files that wouldn't change the version metadata.

### Inputs

- [MNRACT-011.0/D] HAS an input `config-path` (string, default: "steerspec-sync.yml") to specify an alternate config file path.
- [MNRACT-012.0/D] HAS an input `target-filter` (string, optional) to limit monitoring to repositories matching a glob pattern.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Outputs

- [MNRACT-013.0/D] HAS an output `repos-in-sync` (integer) counting repositories with current versions.
- [MNRACT-014.0/D] HAS an output `repos-drifted` (integer) counting repositories with detected drift.
- [MNRACT-015.0/D] HAS an output `issues-created` (integer) counting new issues created.
- [MNRACT-016.0/D] HAS an output `issues-closed` (integer) counting drift issues auto-closed.
- [MNRACT-017.0/D] HAS an output `summary` (JSON string) containing a detailed monitoring report.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
