<div align="center">

<img src="https://steerspec.dev/assets/logo.svg" alt="SteerSpec logo" width="96" height="96">

# SteerSpec Platform

**The rules that span every SteerSpec repository, written as a SteerSpec realm, for engineers who build or maintain SteerSpec components.**

[![CI](https://img.shields.io/github/actions/workflow/status/SteerSpec/strspc-spec/ci.yml?label=CI&labelColor=161B22)](https://github.com/SteerSpec/strspc-spec/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/SteerSpec/strspc-spec?labelColor=161B22&color=161B22)](https://github.com/SteerSpec/strspc-spec/releases)
[![License](https://img.shields.io/github/license/SteerSpec/strspc-spec?labelColor=161B22&color=161B22)](LICENSE)

</div>

[SteerSpec](https://steerspec.dev) manages behavioral rules as structured JSON with deterministic
enforcement. This repository is the `dev.steerspec.platform` realm. It covers what no single
component can own: how the tiers depend on each other, the order they release in, and where rule
logic lives.

The latest release is [v1.1.0](https://github.com/SteerSpec/strspc-spec/releases/tag/v1.1.0), with
the rules attached as archives. The [Rules table](docs/rules.md) describes `develop`, so it is ahead of that
release: STRSPC-003 and STRSPC-004 are Published there and still Draft in v1.1.0. On pull requests,
CI runs realm validation, entity lint and the lifecycle diff, except that the `develop` to `main`
promotion and pushes to `main` skip the diff
([`ci.yml` at c15ce48](https://github.com/SteerSpec/strspc-spec/blob/c15ce48f2120e2a1e712a8a472814c5044d212c7/.github/workflows/ci.yml)).

## Install

The rules are JSON files in this repository, and the `strspc` CLI edits and validates them. Install
the [`strspc` CLI](https://github.com/SteerSpec/strspc-CLI#installation) with Homebrew:

```bash
brew install --cask SteerSpec/tap/strspc
```

## Where to go

| If you want to | Go to |
| -------------- | ----- |
| See how the tiers depend on each other | [Architecture](docs/architecture.md) |
| Read the platform rules | [Platform rules](docs/rules.md), or [`rules/STRSPC.json`](rules/STRSPC.json) for the normative text |
| Find the rules of a component | [Where rules live](docs/architecture.md#where-rules-live) |
| Add or change a rule, or find a file | [Working with the rules](docs/working-with-the-rules.md) |
| Understand how a change reaches a release | [Branching and releases](docs/releases.md) |
| Check commit, version and rule-state conventions | [Conventions](docs/conventions.md) |
| Read the governance and licence | [Governance](#governance) |
| Report a vulnerability | [Security](#security) |
| Contribute | [Community](#community) |

## Governance

Open source software by Consulare.

Licensed under Apache 2.0, see [LICENSE](LICENSE). Changes go through pull requests, as described in
[Branching and releases](docs/releases.md) and [Conventions](docs/conventions.md).

## Security

Report a vulnerability privately: on this repository, go to **Security**, then **Report a
vulnerability**. Do not open a public issue or pull request for it. The
[SteerSpec security policy](https://github.com/SteerSpec/.github/blob/main/SECURITY.md) covers this
repository.

## Community

See the [SteerSpec contributing guide](https://github.com/SteerSpec/.github/blob/main/CONTRIBUTING.md)
and the [code of conduct](https://github.com/SteerSpec/.github/blob/main/CODE_OF_CONDUCT.md).
