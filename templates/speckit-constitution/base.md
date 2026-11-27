<!-- templates v1.0.0: speckit-constitution/base.md -->
# [PROJECT_NAME] Constitution

[PROJECT_SUMMARY: one or two sentences on what the product does and for whom.]

**Version**: 1.0.0 | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE] | **Based on**: Dodo Tech Hub templates v1.1.0 ([MODULES, for example base, mobile])

## Governance

- This constitution overrides other project practices. Runtime guidance for agents lives in `AGENTS.md`.
- When the rules of an agent tool or plugin conflict with this constitution, the constitution wins.
- Amendments are made through a pull request that updates this file, explains the change, and bumps the version.
- Versioning follows [Semantic Versioning](https://semver.org): MAJOR for removing or redefining a principle, MINOR for adding a principle or section or materially expanding guidance, PATCH for wording fixes.
- Every plan MUST include a Constitution Check that confirms compliance with every principle in this file. Any exception MUST be written in the plan with its reason and approved by the project owner.

## Development Workflow & Quality Gates

- Features follow the Spec Kit flow: specify, clarify (optional), plan, tasks, implement, converge.
- A change is ready to merge only when: all tests pass, the type checker and linter report no errors, and every security rule in this constitution is met.
- A report of finished work MUST state the highest level reached: builds (compiles, type check and lint pass), tested (automated tests pass), run (checked in a running app or environment), released. If a level cannot be checked in the current environment, the report MUST say so.

## Core Principles

### Keep It Simple (KISS, YAGNI)

- Build only what a spec asks for. Do not add code, options, or abstractions for a future that is not specified.
- Before writing code, check in this order and stop at the first step that works: (1) is it needed at all, (2) does code in the repo already do it, (3) does the language's standard library do it, (4) does the platform or framework do it, (5) does an installed dependency do it, (6) only then write the minimum new code.
- A deliberate shortcut with a known limit MUST carry a comment that names the limit and when to upgrade.

Rationale: every line of code must be read, tested, and maintained. The simplest design that meets the spec is the cheapest to change.

### Clear Structure (Separation of Concerns, Single Responsibility, DRY)

- **One home per thing**: code is grouped by responsibility. New code goes in the module that owns that responsibility. A new top-level module is allowed only when no existing one fits, and the plan MUST say why. Catch-all files (`utils`, `helpers`, `misc`) are not allowed.
- **One job per file**: each file and function has one clear purpose. Split it when it starts doing more than one job.
- **Business logic apart**: business logic MUST stay separate from UI, network, and storage code, so it can be reused and unit tested on its own.
- **Do not repeat yourself**: when the same logic is needed in two or more places, extract it into one shared module. Do not extract for a single use.
- **One way per job**: follow the patterns already in the codebase. A second way to do the same job (another state library, HTTP client, or styling approach) MUST NOT be added without a plan that justifies it and owner approval.
- **Open for extension**: a new capability SHOULD be a new function, component, or module plugged in at a clear integration point. Existing code MUST NOT grow flags or `if (newFeature)` branches to carry a capability that is not its job. Bug fixes may edit existing code directly.
- **Restructure instead of forcing it**: if a feature cannot fit without reshaping many modules, or needs new architecture, the plan MUST stop and propose a restructure to the owner. An approved restructure is its own change, done and tested before the feature work starts.

Rationale: features are added over time, often by AI agents. Clear homes and one pattern per job keep each change small and safe to review.

### Low-Effort UX

Applies to every project with a user interface.

- A user finishes each task with as few screens, transitions, and taps as possible ([Nielsen heuristic 8](https://www.nngroup.com/articles/ten-usability-heuristics/), Hick's law). Show the next step in place on the same screen before adding a new screen. Every new screen MUST be justified in the plan.
- Option sets of 2 to 5 items use inline choices (segmented button, chips, or radio list). Larger sets use a dropdown menu, searchable when the list can grow.
- Action and category buttons show an icon with a short text label. Leave out the label only when the icon is universally understood (for example close, back, search).
- Confirmation dialogs appear only for destructive or irreversible actions.
- User-facing text uses plain language: short sentences, one idea per sentence, everyday words, no technical terms (for example "Sign in", not "Authenticate").

Rationale: every extra screen, choice, and tap is a chance to stop or make a mistake.

### Readable Code

- Follow the official style guide and formatter of the language (for example Kotlin coding conventions with ktlint, Prettier and ESLint for TypeScript, PEP 8 with Ruff for Python). Formatting is enforced by tools, not by review.
- Avoid nesting `if` statements more than two levels deep. Use guard clauses (early returns) or a lookup table when it fits the case.
- Names say what a thing is or does. Comments explain why, not what.

Rationale: code is read far more often than it is written.

### Relevant Tests

- Tests follow the test pyramid: many fast unit tests, fewer integration tests, few end-to-end tests.
- Business logic MUST have unit tests. User flows changed by a feature MUST have tests that check what the user sees and does.
- Every bug fix MUST start from the root cause: read the code involved, find the cause, check every caller of the code being changed, and make the smallest change that removes the cause. A patch that only hides the reported symptom MUST NOT be merged.
- Every bug fix MUST add a regression test that fails without the fix.
- Every test MUST check behavior that matters to the user or the business logic. Tests written only to raise coverage MUST NOT be added.
- Tests MUST NOT call real external services or use real accounts. Mock them at the boundary.

Rationale: tests are the proof that a feature works and stays working. Relevance matters more than a coverage number.

### Security Baseline

The module principles below name the security standard and level for each kind of project. These rules apply to every project.

- **No secrets in the repository**: never commit secrets, tokens, keys, or `.env` files with secrets. Commit only a `.env.example` with no real values.
- **Least privilege**: request only the permissions, scopes, and access a feature needs. The spec MUST list them.
- **Untrusted input**: treat every outside input (user input, API responses, files, links, messages from other apps) as untrusted. Validate its shape before use.
- **Logging**: never log secrets, tokens, passwords, or personal data (see the OWASP Logging Cheat Sheet).
- **Transport**: HTTPS (TLS) only. Every network host MUST be listed in the spec or plan of the feature that adds it.
- **Dependencies**: add a third-party package only if it is maintained and widely used. Commit the lockfile. The dependency audit MUST report no high or critical issues before a release.
- **Personal data**: collect the minimum, keep it for the shortest time, and let the user delete it. Comply with the data protection law of the countries where the users live.

Rationale: one leaked secret or over-broad permission can harm users, and it is far cheaper to prevent than to fix.

## Specification Style (Encouraged)

These are SHOULD guidelines, not gates. Every requirement MUST still be written in text; diagrams and tables add to the text and do not replace it.

- Use [Mermaid](https://mermaid.js.org) for diagrams, because GitHub renders it natively: `flowchart` for flows and screen maps, `sequenceDiagram` for interactions between actors, `stateDiagram-v2` for anything with states, `erDiagram` for entities and relations.
- Use a Markdown table for a mapping (one row per input) and a decision table for a rule that depends on more than one condition.
- Place each diagram next to the text it explains. Split a diagram when it becomes hard to read.
- No real data in specs: no real emails, tokens, or account IDs.

Rationale: a flow, a state machine, or a table shows the logic faster than paragraphs and exposes gaps early.
