# Branching and releases

[Back to the README](../README.md)

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
