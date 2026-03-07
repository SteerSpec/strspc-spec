# Sync Action

## SyncAction

- [SYNCACT-001.0/D] IS a GitHub Action that synchronizes templates from the central repository to target repositories.
- [SYNCACT-002.0/D] MUST be triggered on push to the central repository when template or manifest files change.
- [SYNCACT-003.0/D] MUST support manual dispatch via `workflow_dispatch`.
- [SYNCACT-004.0/D] MUST read the SyncManifest to determine templates and targets.
- [SYNCACT-005.0/D] MUST resolve target repositories via the GitHub API using the ComponentRegistry.
- [SYNCACT-006.0/D] For each target repository, MUST check the DeploymentState to determine if an update is needed.
- [SYNCACT-007.0/D] For each out-of-date target, MUST render the template using the configured strategy and create or update a SyncPullRequest.
- [SYNCACT-008.0/D] MUST NOT fail the entire workflow when a single target repository fails; errors MUST be isolated per repository.
- [SYNCACT-009.0/D] MUST handle GitHub API rate limiting with exponential backoff.
- [SYNCACT-010.0/D] SHOULD use concurrency groups to prevent parallel sync operations from conflicting.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
	- [SYNCACT-008/001] Per-repo error isolation prevents a single misconfigured or inaccessible repo from blocking updates to all other repos.

### Inputs

- [SYNCACT-011.0/D] HAS an input `dry-run` (boolean, default: false) which when true prevents any PRs from being created or updated.
- [SYNCACT-012.0/D] HAS an input `target-filter` (string, optional) to limit sync to repositories matching a glob pattern.
- [SYNCACT-013.0/D] HAS an input `template-filter` (string, optional) to limit sync to specific template identifiers.
- [SYNCACT-014.0/D] HAS an input `force` (boolean, default: false) which when true forces sync even if DeploymentState shows current version.
- [SYNCACT-015.0/D] HAS an input `config-path` (string, default: "steerspec-sync.yml") to specify an alternate config file path.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Outputs

- [SYNCACT-016.0/D] HAS an output `prs-created` (integer) counting the number of new PRs created.
- [SYNCACT-017.0/D] HAS an output `prs-updated` (integer) counting the number of existing PRs updated.
- [SYNCACT-018.0/D] HAS an output `repos-skipped` (integer) counting repositories that were already up to date.
- [SYNCACT-019.0/D] HAS an output `errors` (integer) counting repositories that encountered errors.
- [SYNCACT-020.0/D] HAS an output `summary` (JSON string) containing a detailed summary of the sync operation including per-repo results.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Reference Implementation

- [SYNCACT-021.0/D] A reference implementation IS RECOMMENDED in Go as a composite GitHub Action.
- [SYNCACT-022.0/D] The reference implementation SHOULD be published as a reusable GitHub Action (e.g., `steerspec/sync-action@v1`).

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
