---
name: ac-review
description: Use when asked for a code review of a branch, PR, or set of changes against requirements. Review a feature branch against a base branch and its acceptance criteria (AC). Checks correctness, test coverage, scope, simplicity, and project conventions.
---

# AC Review

Review a feature branch against a base branch and its acceptance criteria (AC).

## Inputs
- Feature branch
- Base branch
- Acceptance criteria

If any of these are missing from the prompt, ask for them before starting.

## Process
1. Fetch the latest changes and get the diff:
   ```
   git fetch origin
   git diff origin/<base>...<branch>
   ```
2. Determine what kind of codebase this is (AWS microservice, React/Next app, Playwright test suite, etc.) and use that context throughout the review.
3. Analyze what the change does and compare it against the AC.

## Review Criteria
- **Correctness**: Does the solution meet the AC?
- **Test Coverage**: Is the test coverage adequate? Does it avoid low-value tests and assertions? For example, tests should not assert console output.
- **Scope**: Does the change stick to the stated ask, without bundling unrelated changes?
- **Simplicity**: Is the solution more complex than it needs to be, or over-engineered? Is there an approach that meets the ask with less complexity, less code, or less ambiguity?
- **Conventions & Best Practices**: Does the new code follow the project's existing coding patterns? Does it follow industry best practices for this type of codebase?
- **Performance**: Does the change add unnecessary work or cost? Only flag issues with a real impact, not micro-optimizations.
  - AWS: DynamoDB scans where a query or index would work, missing pagination, N+1 calls to downstream services, sequential awaits that could run in parallel, heavy initialization inside the Lambda handler instead of outside it, large dependencies that slow cold starts.
  - React/Next: unnecessary re-renders (unstable props, missing memoization where it measurably matters), data-fetching waterfalls, client components that could be server components, large libraries added to the client bundle, missing `next/image` or lazy loading for heavy assets.
- **Security**: Does the change introduce a vulnerability or weaken existing protections?
  - AWS: IAM permissions broader than needed (wildcard actions or resources), secrets hardcoded or in plain environment variables instead of Secrets Manager or Parameter Store, unvalidated input reaching queries or downstream calls, sensitive data (PII, tokens) written to logs, public S3 or API exposure without authorization.
  - React/Next: secrets exposed through `NEXT_PUBLIC_` variables or client components, `dangerouslySetInnerHTML` with untrusted content, server actions or API routes missing authentication and authorization checks, user input used in redirects or URLs without validation.

## Output
- **Score**: Is the code as written ready to merge? Ready or Not Ready.
  - Not Ready if there are any P0 or P1 issues or any AC that is not fully met.
- **AC Coverage**: List each criterion as Met, Partially Met, or Not Met, with a one-line reason.
- **Issues**: Sorted by severity, each with a `file:line` reference:
  - **P0**: Must fix before merge
  - **P1**: Should fix before merge
  - **P2**: Should fix, can be a follow-up
  - **P3**: Nitpick

  Where applicable, include a code block showing the suggested fix.

  If there are no issues, say so explicitly. Do not invent issues to fill this section.
- **Questions for the author**, if any.