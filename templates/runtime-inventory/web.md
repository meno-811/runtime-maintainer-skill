# Web Resources

Record durable network resources such as databases, proxies, APIs, SSH hosts, dashboards, and remote services. Plaintext credentials are allowed only in long-term runtime files and must be marked sensitive.

## Template

- type: web
- status: confirmed | unverified | stale | deprecated
- source: user-provided | agent-discovered | agent-installed | project-config | remote-inspection | inferred
- updated_at: YYYY-MM-DD
- endpoint:
- protocol:
- auth:
- sensitive: false
- credential_scope:
- handling:
- description:
- verification:
- notes:
