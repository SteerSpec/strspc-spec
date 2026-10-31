<div align="center">

<img src="https://steerspec.dev/assets/logo.svg" alt="SteerSpec logo" width="96" height="96">

# SteerSpec Platform

**The rules that span every SteerSpec repository, written as a SteerSpec realm.**

[![CI](https://github.com/SteerSpec/strspc-spec/actions/workflows/ci.yml/badge.svg)](https://github.com/SteerSpec/strspc-spec/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/SteerSpec/strspc-spec)](https://github.com/SteerSpec/strspc-spec/releases)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

[Architecture](#architecture) · [Rules](#rules) · [Where rules live](#where-rules-live) · [Working with the rules](#working-with-the-rules) · [Layout](#layout) ·
[Releases](#branching-and-releases) · [Conventions](#conventions) · [Contributing](#contributing)

</div>

[SteerSpec](https://steerspec.dev) manages behavioral rules as structured JSON with deterministic
enforcement. This repository is the `dev.steerspec.platform` realm. It covers what no single
component can own: how the tiers depend on each other, the order they release in, and where rule
logic lives.

## Architecture

```mermaid
flowchart LR
    subgraph platform["Governed by dev.steerspec.platform"]
        direction LR
        R["<b>strspc-rules</b><br/>rule format and schemas"]
        M["<b>strspc-manager</b><br/>enforcement engine"]
        C["<b>strspc-CLI</b><br/>command line"]
        K["<b>strspc-cloud</b><br/>hosted service, planned"]

        R -->|consumed by| M
        M -->|imported by| C
        M -.->|imported by| K
    end

    classDef shipped fill:#1f6feb,stroke:#1f6feb,color:#ffffff
    classDef planned fill:#6e7781,stroke:#6e7781,color:#ffffff
    class R,M,C shipped
    class K planned
    style platform fill:transparent,stroke:#6e7781,stroke-dasharray:5 5
```

Dependencies point one way, and releases land in the same order: rules, then manager, then its
consumers.

## Rules

| ID | Topic | Rule | State |
| -- | ----- | ---- | ----- |
| STRSPC-001 | **Tier direction** | Dependencies point downward only: rules → manager → CLI / cloud | Published |
| STRSPC-002 | **Release order** | Releases land upstream first, each consumer pinning a released version | Published |
| STRSPC-003 | **Realm ownership** | Every repository that ships platform code or rules owns a realm; non-core realms depend on core | Draft |
| STRSPC-004 | **CI validation** | Every realm-owning repository's CI validates its realm and lifecycle per PR | Draft |
| STRSPC-005 | **Rule immutability** | Published and Implemented rules are never edited, only superseded | Implemented |
| STRSPC-006 | **Logic placement** | Rule logic lives in strspc-manager; CLI and cloud only present its results | Published |

These are summaries; [`rules/STRSPC.json`](rules/STRSPC.json) holds the normative text. `strspc diff`
enforces STRSPC-005 in CI, and CI doesn't enforce the others yet.

## Where rules live

Each component's rules live with its code:

| Realm | Repository | Scope |
| ----- | ---------- | ----- |
| `dev.steerspec.platform` | this repository | cross-cutting platform rules |
| `dev.steerspec.core` | [strspc-rules](https://github.com/SteerSpec/strspc-rules) | rule format, entities, lifecycle, configuration |
| `dev.steerspec.manager` | [strspc-manager](https://github.com/SteerSpec/strspc-manager) | enforcement engine modules and Go conventions |
| `dev.steerspec.cli` | [strspc-CLI](https://github.com/SteerSpec/strspc-CLI) | commands, configuration, conventions |
| `dev.steerspec.sync` | [strspc-sync](https://github.com/SteerSpec/strspc-sync) | configuration sync tool and actions |
| `dev.steerspec.prreview` | [strspc-pr-review](https://github.com/SteerSpec/strspc-pr-review) | automated pull request approval |
| `dev.steerspec.cloud` | strspc-cloud (private) | hosted service, planned |

## Working with the rules

Install the [`strspc` CLI](https://github.com/SteerSpec/strspc-CLI):

```bash
brew install --cask SteerSpec/tap/strspc
```

Edit entity files only through it, so versions and hashes stay consistent:

```bash
strspc rule add STRSPC --dir rules --body "..." --added-by @you
strspc rule promote STRSPC-001 --dir rules
```

Run the same validation as CI before opening a pull request:

```bash
strspc realm validate rules --recursive --strict
strspc lint rules --strict
strspc diff rules --base origin/develop
strspc check --static-only
```

CI runs these through the reusable workflows in
[SteerSpec/.github](https://github.com/SteerSpec/.github). `strspc realm validate` compares the realm
with its pinned dependencies; only `strspc check` verifies cross-realm references such as `inherits`
and `overrides`.

## Layout

```text
strspc-spec/
├── rules/
│   ├── realm.json      # the dev.steerspec.platform manifest and its core pin
│   ├── STRSPC.json     # the STRSPC entity
│   └── _schema/        # schemas from steerspec.dev
├── .strspc/
│   └── config.yaml     # strspc check configuration
├── .github/workflows/  # CI, release-please and pull request approval
├── release-please-config.json
├── .release-please-manifest.json
├── CLAUDE.md
└── LICENSE
```

## Branching and releases

`develop` is the default branch and collects the changes for the next release; `main` only moves
when a release is prepared:

1. Pull requests target `develop` and are squashed, so the title becomes the commit release-please
   reads.
2. A `chore: promote develop to main` pull request carries the bundle to `main`. Merge it with a
   **merge commit**: release-please reads the individual commits, and a squash would hide them.
3. [release-please](https://github.com/googleapis/release-please) opens a release pull request
   against `main` that sets `realm.version` and updates `CHANGELOG.md`.
4. Merging it tags `v<realm.version>` and publishes a GitHub release with the rules as archives.

> [!IMPORTANT]
> Promote `develop` before merging a release pull request. release-please stops reading at the last
> release commit, by date, so a `feat:` or `fix:` left on `develop` while a release merges is dropped
> from the next release.

Title types set the version, so rule changes need the right one:

| Change | Title | Version |
| ------ | ----- | ------- |
| Add or promote a rule | `feat(rules): ...` | minor |
| Correct a Draft rule | `fix(rules): ...` | patch |
| Breaking change | `feat(rules)!: ...` | major |
| Docs, CI, chores, core pins | `docs:`, `ci:`, `chore:` | no release |

`main` holds the released `realm.version`. `develop` is not back-merged, so its copy stays behind.

The `develop` to `main` promotion pull request and pushes to `main` skip the `strspc diff` lifecycle
check: they aggregate transitions that were validated one pull request at a time on the way into
`develop`. Any other pull request into `main` is checked.

## Conventions

Rules move one state per pull request. Published and Implemented rules are never edited, they are
superseded.

```mermaid
flowchart LR
    D["<b>Draft</b><br/>text may change"]
    P["<b>Published</b><br/>text is final"]
    I["<b>Implemented</b><br/>enforced in CI"]

    D -->|one PR| P
    P -->|one PR| I

    classDef draft fill:#6e7781,stroke:#6e7781,color:#ffffff
    classDef published fill:#1f6feb,stroke:#1f6feb,color:#ffffff
    classDef implemented fill:#1a7f37,stroke:#1a7f37,color:#ffffff
    class D draft
    class P published
    class I implemented
```

A rule can also leave this path: a dropped Draft becomes Abandoned, a rule that must be removed is
Retired, and one no longer in force is Terminated.

| Topic | Convention |
| ----- | ---------- |
| **Commits** | [Conventional Commits](https://www.conventionalcommits.org/), as `type(scope): subject`. CI checks pull request titles |
| **Versions** | `realm.version` in `rules/realm.json` is the specification version, in [SemVer](https://semver.org/) form. release-please sets it, and `v<realm.version>` is the release tag |
| **Dependencies** | The core realm is pinned to one exact released version |

## Contributing

See the [SteerSpec contributing guide](https://github.com/SteerSpec/.github/blob/main/CONTRIBUTING.md).

## License

Apache 2.0, see [LICENSE](LICENSE).
