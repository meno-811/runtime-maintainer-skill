# Runtime Inventory

Use this inventory for runtime facts that remain useful after the current session ends.

```text
.runtime-inventory/
├── INDEX.md
├── command.md
├── file.md
├── web.md
├── service.md
├── remote/
├── cache/
└── archive/
```

## Files

- `command.md`: Agent-installed or agent-configured global commands.
- `file.md`: Important files and directories, including projects, configs, models, datasets, reusable scripts, and selected source structure.
- `web.md`: Durable network resources such as databases, proxies, APIs, SSH hosts, and web services.
- `service.md`: Persistent services that remain meaningful across sessions.
- `remote/`: Recursive runtime inventories for reachable remote hosts.
- `cache/`: Lightweight session cache files named `YYYY-MM-DD_session-topic.md`.
- `archive/`: Archived cache files and whole-file snapshots.

## Update Rules

- Read a target runtime file before editing it.
- Record durable facts, not one-off session noise.
- Check relevant cache before reading main files.
- Create a cache file for the session when runtime maintenance is needed and no suitable cache exists.
- Do not put plaintext credentials in cache.
- Mark stale information instead of deleting it by default.

## Recent Updates

Add short notes here when files are materially updated.

## High-Frequency Entries

Add links or short references here when an entry is used often.
