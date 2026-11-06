# Platform rules

[Back to the README](../README.md)

| ID | Topic | Rule | State |
| -- | ----- | ---- | ----- |
| STRSPC-001 | **Tier direction** | Dependencies point downward only: rules → manager → CLI / cloud | Published |
| STRSPC-002 | **Release order** | Releases land upstream first, each consumer pinning a released version | Published |
| STRSPC-003 | **Realm ownership** | Every repository that ships platform code or rules owns a realm; non-core realms depend on core | Published |
| STRSPC-004 | **CI validation** | Every realm-owning repository's CI validates its realm and lifecycle per PR | Published |
| STRSPC-005 | **Rule immutability** | Published and Implemented rules are never edited, only superseded | Implemented |
| STRSPC-006 | **Logic placement** | Rule logic lives in strspc-manager; CLI and cloud only present its results | Published |

These are summaries; [`rules/STRSPC.json`](https://github.com/SteerSpec/strspc-spec/blob/c15ce48f2120e2a1e712a8a472814c5044d212c7/rules/STRSPC.json) holds the normative text. `strspc diff`
enforces STRSPC-005 in CI, and CI doesn't enforce the others yet.


The table describes `develop`, so it is ahead of the [v1.1.0](https://github.com/SteerSpec/strspc-spec/releases/tag/v1.1.0) release: STRSPC-003 and STRSPC-004 are Published there and still Draft in v1.1.0.
