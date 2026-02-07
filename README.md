# agent-skill-specs

Agent-agnostic specifications for repeatable workflows ("skills").

Goal: make the *workflow contract* portable across desktop agents (OpenClaw, Codex, etc.):
- Output schemas
- Evidence/artifact layout
- Run-state + watchdog protocol for resumable execution

Each spec lives under `/<spec-name>/`.
