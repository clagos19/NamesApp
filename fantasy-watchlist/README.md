# Fantasy Watchlist

A list of hockey (NHL) and football (NFL) players to pull news reports for. It's kept separate from the NamesApp contacts app and is updated through chat with Claude.

All players live in [`players.json`](players.json), grouped two ways so they're easy to tell apart:

- **Sport** — `hockey` or `football`
- **Status** — `drafted` (on one of your fantasy rosters) or `watching` (players you're interested in but haven't drafted)

## Player entry

```json
{
  "name": "Connor McDavid",
  "team": "EDM",
  "position": "C",
  "league": "Work League",
  "notes": "Optional free text",
  "added": "2026-09-29"
}
```

- `league` is the fantasy league the player is on; leave it out for `watching` players.
- `team`, `position`, and `notes` are optional.

## Updating it

Just tell Claude in chat, e.g. "add Connor McDavid to my drafted hockey players in my Work League" or "move Josh Allen from watching to drafted." Claude edits `players.json`, updates `last_updated`, and commits the change.
