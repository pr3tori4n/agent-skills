---
name: ac-review
description: Use when asked for a code review of a branch, PR, or set of changes, including reviews against acceptance criteria (AC), requirements, or a ticket.
---

# AC Review

Review a feature branch against a base branch and its acceptance criteria (AC).

## Inputs
- Feature branch
- Base branch
- Acceptance criteria
- Repository (Repo)

If any of these are missing from the prompt, ask for them before starting.
If the prompt is made with a single repo context, use that as the repo and don't request it. If it's from a context with multiple repos, prompt for which repos the ticket spans.

## Process
1. Fetch the latest changes and get the diff:
   ```
   git fetch origin
   git diff origin/<base>...<branch>
   ```
2. Determine what kind of codebase this is (AWS microservice, React/Next app, Playwright test suite, etc.) and use that context throughout the review.
3. Analyze what the change does and compare it against the AC.
4. Never change the current checkout; to run tests, use a separate worktree (`git worktree add ../review-<branch> origin/<branch>`), and state in the review if tests were not run.
5. Go through the tests added or changed in the diff one at a time. For each, ask what would have to break for it to fail; a test with no such answer, or one whose answer does not match its name, is low-value. Count them as you go. Review test files test by test, not as a block — thorough coverage in one area does not excuse a weak test in another. Report only the totals and the findings worth acting on, not a line per test.

## Review Criteria
- **Correctness**: Does the solution meet the AC?
- **Test Coverage**: Is the coverage adequate, and does each test earn its place? Flag these patterns by name:
  - **Asserts nothing its name claims** — `it('uses 8000ms duration')` that only asserts the element rendered.
  - **Asserts a mock** — the thing under test is mocked out, so the test proves the mock renders.
  - **Change detector** — `not.toHaveClass('p-4')` pinning the value this diff removed; passes forever after merge.
  - **Redundant** — a weaker assertion the same diff has superseded elsewhere.
  - **Smoke test as coverage** — "it renders" standing in for a behavioural assertion.
  - **Asserts console output** — the test checks what was logged rather than what the code did.

  A pre-existing weak test that this diff makes newly fixable or newly redundant is in scope.
- **Scope**: Does the change stick to the stated ask, without bundling unrelated changes?
- **Simplicity**: Is the solution more complex than it needs to be, or over-engineered? Is there an approach that meets the ask with less complexity, less code, or less ambiguity?
- **Conventions & Best Practices**: Does the new code follow the project's existing coding patterns? Does it follow industry best practices for this type of codebase?
- **Compatibility**: Does the change break existing consumers (API or event shapes, database keys or schemas) without a migration or rollout plan?
- **Performance**: Does the change add unnecessary work or cost? Only flag issues with a real impact, not micro-optimizations.
  - AWS: DynamoDB scans where a query or index would work, missing pagination, N+1 calls to downstream services, sequential awaits that could run in parallel, heavy initialization inside the Lambda handler instead of outside it, large dependencies that slow cold starts.
  - React/Next: unnecessary re-renders (unstable props, missing memoization where it measurably matters), data-fetching waterfalls, client components that could be server components, large libraries added to the client bundle, missing `next/image` or lazy loading for heavy assets.
- **Security**: Does the change introduce a vulnerability or weaken existing protections?
  - AWS: IAM permissions broader than needed (wildcard actions or resources), secrets hardcoded or in plain environment variables instead of Secrets Manager or Parameter Store, unvalidated input reaching queries or downstream calls, sensitive data (PII, tokens) written to logs, public S3 or API exposure without authorization.
  - React/Next: secrets exposed through `NEXT_PUBLIC_` variables or client components, `dangerouslySetInnerHTML` with untrusted content, server actions or API routes missing authentication and authorization checks, user input used in redirects or URLs without validation.

## Output
- **Score**: Is the code as written ready to merge? Ready or Not Ready.
  - Not Ready if any AC is Not Met or there are any P0 or P1 issues.
- **AC Coverage**: List each criterion as Met or Not Met, with a one-line reason. Mark it Met when the required behavior works, even if the implementation has quality problems. Record quality problems in delivered code (edge cases, weak tests, style) under Issues with their own severity, not here.
- **Issues**: Sorted by severity, each with a `file:line` reference:
  - **P0**: Must fix before merge. Critical impact issue that cannot hit production. An exploitable security issue, data loss or corruption, or an AC not met.
  - **P1**: Should fix before merge. A high impact issue. A bug outside the AC, , over-broad permissions, PII in logs, or breaking a documented project rule. Unnecessary complexity that requires a re-write.
  - **P2**: Should fix, can be a follow-up. Issue of moderate impact, not business critical. Missing tests for the main behavior an AC describes (e.g., the AC adds login and there are no login tests). Weak or low-value tests, unnecessary complexity that can be iterated on, unrelated changes in the branch. An issue that pre-dates this branch - even if it under other circumstances would be rated higher severity.
  - **P3**: Nitpick. Style, naming, small improvements. Missing tests for non-core functionality (e.g., login is tested but not expired tokens or lockout).

  If an issue fits more than one level, use the highest. If an issue fits into one of the Review Criteria categories, tag it with that category.

  Where applicable, include a code block showing the suggested fix.

  If there are no issues, say so explicitly. Do not invent issues to fill this section.
- **Test Summary**: One line — tests added, tests changed, and how many are low-value, e.g. `14 added, 3 changed, 4 low-value`. Count a pre-existing weak test only if this diff makes it newly redundant or newly fixable, and say so. If the count is above zero, the tests themselves go under Issues with their pattern named; do not list the sound ones anywhere.
