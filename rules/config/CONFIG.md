# Configuration Schema

## SyncConfig

- [SYNCCFG-001.0/D] IS the schema for the `steerspec-sync.yml` configuration file.
- [SYNCCFG-002.0/D] MUST be expressed as a YAML file named `steerspec-sync.yml` at the repository root.
- [SYNCCFG-003.0/D] MUST contain a `version` field specifying the config schema version in semver format.
- [SYNCCFG-004.0/D] MUST contain an `auth` section specifying the authentication method.
- [SYNCCFG-005.0/D] MAY contain a `variables` section for global template variables.
- [SYNCCFG-006.0/D] MUST contain a `templates` section defining one or more templates.
- [SYNCCFG-007.0/D] MUST contain a `targets` section defining the ComponentRegistry.
- [SYNCCFG-008.0/D] MAY contain a `sync` section for sync behavior configuration.
- [SYNCCFG-009.0/D] MAY contain a `monitor` section for monitoring behavior configuration.
- [SYNCCFG-010.0/D] MAY contain a `conflicts` section for conflict detection configuration.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Auth Configuration

- [SYNCCFG-011.0/D] The `auth` section MUST specify a `method` which MUST be one of: "github-app", "pat", "github-token".
- [SYNCCFG-012.0/D] The "github-app" method IS RECOMMENDED for production use due to better rate limits and audit trails.
- [SYNCCFG-013.0/D] When using "github-app", the section MUST include `app-id` and `private-key` fields (MAY reference secrets).
- [SYNCCFG-014.0/D] When using "pat" or "github-token", the section MUST include a `token` field (MAY reference secrets).

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Sync Behavior Configuration

- [SYNCCFG-015.0/D] The `sync` section MAY define `concurrency` as an integer (default: 5) controlling parallel PR creation.
- [SYNCCFG-016.0/D] The `sync` section MAY define `pr-label` as a string (default: "steerspec-sync").
- [SYNCCFG-017.0/D] The `sync` section MAY define `close-stale-prs` as a boolean (default: true).
- [SYNCCFG-018.0/D] The `sync` section MAY define `dry-run` as a boolean (default: false).

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Monitor Behavior Configuration

- [SYNCCFG-019.0/D] The `monitor` section MAY define `schedule` as a cron expression (default: "0 8 * * 1" -- weekly Monday 08:00 UTC).
- [SYNCCFG-020.0/D] The `monitor` section MAY define `issue-label` as a string (default: "steerspec-drift").
- [SYNCCFG-021.0/D] The `monitor` section MAY define `auto-close` as a boolean (default: true) controlling whether resolved drift issues are auto-closed.

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

### Conflict Detection Configuration

- [SYNCCFG-022.0/D] The `conflicts` section MAY define `schedule` as a cron expression (default: "0 9 * * 1" -- weekly Monday 09:00 UTC).
- [SYNCCFG-023.0/D] The `conflicts` section MAY define `tiers` as a list of enabled detection tiers (default: [1, 2]).
- [SYNCCFG-024.0/D] Tier 3 (LLM-assisted) MUST be explicitly opted into and MUST NOT be enabled by default.
- [SYNCCFG-025.0/D] The `conflicts` section MAY define `issue-label` as a string (default: "steerspec-conflict").

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:

## Example Configuration

```yaml
version: "1.0.0"

auth:
  method: github-app
  app-id: ${{ secrets.STEERSPEC_APP_ID }}
  private-key: ${{ secrets.STEERSPEC_PRIVATE_KEY }}

variables:
  org_name: "acme-corp"
  default_model: "claude-sonnet-4-6"
  coding_standards_url: "https://acme-corp.dev/standards"

templates:
  - id: claude-md
    type: claude-md
    source: templates/CLAUDE.md.mustache
    destination: CLAUDE.md
    strategy: marker
    variables:
      managed_sections:
        - project-standards
        - coding-conventions

  - id: agent-code-review
    type: agent
    source: templates/agents/code-review.md
    destination: .claude/agents/code-review.md
    strategy: full-replace

  - id: skill-commit
    type: skill
    source: templates/skills/commit.md
    destination: .claude/skills/commit.md
    strategy: full-replace

  - id: claude-settings
    type: config
    source: templates/settings.json.mustache
    destination: .claude/settings/settings.json
    strategy: mustache

targets:
  include:
    - "acme-corp/*"
  exclude:
    - "acme-corp/archived-*"
    - "acme-corp/.github"
  topics:
    - claude-managed
  overrides:
    - repo: acme-corp/api-server
      default_branch: develop
      variables:
        project_type: "api"
      exclude_templates:
        - claude-settings
    - repo: acme-corp/frontend
      variables:
        project_type: "frontend"
        default_model: "claude-sonnet-4-6"

sync:
  concurrency: 5
  pr-label: steerspec-sync
  close-stale-prs: true

monitor:
  schedule: "0 8 * * 1"
  issue-label: steerspec-drift
  auto-close: true

conflicts:
  schedule: "0 9 * * 1"
  tiers: [1, 2]
  issue-label: steerspec-conflict
```

Version(s):
	- 0.1.0-draft (2026-03-07): initial draft

Notes:
