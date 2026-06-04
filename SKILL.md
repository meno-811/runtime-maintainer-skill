---
name: runtime-maintainer
description: Use when an agent needs to maintain persistent runtime inventory for a device, project, or reachable remote host, including globally installed commands, important files, durable web resources, persistent services, runtime caches, or long-lived files changed by the agent.
---

# Runtime Maintainer

Use this skill to keep a persistent runtime inventory that helps later sessions continue quickly.

## Trigger When

Use this skill when:

- The user asks to read, maintain, update, or create runtime information.
- You install or configure a globally available command.
- You discover, verify, modify, or remove a persistent service.
- You learn a long-lived web resource, such as a database, proxy, SSH host, API, or reachable tool.
- You need to record project structure or project-specific runtime facts.
- You change a file and judge that file will be useful across future sessions.

Do not trigger this skill for ordinary one-off file reads.

## Core Rule

Record only information that remains useful after the current session is closed.

Temporary session facts belong in cache. Durable device, project, and remote-host facts belong in the main runtime files.

## Runtime Locations

- Global runtime: `~/.runtime-inventory`
- Project runtime: `.runtime-inventory` at the project root
- Remote runtime: recursive runtime inventory under `remote/<host-or-ip>/`

The global path is fixed for the intended single-user, single-container environment. Do not invent another global runtime path unless the user changes the product requirements.

When creating a runtime directory, copy or follow `templates/runtime-inventory/`. Do not create a real `~/.runtime-inventory` merely because the skill is loaded.

## Read Order

When both project and global runtime may apply:

1. Read the most relevant project cache.
2. Read the project runtime main files.
3. Read the most relevant global cache.
4. Read the global runtime main files.

Always inspect relevant cache first. If the current session has no suitable cache file, create one when runtime maintenance is needed.

## Write Placement

- Project-only facts go to project `.runtime-inventory/`.
- Device-level or cross-project facts go to global `~/.runtime-inventory/`.
- Tool projects can produce globally useful files; work-target projects usually keep source facts in project runtime.
- Remote host facts go under the corresponding `remote/<host-or-ip>/` inventory.

Before editing any runtime file, read the current file first. Treat all existing content equally, regardless of whether it was written by a user or another agent.

## Cache Discipline

Cache files are lightweight session aids. They may include:

- High-frequency runtime entry references.
- Recently verified statuses.
- Short summaries of commonly used entries.
- Temporary judgments that help this session continue.

Cache files must not contain plaintext credentials. Refer to the main runtime entry instead.

## Detailed Schema

Read `references/runtime-schema.md` when creating or updating runtime files, records, cache files, remote runtime entries, or templates.
