# Deriving entries from a calendar

Use this when the user says things like "log my day", "what did I do on
Tuesday", or "fill in last week from my calendar".

## 1. Get the events

Use whichever calendar source the user has connected: a calendar MCP server, a
CLI, or an export they paste into the conversation. Do not invent events and do
not assume a source exists. Ask for the range you need first, usually the days
the user named and nothing wider.

Ask for, or read from the source: summary, start, end, all-day flag, attendees,
location, and the event's timezone. The timezone matters more than the title.

If the calendar is Outlook or Microsoft 365 and no source is connected, say so
and stop. Do not guess the user's day from memory.

## 2. Decide what counts

- **Meetings** become entries on the project they served. A recurring team
  stand-up is one project; a customer call is the project that customer's work
  belongs to. When the mapping is not obvious, ask — one question for the whole
  table, not one per event.
- **Focus blocks** ("Deep work", "No meetings") are evidence, not entries. They
  say time was reserved, not that work happened. Do not log them on their own.
- **All-day events and out-of-office** are not work. Skip them, and mention what
  you skipped.
- **Declined, tentative or cancelled** events are not work. If the status is
  unknown, ask.
- **Personal events** (lunch, school run) are not work unless the user says the
  workspace counts them.

## 3. Shape the candidates

- Use the event's own timezone, not the machine's. For a meeting booked across
  zones, the zone the invite states is the right one.
- Keep the event's real start and end. Do not round to the nearest quarter
  unless the user asks; rounding is what makes a timesheet drift from the
  calendar.
- Merge back-to-back events on the same project into one entry only when the
  gap is under five minutes, and say that you merged them.
- Do not fill gaps between meetings with guessed work. A gap stays a gap until
  the user fills it or an agent-history reference explains it.
- Notes: the event title, trimmed. Never paste attendee lists, dial-in numbers
  or descriptions into a note.

## 4. Propose, then log

Show the whole table first: date, project, start–end, timezone, note, and the
hours it adds per day. Flag anything that overlaps an existing entry from
`list_entries`, anything that would exceed 24 hours, and anything that lands on
an archived project.

Log only after the user confirms, and log the confirmed rows only. If they edit
the table, restate the edited rows before writing. After writing, report what
was created by date and total hours, and stop.
