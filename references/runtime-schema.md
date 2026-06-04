# Runtime Inventory Schema

Runtime inventory is a persistent local fact store for agents. It records commands, files, web resources, services, remote hosts, and lightweight session caches that remain useful across sessions.

## Paths

- Global inventory: `~/.runtime-inventory`
- Project inventory: `<project-root>/.runtime-inventory`
- Remote inventory: `remote/<host-or-ip>/` inside an inventory

Do not write real inventory data inside the skill folder. The skill folder contains only instructions, references, and templates.

## Directory Structure

```text
.runtime-inventory/
├── INDEX.md
├── command.md
├── file.md
├── web.md
├── service.md
├── remote/
│   └── <host-or-ip>/
│       ├── INDEX.md
│       ├── command.md
│       ├── file.md
│       ├── web.md
│       ├── service.md
│       ├── remote/
│       ├── cache/
│       └── archive/
├── cache/
│   └── YYYY-MM-DD_session-topic.md
└── archive/
```

Remote inventory is recursive. Templates should show the complete structure, but real runtime maintenance may create only the files and directories that are currently needed.

## Read And Write Rules

- Always read the target runtime file before updating it.
- Preserve useful existing content and update according to facts.
- Do not delete stale information by default; mark it `stale` or `deprecated`.
- Prefer concise records over exhaustive listings.
- If uncertain, record uncertainty with `status: unverified` or `status: stale`.

When both project and global inventory may apply, read project cache and files first, then global cache and files. Write project-specific facts to project inventory. Write device-level, cross-project, or tool-like facts to global inventory.

## Common Fields

Use Markdown sections with English field names.

```markdown
## entry-name

- type: command | file | web | service
- status: confirmed | unverified | stale | deprecated
- source: user-provided | agent-discovered | agent-installed | project-config | remote-inspection | inferred
- updated_at: YYYY-MM-DD
- description: Short description of why this matters.
- notes: Optional operational notes.
```

No stable ID is required. A section heading is enough.

`source` may contain multiple values:

```markdown
- source: user-provided, agent-discovered
```

## Status Values

- `confirmed`: Verified usable.
- `unverified`: Provided, inferred, or discovered but not verified.
- `stale`: Probably outdated or needs recheck.
- `deprecated`: Kept for history but not recommended for new use.

## Sensitive Fields

Plaintext credentials are allowed only in long-term runtime files, not in cache. Any entry containing plaintext credentials must include:

```markdown
- sensitive: true
- credential_scope: Where and when the credential may be used.
- handling: Do not expose outside local runtime maintenance unless the user explicitly asks.
```

Cache files must reference the main entry instead of repeating plaintext credentials.

## command.md

Record commands installed or configured by an agent that became globally available.

Do not record every command in `PATH`. Do not record system-provided commands merely because they exist.

Suggested fields:

```markdown
## psql-client

- type: command
- status: confirmed
- source: agent-installed
- updated_at: 2026-06-04
- command: psql
- path: /usr/local/bin/psql
- install_method: apt install postgresql-client
- description: PostgreSQL client used to inspect reachable databases.
- notes: Record only because the agent installed it for reuse.
```

If a downloaded executable is kept but not made global, record it in `file.md`. If shell configuration or global path changes make it globally usable, record it in `command.md`.

## file.md

Record important files and directories: projects, source entry points, configs, models, datasets, generated artifact directories, reusable scripts, and files explicitly meant for future reuse.

Avoid uncontrolled growth. Directory-level summaries are often better than per-file entries.

Suggested fields:

```markdown
## project-runtime-docs

- type: file
- status: confirmed
- source: project-config, agent-discovered
- updated_at: 2026-06-04
- path: /path/to/project
- kind: project-root
- description: Product documentation workspace for the runtime-maintainer skill.
- notes: Use directory-level summaries unless individual files become important.
```

For changed files, record only when the file is an entry point, config, model, dataset, generated artifact directory, project structure document, reusable script, or otherwise likely to matter across future sessions.

Placement depends on context. A reusable tool script may belong in global inventory; work-target source code usually belongs in project inventory.

## web.md

Record durable network resources, including databases, proxies, APIs, SSH hosts, dashboards, and reachable services on other hosts.

Suggested fields:

```markdown
## local-network-postgres

- type: web
- status: unverified
- source: user-provided
- updated_at: 2026-06-04
- endpoint: 192.168.2.13:8002
- protocol: postgresql
- auth: username psql, password 123
- sensitive: true
- credential_scope: Local network PostgreSQL access only.
- handling: Do not expose outside local runtime maintenance unless the user explicitly asks.
- description: PostgreSQL database reachable on the local network.
- verification: Connectivity may be tested when needed; no lightweight query is required just for verification.
```

Agents may perform connectivity tests when useful. Test frequency is not fixed. Database verification does not require a query unless the task needs it.

For SSH hosts, create a recursive inventory under `remote/<host-or-ip>/` and record the SSH entry in `web.md`.

## service.md

Record persistent services that remain active or meaningful across sessions.

Do not record temporary development servers unless the user explicitly makes them persistent runtime facts.

Suggested fields:

```markdown
## ollama

- type: service
- status: confirmed
- source: agent-discovered
- updated_at: 2026-06-04
- service_name: ollama
- endpoint: http://127.0.0.1:11434
- start_method: system service or existing daemon
- check_method: curl http://127.0.0.1:11434/api/tags
- description: Local model service available across sessions.
- notes: Record because it is persistent, not because a temporary process is running.
```

## cache

Cache files live in `cache/` and are named:

```text
YYYY-MM-DD_session-topic.md
```

Chinese session topics may remain Chinese. `last_updated_at` goes inside the file, not in the filename.

Cache header:

```markdown
- session_topic: runtime maintainer design
- last_updated_at: 2026-06-04T12:30:00+08:00
- scope: Project, global, or remote scope for this cache.
```

Cache may contain high-frequency runtime references, recently verified status, short summaries, and temporary session judgments. It should not copy entire main files, large source summaries, or plaintext credentials.

## archive

The first version of `archive/` stores archived cache files and whole-file snapshots only. Do not move individual long-term records into archive unless the entire resource no longer needs to be visible. Prefer marking entries `stale` or `deprecated`.

## INDEX.md

`INDEX.md` describes the file tree, the purpose of each file, update rules, recent updates, and high-frequency entry points. It should not duplicate all records from the category files.
