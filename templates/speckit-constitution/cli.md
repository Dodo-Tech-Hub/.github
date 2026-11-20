
<!-- templates v1.0.0: speckit-constitution/cli.md (draft: not yet used in a project) -->
## CLI Principles

### Command Line Interface Guidelines

Commands follow the [Command Line Interface Guidelines](https://clig.dev):

- Every command has `--help`, and the tool has `--version`.
- Exit code `0` means success. Any failure exits with a non-zero code.
- Data goes to stdout. Messages, progress, and errors go to stderr.
- Output for people is the default. A `--json` flag gives stable, machine-readable output.
- Never prompt when stdin is not a terminal. Every prompt has a flag that answers it.
- Respect `NO_COLOR` and disable color when output is not a terminal.
- Settings are read in this order, first wins: flags, environment variables, project config, user config, defaults. Config files follow the [XDG Base Directory](https://specifications.freedesktop.org/basedir-spec/latest/) layout.
- A destructive action asks for confirmation, or needs `--force` when not interactive.

Rationale: a CLI is used by people and by scripts. These conventions let both use it without reading the source.

### Stable Interface

- Command names, flags, output formats, and exit codes are the public interface and follow [Semantic Versioning](https://semver.org).
- Never print secrets. Read them from the environment, a file, or stdin, never from a flag value (flags appear in shell history and process lists).

Rationale: scripts break silently when a CLI changes its interface.
