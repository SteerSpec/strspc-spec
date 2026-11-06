# Working with the rules

[Back to the README](../README.md)

Edit entity files only through the `strspc` CLI (see [Install](../README.md#install)), so versions and hashes stay consistent:

```bash
strspc rule add STRSPC --dir rules --body "..." --added-by github:you
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
