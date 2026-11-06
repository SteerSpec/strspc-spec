# Conventions

[Back to the README](../README.md)

Rules move one state per pull request. Published and Implemented rules are never edited, they are
superseded.

```mermaid
flowchart LR
    D["<b>Draft</b><br/>text may change"]
    P["<b>Published</b><br/>text is final"]
    I["<b>Implemented</b><br/>enforced in CI"]

    D -->|one PR| P
    P -->|one PR| I

    classDef draft fill:#161B22,stroke:#6E7681,color:#E6EDF3
    classDef published fill:#21262D,stroke:#3FB950,color:#E6EDF3
    classDef implemented fill:#238636,stroke:#3FB950,color:#E6EDF3
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
