---
name: tstamps
description: Log, correct and read time in a Tstamps workspace. Use when the user asks to start or stop a timer, log or backfill hours, fix a wrong entry, or read hours by project, person, day, ISO week or month. Also use when deriving entries from a calendar, a meeting list or past agent work. Needs the `tstamps` CLI installed and logged in.
---

# Tstamps

Tstamps is team time tracking. People log **entries** against **projects**; a
**running entry** is an entry with a start and no end yet. Hours are the
company's timekeeping record, so a wrong write is worse than a missing one.

## Before the first write

The workspace is reached through the `tstamps` MCP tools, or through the
`tstamps` CLI when the tools are not available but you have a shell. Both use
the config written by `tstamps login`; never ask for, echo or store a token
yourself. Check the connection with `whoami` / `tstamps whoami` before
proposing anything, so a bad token is not mistaken for an empty workspace.

If neither works, stop and tell the user to install the CLI and run
`tstamps login --url <workspace>`.

## Rules an agent gets wrong

- **The entry's timezone decides the day.** Every entry stores the IANA zone it
  was logged in, and that zone, not the user's profile zone and not the
  viewer's, decides which day, ISO week and month the hours fall into. Pass the
  zone the work actually happened in (`Australia/Sydney`, not `+11:00`). When
  the user's day and the entry's day differ, trust the entry.
- **One running entry per person.** A second start is refused with the running
  entry in the error. Stop or correct that one instead of starting another.
- **Dates are `YYYY-MM-DD`, `from` inclusive and `to` exclusive.** Both default
  to the current ISO week, Monday to Sunday. Say the range out loud when
  reporting, because "last week" is ambiguous to humans and not to the API.
- **Archived projects reject new entries.** Their old hours still count in
  reports. Pick a live project or ask.
- **Overlaps are allowed, not corrected.** If a new or moved entry overlaps an
  existing one, the timeline marks it and the API accepts it. Mention the
  overlap; do not silently delete the other entry.
- **A finished entry is at most 24 hours** and may not start more than five
  minutes in the future. If stopping would exceed 24 hours the API refuses and
  leaves the entry running: ask for the end time.
- **Permissions are not retryable.** A member reads and writes only their own
  entries; a lead also reads hours on their teams' projects; an admin reads
  everything. A 403 means explain who can do it, not try again with another
  filter.

## Write policy

Read freely. For every write — create, update, stop, delete — first show the
user a candidate table with project, date, start, end, timezone and note, then
act only after they confirm. This applies to one-line requests too, except when
the user's message already states the exact project and times, in which case
echo them as you log.

Never stop a running entry, delete an entry, or move hours between projects
because it seems tidy. Do those when asked, and say what will happen first.

Nothing here restores anything: only an admin can restore a deleted entry or an
archived project, and only through the admin console or the API.

## Reading hours

Reports group logged time by project and by person over `day`, `week`, `month`
or `none` buckets. Deleted entries are excluded; archived projects are included
and labelled. Members read their own hours, leads read their teams' projects,
admins read everything. Round to minutes when talking to a person; keep seconds
out of summaries unless asked.

## Deriving entries from something else

Two references help turn evidence into candidate entries:

- `references/from-calendar.md` — meetings and focus blocks.
- `references/from-agent-history.md` — what an agent session actually did.

Both end in the same place: a candidate table, then the user's confirmation.

## CLI fallback

When the MCP tools are unavailable and you have a shell:

| Action | Command |
| --- | --- |
| Current user | `tstamps whoami` |
| Projects | `tstamps projects` |
| Running entry | `tstamps ls` — the entry with no end is the running one |
| Start / stop | `tstamps start <project> --note "..."` / `tstamps stop` |
| Finished entry | `tstamps add <project> --from 09:00 --to 11:30 [--date YYYY-MM-DD] [--note "..."]` |
| Entries | `tstamps ls --from <date> --to <date> [--json]` |
| Report | `tstamps report --from <date> --to <date> --by week [--json\|--csv]` |
| Correct a zone | `tstamps edit <id> --timezone <IANA>` |

The CLI sends the machine's zone with a new entry and has no flag to override
it. When the work happened in another zone, create the entry through the MCP
tools (which take `timezone`), or fix it afterwards with `tstamps edit`.
Bare times mean today in the machine's zone.
