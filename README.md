# AI Review Cycle Playground

Test environment for the AI code review system from postscript-marketing-ai PR #1191.

## Workflows

- **ai-review.yml** - Two-step AI reviewer + judge pipeline (triggered by `ready-for-review` label)
- **review-feedback.yml** - Feedback capture on PR close
- **policy-review.yml** - Weekly scheduled policy review

## Labels

| Label | Purpose |
|-------|---------|
| `ready-for-review` | Opt-in trigger for AI review |
| `auto-approve-eligible` | Applied by judge when PR passes all merge policy criteria |
| `needs-human-review` | Applied by judge when human review is required |
