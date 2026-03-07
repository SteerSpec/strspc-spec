# SteerSpec Sync -- Rule Index

Specification version: `0.1.0-draft`

## Domain Index

| Directory | Description | File |
|-----------|-------------|------|
| `core/` | RFC-2119 keywords | [KEYWORDS.md](core/KEYWORDS.md) |
| `core/` | Rule format definitions (Entity, Rule, RuleState, RuleSet) | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| `core/` | Core sync entities (Manifest, Template, Registry, Operations) | [SYNC_CORE.md](core/SYNC_CORE.md) |
| `config/` | `steerspec-sync.yml` configuration schema | [CONFIG.md](config/CONFIG.md) |
| `versioning/` | Semver strategy, version bumps, tagging | [VERSIONING.md](versioning/VERSIONING.md) |
| `actions/` | sync-claude-md GitHub Action | [SYNC_ACTION.md](actions/SYNC_ACTION.md) |
| `actions/` | monitor-sync-status GitHub Action | [MONITOR_ACTION.md](actions/MONITOR_ACTION.md) |
| `actions/` | detect-conflicts GitHub Action | [CONFLICT_ACTION.md](actions/CONFLICT_ACTION.md) |
| `conflicts/` | Conflict detection entities and strategies | [CONFLICTS.md](conflicts/CONFLICTS.md) |

## Entity Summary

| Entity | Shortname | Rules | File |
|--------|-----------|-------|------|
| Key Word | `KWRD` | 6 | [KEYWORDS.md](core/KEYWORDS.md) |
| Entity | `ENT` | 5 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Entity Unique Identifier | `ENTUQID` | 3 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule Identifier Format | `RLIFRMT` | 7 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule | `RUL` | 8 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule State | `RULST` | 8 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule Set | `RST` | 2 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule Set Version | `RSTVRS` | 4 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Rule Set Hash | `RSTHSH` | 2 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| Note | `NTE` | 6 | [RULE_FORMAT.md](core/RULE_FORMAT.md) |
| SyncManifest | `SYNCMFST` | 8 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| Template | `TMPLT` | 8 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| TemplateVersion | `TMPLTVRS` | 5 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| ComponentRegistry | `CMPREG` | 5 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| ComponentTarget | `CMPTGT` | 6 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| SyncOperation | `SYNCOP` | 8 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| SyncPullRequest | `SYNCPR` | 6 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| DeploymentState | `DPLYST` | 5 | [SYNC_CORE.md](core/SYNC_CORE.md) |
| SyncConfig | `SYNCCFG` | 25 | [CONFIG.md](config/CONFIG.md) |
| SyncVersioning | `SYNCVRS` | 10 | [VERSIONING.md](versioning/VERSIONING.md) |
| SyncAction | `SYNCACT` | 22 | [SYNC_ACTION.md](actions/SYNC_ACTION.md) |
| MonitorAction | `MNRACT` | 17 | [MONITOR_ACTION.md](actions/MONITOR_ACTION.md) |
| ConflictAction | `CNFLACT` | 15 | [CONFLICT_ACTION.md](actions/CONFLICT_ACTION.md) |
| ConflictReport | `CNFLRPT` | 5 | [CONFLICTS.md](conflicts/CONFLICTS.md) |
| ConflictEntry | `CNFLENT` | 13 | [CONFLICTS.md](conflicts/CONFLICTS.md) |
| ConflictDetectionStrategy | `CNFLDTS` | 8 | [CONFLICTS.md](conflicts/CONFLICTS.md) |

**Total: 26 entities, 217 rules**

## Version History

| Version | Date | Description |
|---------|------|-------------|
| 0.1.0-draft | 2026-03-07 | Initial specification draft |
