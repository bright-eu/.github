# bright-eu/.github

Org-level GitHub config and reusable workflows for Bright Energy.

## Reusable workflows

### `claude-review-go.yml` — Claude PR review for Go repos

Runs a Claude-based PR review focused on Go-specific concerns (error handling,
resource leaks, context propagation, SpiceDB patterns, modern Go). Versioned
via tags (`v1`, `v2`, ...).

**Caller example:**

```yaml
name: Claude PR Review

on:
  pull_request:
    types: [opened, synchronize, ready_for_review, reopened]
    paths:
      - "**.go"
      - "go.mod"
      - "go.sum"

jobs:
  review:
    uses: bright-eu/.github/.github/workflows/claude-review-go.yml@v1
    secrets: inherit
```

**Requirements on the consuming repo:**

- The `BRIGHT_CLAUDE_GITHUB` GitHub App must be installed.
- Org-level secrets `BRIGHT_CLAUDE_GITHUB_APP_ID`,
  `BRIGHT_CLAUDE_GITHUB_APP_PRIVATE_KEY`, `CLAUDE_CODE_OAUTH_TOKEN` must be
  available to the repo.
- Trigger `paths:` filter stays in the caller — GitHub only honors it on the
  triggering workflow.