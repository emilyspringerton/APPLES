# APPLES

Git-authoritative durable record of all EINHORN_INDUSTRIAL agent activity.

Every Apple is a committed JSON file. Git is the custody chain.

## Structure

```
YYYYMMDD/<apple-id>_<apple-type>.json   ← one file per Apple
MANIFEST.json                           ← index for MJOLNIR offline cache
docs/NORTHSTAR.md                       ← intent and architecture
docs/SCHEMA.md                          ← Apple JSON schema
```

## How to read

```bash
git log --oneline           # activity log
git show <hash>             # full Apple
grep -r "requires_ceo" .    # CEO-visibility escalations
cat MANIFEST.json | jq '.apples | length'  # total Apple count
```

## How Apples get here

`emily sync --apples-git-dir /home/fatbaby/APPLES --watch`

Polls IDUNA, commits each new Apple, pushes. Runs as part of `emily start`.

## Docs

- [Northstar](docs/NORTHSTAR.md) — why this repo exists
- [Schema](docs/SCHEMA.md) — Apple JSON field reference

## Consumers

- **MJOLNIR** — Android app clones this repo for offline Apple feed
- **Emily Prime TUI** — `emily watch` tails IDUNA live
- **RSI loop** — reads completion Apples to verify cycle closure
