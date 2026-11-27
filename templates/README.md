# Project templates

Version 1.1.0. Starting rules for new Dodo Tech Hub projects: a Spec Kit constitution and an `AGENTS.md`, built from modules.

Rules are based on popular industry standards (OWASP, Twelve-Factor, WCAG, Material Design, Semantic Versioning, Conventional Commits, and others) and name them, so people and agents can look them up. Add a custom rule only when no standard covers it.

## Layout

| Module | Constitution | AGENTS.md | Status |
|--------|--------------|-----------|--------|
| `base` | `speckit-constitution/base.md`: governance, quality gates, KISS/YAGNI, structure, low-effort UX, tests, security baseline, spec style | `agents.md/base.md`: Spec Kit workflow, docs rules, git rules | Used by magpie and android-scheduler |
| `mobile` | `speckit-constitution/mobile.md`: OWASP MASVS, WCAG 2.2, adaptive layout, device check | `agents.md/mobile.md`: build commands, devices, release, platform limits. Also `skills/app-store-audit/` and `dependabot.yml` | Used by magpie and android-scheduler |
| `backend` | `speckit-constitution/backend.md`: Twelve-Factor, OWASP ASVS, migrations, backups | `agents.md/backend.md`: environments, deploy, rollback | Draft |
| `api` | `speckit-constitution/api.md`: OpenAPI, RFC 9457, OWASP API Security Top 10 | `agents.md/api.md`: contract path and tests | Draft |
| `cli` | `speckit-constitution/cli.md`: clig.dev, exit codes, stable interface | `agents.md/cli.md`: build, release | Draft |
| `web` | `speckit-constitution/web.md`: OWASP ASVS, WCAG 2.2, Core Web Vitals | `agents.md/web.md`: dev server, hosting | Draft |

`base` is always first. Add one or more kind modules after it. A Draft module has not been used in a real project yet: review it closely and improve it with what the first project learns.

## Start a new project

1. Create the repository and run `specify init --here --ai claude` (or your agent) to install Spec Kit.
2. Assemble the files, choosing the modules the project needs. For a mobile app with its own backend API:

    ```bash
    T=path/to/.github/templates
    cat $T/speckit-constitution/{base,mobile,backend,api}.md > .specify/memory/constitution.md
    cat $T/agents.md/{base,mobile,backend,api}.md > AGENTS.md
    echo '@AGENTS.md' > CLAUDE.md
    ```

3. For a mobile app, add the store compliance audit. It uses the [App Store Compliance Playbook](https://github.com/mjmirza/app-store-compliance) as a git submodule, and Dependabot opens a pull request when the playbook changes:

    ```bash
    git submodule add --depth 1 https://github.com/mjmirza/app-store-compliance .agents/vendor/app-store-compliance
    git config -f .gitmodules submodule..agents/vendor/app-store-compliance.shallow true
    mkdir -p .agents/skills .claude/skills .github
    cp -r $T/skills/app-store-audit .agents/skills/
    ln -s ../../.agents/skills/app-store-audit .claude/skills/app-store-audit
    cp $T/dependabot.yml .github/dependabot.yml   # or add its entry to an existing file
    ```

    Link the skill from every other agent's skills folder the same way (for example `.cursor/skills/`). Add the audit to the project's release steps. The audit warns only; the developer decides whether to continue. Example: [magpie](https://github.com/Dodo-Tech-Hub/magpie) (pull request #55).
4. Fill every `[PLACEHOLDER]`. Running `/speckit-constitution` fills the constitution's placeholders interactively. Find what is left with `grep -rnE '\[[A-Z_]+[],:]' AGENTS.md .specify/memory/constitution.md .agents/skills/app-store-audit`.
5. Add the project's own principles to the constitution (for example domain rules, third-party API limits). Number the principles if you want to refer to them by number.
6. Delete rules that do not apply, and say why in the first commit message.

`CLAUDE.md` exists because Claude Code reads `CLAUDE.md`, and `@AGENTS.md` imports the shared file. Other agents read `AGENTS.md` directly.

## Keep templates and projects in step

- Each file starts with a comment naming the template version and module, so a project shows what it was built from.
- When a project adds a rule that would help every project of its kind, move it here in a pull request and bump the version.
- Projects are not updated automatically. Copy a template change into a project only when the project needs it.
- Versioning follows [Semantic Versioning](https://semver.org): MAJOR when a rule is removed or redefined, MINOR when a rule or module is added, PATCH for wording fixes.
