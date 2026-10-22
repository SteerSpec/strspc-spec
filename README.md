# SteerSpec Platform

**The cross-cutting specification of the SteerSpec platform, written as a SteerSpec realm.**

## Overview

This repository is the `dev.steerspec.platform` realm. It holds the rules that span the whole
platform: how the tiers depend on each other, in which order they release, that every component
owns its rules as a realm and validates them in CI, that published rules are immutable, and where
rule logic lives.

Component specifications live with their code:

| Realm | Repository | Scope |
| ----- | ---------- | ----- |
| `dev.steerspec.core` | [strspc-rules](https://github.com/SteerSpec/strspc-rules) | rule format, entities, lifecycle, configuration |
| `dev.steerspec.manager` | [strspc-manager](https://github.com/SteerSpec/strspc-manager) | enforcement engine modules and Go conventions |
| `dev.steerspec.cli` | [strspc-CLI](https://github.com/SteerSpec/strspc-CLI) | commands, configuration, conventions |
| `dev.steerspec.sync` | [strspc-sync](https://github.com/SteerSpec/strspc-sync) | configuration sync tool and actions (166 rules) |

## Layout

```text
strspc-spec/
├── rules/
│   ├── realm.json      # dev.steerspec.platform, depends on dev.steerspec.core@1.7.0
│   ├── STRSPC.json     # the STRSPC entity
│   └── _schema/        # schemas from steerspec.dev
├── CLAUDE.md
└── LICENSE
```

## Rules

| Rule | Statement |
| ---- | --------- |
| STRSPC-001 | Dependencies point downward only: rules → manager → CLI / cloud |
| STRSPC-002 | Releases land upstream first, each consumer pinning a released version |
| STRSPC-003 | Every repository owns a realm; non-core realms depend on core |
| STRSPC-004 | Every repository's CI validates its realm and lifecycle per PR |
| STRSPC-005 | Published and Implemented rules are never edited, only superseded |
| STRSPC-006 | Rule logic lives in strspc-manager; CLI and cloud only present its results |

All rules are Draft; promotion follows the SteerSpec lifecycle, one step per pull request.

## Authoring and validation

Entity files are edited through the [`strspc` CLI](https://github.com/SteerSpec/strspc-CLI)
(`strspc rule add`, `strspc rule promote`, `strspc note add`) and validated in CI with
`strspc realm validate`, `strspc lint` and `strspc diff` via the org reusable workflow in
[SteerSpec/.github](https://github.com/SteerSpec/.github).

## License

Apache 2.0, see [LICENSE](LICENSE).
