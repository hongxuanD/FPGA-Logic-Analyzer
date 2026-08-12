# Claude collaboration instructions

Before working in this repository, read `PROJECT_CONTEXT.md` completely. It is
the shared source of truth used by Codex and Claude for architecture, build
commands, verified results, known issues, and coordination rules.

Two rules from that file are easy to get wrong and worth repeating here:

- **Every commit needs the user's explicit permission, asked for that specific
  commit.** Permission does not carry forward. Commit to `main` with no branch,
  message body only, and no attribution trailer.
- **Ask before running a Vivado simulation.** The user wants to watch waveforms
  themselves. Supply the GUI steps and the pass/fail criterion instead.

Treat all existing uncommitted changes and unfamiliar commits as work belonging
to the user or the other assistant. Inspect Git state before editing, do not
overwrite or revert that work, and update `PROJECT_CONTEXT.md` after material
changes.
