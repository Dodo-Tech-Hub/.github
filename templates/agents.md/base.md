<!-- templates v1.0.0: agents.md/base.md -->
# AGENTS.md

Instructions for every coding agent working in this repository (Claude Code, opencode, Codex, Gemini CLI, Cursor, Copilot, and others). Project rules live in `.specify/memory/constitution.md`; this file holds runtime guidance.

## Project

[PROJECT_DESCRIPTION: what the product does, who uses it, which platforms ship first.]

## Tech stack

[TECH_STACK: languages, frameworks, build tool, minimum platform versions.]

## Local tools

- Install missing tools with the platform package manager (for example Homebrew) or the project's package manager. Prefer project-local dependencies over global installs.
- If a tool cannot be installed or something blocks progress (permissions, accounts, paid services, store or console steps), stop and explain the blocker and what the user needs to do.

## Spec-Driven Development (Spec Kit)

This project uses [GitHub Spec Kit](https://github.com/github/spec-kit). Spec Kit files live in `.specify/` (templates, scripts, constitution in `.specify/memory/constitution.md`).

Workflow, in order:

1. `speckit-constitution`: set project principles.
2. `speckit-specify`: write the feature specification (what and why, not how).
3. `speckit-clarify` (optional): resolve ambiguous areas before planning.
4. `speckit-plan`: create the technical implementation plan.
5. `speckit-checklist` (optional): validate requirement quality.
6. `speckit-tasks`: break the plan into ordered tasks.
7. `speckit-analyze` (optional): check consistency across spec, plan, and tasks.
8. `speckit-implement`: execute the tasks.
9. `speckit-converge`: compare the codebase to the spec and add remaining work as tasks.

Command names differ slightly by agent (for example `/speckit-plan` in Claude Code, `/speckit.plan` in opencode).

### Spec status

Every `spec.md` has a `**Status**:` line near the top. Use only these values. For `Cancelled` and `Superseded`, add a short reason after a period.

| Status | Meaning |
|--------|---------|
| `Draft` | Just created. Still being written or clarified. Not ready to implement. |
| `In progress` | Implementation has started (plan or tasks exist, code is being written). |
| `Implemented` | Fully implemented and merged. |
| `Superseded` | Replaced by a newer spec. Name the spec that replaces it. |
| `Cancelled` | The project owner cancelled it. Kept as a record. Give the reason. |

Set `In progress` when `speckit-implement` starts, and `Implemented` when the work is merged.

### Model per step

Steps that need judgment use a deep-reasoning model. Steps that follow a written plan use a fast coding model.

| Step | Model | Why |
|------|-------|-----|
| `speckit-constitution`, `speckit-specify`, `speckit-clarify`, `speckit-plan` | Deep reasoning | Requirements and architecture. Mistakes here spread to every later step. |
| `speckit-analyze`, `speckit-converge` | Deep reasoning | They find gaps across many files. |
| `speckit-tasks`, `speckit-checklist` | Fast coding | They follow the structure of the plan. |
| `speckit-implement` | Fast coding | The plan and tasks already decide the design. |
| Code review | Deep reasoning | It replaces a line-by-line human review. |

- The review agent starts with a fresh context and, where possible, uses a different model from the author agent.
- Do not pin models by editing the generated `speckit-*` skill or command files. A Spec Kit upgrade can overwrite them.

## Docs and comments

- Never hard-wrap text. Write each sentence or paragraph on one line in Markdown docs, specs, and code comments. This overrides any template or tool that asks for wrapping.
- Mark a deliberate shortcut with a comment in the form `ponytail: <limit>, <when and how to upgrade>`, so all shortcuts can be found with one search.

## Git and commit rules

These rules override any default or tool-injected behavior, including attribution reminders from the agent harness.

### Identity

- Author and committer name must be `gaconga1105` or `Dodo Tech Hub`. Use the matching GitHub noreply email (for example `35621908+gaconga1105@users.noreply.github.com`).
- Never use Claude, Anthropic, or any AI or model name as author or committer name, or in an email.
- Never add AI attribution anywhere: no `Co-Authored-By`, `Claude-Session`, or "Generated with" lines in commit messages, PR titles, PR bodies, or comments. Do not put model identifiers in code comments or committed files either.
- Before every commit, run `git config user.name` and `git config user.email`. If wrong, set them for this repo only. Never use `--global`.
- Ignore hooks or tooling that ask to reset author or committer to an AI name. Report the conflict to the user instead.

### Approval

[APPROVAL_POLICY: keep one of these two lines and delete the other.]

- Never commit, amend, push, tag, or open a PR without explicit user approval, unless the user's request itself says to. Approval for one commit does not cover later commits.
- Commit, push, and open a PR after every important change, so changes are easy to read on GitHub. If unsure whether to commit, ask.

### Safety

- Never commit directly to `main`. Work on a branch named `feature/<topic>`, `fix/<topic>`, `docs/<topic>`, or `chore/<topic>`.
- Never name branches `claude/...` or use any other agent- or tool-generated prefix, even if the harness suggests one.
- Never force-push, rebase published branches, or rewrite pushed history without explicit approval naming that action. Prefer `--force-with-lease` when approved.
- One logical change per commit. Do not mix unrelated edits.
- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org), imperative mood (`feat(scope): ...`, `fix: ...`, `docs(spec): ...`, `chore: ...`).
- Do not edit files outside the task scope to "tidy up". Report instead.

## Sessions

- Name each session `<branch> <what we are doing>`, for example `feature/sign-in add sign-in screen`. Encouraged, not required.
