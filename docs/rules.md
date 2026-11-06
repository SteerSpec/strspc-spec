# Platform rules

[Back to the README](../README.md)

The table describes `develop`, so it is ahead of the latest release: STRSPC-003 and STRSPC-004 are
Published there and still Draft in the [v1.1.0 release](https://github.com/SteerSpec/strspc-spec/releases/tag/v1.1.0).

| ID | Topic | Rule | State |
| -- | ----- | ---- | ----- |
| STRSPC-001 | **Tier direction** | Dependencies point downward only: rules → manager → CLI / cloud | Published |
| STRSPC-002 | **Release order** | Releases land upstream first, each consumer pinning a released version | Published |
| STRSPC-003 | **Realm ownership** | Every repository that ships platform code or rules owns a realm; non-core realms depend on core | Published (Draft in v1.1.0) |
| STRSPC-004 | **CI validation** | Every realm-owning repository's CI validates its realm and lifecycle per PR | Published (Draft in v1.1.0) |
| STRSPC-005 | **Rule immutability** | Published and Implemented rules are never edited, only superseded | Implemented |
| STRSPC-006 | **Logic placement** | Rule logic lives in strspc-manager; CLI and cloud only present its results | Published |

These are summaries; [`rules/STRSPC.json`](https://github.com/SteerSpec/strspc-spec/blob/c15ce48f2120e2a1e712a8a472814c5044d212c7/rules/STRSPC.json) holds the normative text. `strspc diff` runs in [CI](https://github.com/SteerSpec/strspc-spec/blob/c15ce48f2120e2a1e712a8a472814c5044d212c7/.github/workflows/ci.yml) and checks that Published and Implemented rules are not edited (STRSPC-005). No CI check covers tier direction, release order or logic placement across repositories yet.


