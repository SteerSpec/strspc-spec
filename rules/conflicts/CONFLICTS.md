# Conflict Detection

## ConflictReport

- [CNFLRPT-001.0/D] IS the result of a conflict detection scan across target repositories.
- [CNFLRPT-002.0/D] HAS a UUID.
- [CNFLRPT-003.0/D] HAS a UTC timestamp.
- [CNFLRPT-004.0/D] HAS zero or more ConflictEntries.
- [CNFLRPT-005.0/D] HAS a severity summary containing counts per severity level (critical, warning, info).

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

## ConflictEntry

- [CNFLENT-001.0/D] IS a single detected conflict or inconsistency.
- [CNFLENT-002.0/D] HAS a severity which MUST be one of: "critical", "warning", "info".
- [CNFLENT-003.0/D] HAS a type which MUST be one of: "version-drift", "manual-override", "cross-reference-broken", "duplicate-skill", "contradictory-instruction", "unmanaged-file".
- [CNFLENT-004.0/D] HAS a repository identifier in `owner/repo` format.
- [CNFLENT-005.0/D] HAS a file path relative to the repository root.
- [CNFLENT-006.0/D] HAS a human-readable description of the conflict.
- [CNFLENT-007.0/D] MAY have a suggested resolution.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Conflict Types

- [CNFLENT-008.0/D] `version-drift` IS a managed file at a version different from the expected version in DeploymentState; severity: critical.
- [CNFLENT-009.0/D] `manual-override` IS a modification to managed sections (between STEERSPEC markers) outside of the sync process; severity: warning.
- [CNFLENT-010.0/D] `cross-reference-broken` IS a reference in CLAUDE.md to a skill or agent file that does not exist in the repository; severity: critical.
- [CNFLENT-011.0/D] `duplicate-skill` IS the same skill defined with different content across repositories that should be identical; severity: warning.
- [CNFLENT-012.0/D] `contradictory-instruction` IS conflicting directives within the same repository's AI configuration files; severity: warning.
- [CNFLENT-013.0/D] `unmanaged-file` IS an AI configuration file (CLAUDE.md or `.claude/**`) not tracked in the SyncManifest; severity: info.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
	- [CNFLENT-012/001] Contradictory instructions require semantic analysis and are only reliably detectable at Tier 3 (LLM-assisted).

## ConflictDetectionStrategy

- [CNFLDTS-001.0/D] IS a tiered approach to detecting conflicts with increasing sophistication and cost.
- [CNFLDTS-002.0/D] Tier 1 (deterministic) MUST include: version drift detection via hash comparison, missing file references, and unmanaged file detection.
- [CNFLDTS-003.0/D] Tier 2 (heuristic) MUST include: duplicate skill detection via filename and content hash comparison, and section overlap detection between managed files.
- [CNFLDTS-004.0/D] Tier 3 (LLM-assisted) MAY include: semantic contradiction detection and redundant content identification.
- [CNFLDTS-005.0/D] Tier 3 MUST be explicitly opted into via configuration and MUST NOT be enabled by default.
- [CNFLDTS-006.0/D] Tier 3 SHOULD document estimated cost per scan based on repository count and file sizes.
- [CNFLDTS-007.0/D] Each tier MUST be independently enableable.
- [CNFLDTS-008.0/D] Results from all enabled tiers MUST be combined into a single ConflictReport.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
	- [CNFLDTS-004/001] LLM-assisted detection is non-deterministic and expensive. It is best suited for periodic deep audits rather than continuous monitoring.
	- [CNFLDTS-002/001] Deterministic checks are fast, cheap, and reliable. They should catch the majority of real-world conflicts.
