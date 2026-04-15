# Merge Policy

This document defines the criteria the AI judge uses to decide whether a pull request
in postscript-marketing-ai is eligible for auto-approve or requires human review.

The judge should err on the side of requiring human review. This service generates
customer-facing AI content -- a bad merge can result in incorrect messages reaching
end users.

## Auto-Approve Eligible

A PR may be labeled "auto-approve-eligible" ONLY if ALL of the following are true:

### 1. CI Passes
All required CI checks must be passing. If any check is pending or failing, the PR
is NOT eligible for auto-approve.

### 2. Low Risk Level
The reviewer's structured verdict must have a risk_level of "low". Medium and high
risk PRs always require human review.

### 3. No Blocking Concerns
The reviewer's structured verdict must have zero concerns with severity "blocking".
PRs with blocking concerns always require human review.

### 4. No Warning Concerns on Critical Paths
If the reviewer flagged any concerns with severity "warning", the PR requires human
review UNLESS the concerns are exclusively in test files or documentation.

### 5. High Review Confidence
The reviewer's review_confidence must be "high". If the reviewer is uncertain about
its own assessment, a human should verify.

### 6. Safe Categories Only
The PR's categories must be exclusively from this safe list:
- `test_only`
- `documentation`
- `configuration` (non-AI configuration only -- e.g., CI config, linting rules)
- `dependency_update` (minor/patch versions only)
- `refactor` (only if risk_level is "low" AND zero concerns)
- `bug_fix` (only if risk_level is "low" AND zero concerns AND not in AI generation code)
- `infrastructure` (CI config, Docker, deployment files only -- only if risk_level is "low" AND zero concerns of any severity)

The following categories ALWAYS require human review, regardless of other signals:
- `prompt_change`
- `ai_generation_logic`
- `celery_task`
- `database_model`
- `api_endpoint`
- `new_feature`

### 7. Reasonable PR Size
Very large PRs (touching more than 20 files or more than 500 lines changed) should
default to human review, even if other signals are clean. Large changes are harder
for the AI reviewer to assess completely.

## Needs Human Review

A PR should be labeled "needs-human-review" if ANY of the following are true:

1. CI is not passing
2. Risk level is "medium" or "high"
3. Any blocking concerns exist
4. Warning concerns exist on non-test, non-documentation files
5. Review confidence is "medium" or "low"
6. Categories include any "always requires human review" category
7. The PR is unusually large
8. The judge is uncertain about any of the above criteria

## Edge Cases

- **Mixed categories:** If a PR has both safe and unsafe categories (e.g., `test_only` + `celery_task`), the unsafe category takes precedence. The PR needs human review.
- **Zero concerns but high risk:** A high-risk PR with no specific concerns still needs human review. The absence of flagged concerns does not mean the absence of risk.
- **Reviewer errors:** If the structured verdict appears inconsistent (e.g., risk_level "low" but multiple blocking concerns), default to human review and flag the inconsistency in the judge's reasoning.

## Policy Version

v1.0 -- Initial policy. Expected to be refined based on feedback loop data.
The weekly policy-review.yml workflow aggregates feedback artifacts and opens
draft PRs with suggested changes.
