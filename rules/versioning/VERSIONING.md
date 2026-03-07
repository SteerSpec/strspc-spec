# Versioning

## SyncVersioning

- [SYNCVRS-001.0/D] All template versions MUST use semantic versioning (MAJOR.MINOR.PATCH).
- [SYNCVRS-002.0/D] A MAJOR version bump IS REQUIRED for breaking changes: rendering strategy change, required variable addition or removal, or destination path change.
- [SYNCVRS-003.0/D] A MINOR version bump IS REQUIRED for new optional content or new optional variables.
- [SYNCVRS-004.0/D] A PATCH version bump IS REQUIRED for content corrections, typo fixes, or formatting changes.
- [SYNCVRS-005.0/D] Version bumps MUST be determined by the template author and MUST NOT be auto-inferred.
- [SYNCVRS-006.0/D] Each template version MUST be tracked via a git tag in the format `tmpl/<template-id>/v<version>`.
- [SYNCVRS-007.0/D] The specification version MUST be tracked in a `VERSION` file at the repository root.
- [SYNCVRS-008.0/D] Per-template version metadata MUST be stored in `.steerspec/versions.json` in the central repository.
- [SYNCVRS-009.0/D] Each entry in `versions.json` MUST contain: version string, Blake3 hash, UTC timestamp, and author.
- [SYNCVRS-010.0/D] Version entries in `versions.json` MUST be append-only; previous entries MUST NOT be modified or deleted.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
	- [SYNCVRS-002/001] Breaking changes force target repos to potentially update their local sections or variable definitions, warranting a major bump.
	- [SYNCVRS-005/001] Auto-inferring version bumps from diffs is fragile and cannot reliably distinguish between content corrections (patch) and meaningful additions (minor).

### Example `versions.json`

```json
{
  "claude-md": [
    {
      "version": "1.0.0",
      "hash": "b3_a1b2c3d4e5f6...",
      "timestamp": "2026-01-15T10:30:00Z",
      "author": "alice"
    },
    {
      "version": "1.1.0",
      "hash": "b3_f6e5d4c3b2a1...",
      "timestamp": "2026-02-20T14:00:00Z",
      "author": "bob"
    }
  ],
  "agent-code-review": [
    {
      "version": "1.0.0",
      "hash": "b3_1a2b3c4d5e6f...",
      "timestamp": "2026-01-15T10:30:00Z",
      "author": "alice"
    }
  ]
}
```
