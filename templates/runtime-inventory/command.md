# Commands

Record agent-installed or agent-configured commands that became globally available. Do not list every command in `PATH`, and do not record system commands just because they exist.

## Template

- type: command
- status: confirmed | unverified | stale | deprecated
- source: user-provided | agent-discovered | agent-installed | project-config | remote-inspection | inferred
- updated_at: YYYY-MM-DD
- command:
- path:
- install_method:
- description:
- notes:
