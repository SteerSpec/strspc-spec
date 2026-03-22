# SteerSpec Platform

**The specification realm for the SteerSpec platform, defined using SteerSpec's own rule format.**

## Overview

This repository is the `dev.steerspec.platform` realm -- the formal specification for all SteerSpec components. It dogfoods the SteerSpec toolchain: entities are JSON files validated by `strspc-CLI` and `strspc-manager`, organized as a realm with sub-realms per component.

Core rule format definitions (keywords, rule format, entity format) live in `dev.steerspec.core` ([strspc-rules](https://github.com/steerspec/strspc-rules)) and are declared as a dependency.

## Realm Structure

```
strspc-spec/
├── realm.json          # dev.steerspec.platform
├── _schema/            # Validation schemas (from dev.steerspec.core)
├── STRSPC.json         # SteerSpec Platform entity
├── cli/                # dev.steerspec.platform.cli (stub)
├── manager/            # dev.steerspec.platform.manager (stub)
├── sync/               # dev.steerspec.platform.sync (166 rules)
├── cloud/              # dev.steerspec.platform.cloud (stub)
└── VERSION
```

## Sub-Realms

### sync/ -- SteerSpec Sync

Specification for keeping AI configuration files consistent across a GitHub organization. 166 rules across 16 entities.

| File | Entity | Rules | Description |
|------|--------|-------|-------------|
| `SYNCMNFST.json` | SYNCMFST | 8 | Central sync configuration |
| `TMPL.json` | TMPLT + TMPLTVRS | 8 + 5 | Templates and versioned snapshots |
| `COMPREG.json` | CMPREG + CMPTGT | 5 + 6 | Target repository registry |
| `SYNCOP.json` | SYNCOP + SYNCPR | 8 + 6 | Sync operations and pull requests |
| `DEPLST.json` | DPLYST | 5 | Deployment state tracking |
| `SYNCFG.json` | SYNCCFG | 25 | Configuration schema (steerspec-sync.yml) |
| `SYNCVER.json` | SYNCVRS | 10 | Versioning strategy |
| `SYNCACT.json` | SYNCACT | 22 | Sync GitHub Action |
| `MONACT.json` | MNRACT | 17 | Monitor GitHub Action |
| `CNFLACT.json` | CNFLACT | 15 | Conflict detection GitHub Action |
| `CNFL.json` | CNFLRPT + CNFLENT + CNFLDTS | 5 + 13 + 8 | Conflict detection and resolution |

### cli/ -- SteerSpec CLI

Specification for the command-line tool. Populates as features are built in [strspc-CLI](https://github.com/steerspec/strspc-CLI).

### manager/ -- SteerSpec Manager

Specification for the core enforcement engine. Populates as features are built in [strspc-manager](https://github.com/steerspec/strspc-manager).

### cloud/ -- SteerSpec Cloud

Specification for the SaaS service. Populates as features are built in [strspc-cloud](https://github.com/steerspec/strspc-cloud).

## Dependencies

This realm depends on `dev.steerspec.core` (v0.1.0) for core rule format definitions including RFC-2119 keywords, rule identifiers, entity format, and realm structure.

## Dogfooding

This repo is both the specification and the test bed for SteerSpec tooling:

- **Validate**: `strspc realm validate` checks realm structure and entity schemas
- **Render**: `strspc render sync/SYNCMNFST.json` renders entities to readable markdown
- **Lint**: `strspc lint sync/` validates entity files against schemas (coming soon)

As new features land in `-CLI` and `-manager`, they are specified here first and then validated by the very tools being built.

## Version

Current specification version: `0.1.0-draft`

## License

This specification is open source. See [LICENSE](LICENSE) for details.
