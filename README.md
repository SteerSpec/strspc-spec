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

The rules ship as archives attached to each [release](https://github.com/SteerSpec/strspc-spec/releases/latest).
The [Rules table](docs/rules.md) describes `develop`, so it can be ahead of the latest release. CI
runs realm validation, entity lint and the lifecycle diff on pull requests, except that the
lifecycle diff is skipped for the `develop` to `main` promotion pull request and for pushes to `main`
([`ci.yml`](https://github.com/SteerSpec/strspc-spec/blob/c15ce48f2120e2a1e712a8a472814c5044d212c7/.github/workflows/ci.yml)).

## Install

The rules are JSON files in this repository, and the `strspc` CLI edits and validates them. Install
the [`strspc` CLI](https://github.com/SteerSpec/strspc-CLI#installation) with Homebrew (macOS; the CLI README lists other routes):

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

The copyright holder is named in [NOTICE](NOTICE).

Licensed under Apache 2.0, see [LICENSE](LICENSE). Changes go through pull requests, as described in
[Branching and releases](docs/releases.md) and [Conventions](docs/conventions.md).

## Security

Report a vulnerability privately, never in a public issue, discussion or pull request. The
[SteerSpec security policy](https://github.com/SteerSpec/.github/blob/main/SECURITY.md) covers this
repository and gives the private reporting route.

## Community

See the [SteerSpec contributing guide](https://github.com/SteerSpec/.github/blob/main/CONTRIBUTING.md)
and the [code of conduct](https://github.com/SteerSpec/.github/blob/main/CODE_OF_CONDUCT.md).
