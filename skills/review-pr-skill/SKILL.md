---
name: review-pr-skill
description: Review a PR (this branch vs main) using a consistent workflow and output format. Use when the user asks to review a pull request, inspect branch diff quality, or report findings with severity and actionable fixes.
---

# review_pr_skill

## Scope
- Compare: this branch vs main
- Only comment on changes introduced by this branch (diff scope).
- If needed, mention relevant surrounding code for context, but anchor findings to the diff.

## Workflow
1) Identify changed files and main change themes.
2) For your assigned review point, scan diffs and related call sites.
3) For each potential issue, collect "evidence":
   - file path(s)
   - function/class/symbol name(s)
   - what changed (brief)
4) Classify severity:
   - High: security risk, data loss/corruption, serious bug, production outage risk
   - Medium: likely bug, missing checks, degraded reliability, poor test signal
   - Low: readability/nits, minor maintainability concerns
   - Uncertain: not enough evidence / needs confirmation
5) Propose an actionable fix (smallest effective change).
   - Avoid broad refactors unless required to resolve the issue.

## Output format (use exactly)
For each finding:

- Title: （短い要約）
- Severity: High / Medium / Low / Uncertain
- Summary: （何が問題か）
- Evidence: （根拠：ファイル/シンボル/差分の要点。分からなければ「未確認」）
- Impact: （どういう不具合/リスクにつながるか）
- Suggestion: （最小の対応案）
- Notes: （前提・仮説があれば明記）
