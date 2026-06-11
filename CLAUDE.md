# APPLES — Git-Authoritative Apple Audit Trail

APPLES is the durable, git-committed backup of every Apple filed to IDUNA. Each Apple is a
committed JSON file. Git is the custody chain. MJOLNIR reads MANIFEST.json as an offline cache.

## North Star

`docs/NORTHSTAR.md` — intent and architecture.

## How Apples Get Here

1. Emily Prime or any agent calls `POST /api/v1/apples` on IDUNA.
2. IDUNA persists the Apple to SQLite.
3. `IdunaClient.PostApple()` (emily-agent/iduna.go) fires a goroutine: `emily sync --apples-git-dir <dir>`.
4. `emily sync` fetches new Apples from IDUNA, commits each as a JSON file, pushes.

Auto-sync is triggered by the `APPLES_GIT_DIR` env var in emily-agent.

## File Layout

```
YYYYMMDD/<apple-id>_<apple-type>.json   ← one file per Apple
MANIFEST.json                           ← index for MJOLNIR offline cache
docs/NORTHSTAR.md
docs/SCHEMA.md                          ← Apple JSON schema (fields, types)
```

## Apple JSON Schema

Key fields: `id`, `apple_type`, `title`, `body`, `source_repo`, `source_agent`,
`requires_ceo` (bool), `recorded_at`.

Apple types: `improvement`, `observation`, `audit`, `escalation`, `completion`, `backlog_completion`.

## Manual Sync

```bash
emily sync --apples-git-dir /home/fatbaby/APPLES
emily sync --apples-git-dir /home/fatbaby/APPLES --watch   # continuous
```

## Related Repos

- `IDUNA` — primary Apple store; APPLES is the git backup
- `EMILY` — emily-agent fires auto-sync after every Apple POST (APPLES_GIT_DIR env var)
- `MJOLNIR` — Android app; reads MANIFEST.json as offline Apple cache
- `emily.cli` — `emily sync` is the sync tool
