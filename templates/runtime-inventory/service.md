# Services

Record persistent services that remain useful across sessions. Do not record temporary development servers unless they become durable runtime facts.

## Template

- type: service
- status: confirmed | unverified | stale | deprecated
- source: user-provided | agent-discovered | agent-installed | project-config | remote-inspection | inferred
- updated_at: YYYY-MM-DD
- service_name:
- endpoint:
- start_method:
- check_method:
- stop_or_restart_method:
- description:
- notes:
