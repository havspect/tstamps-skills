# Deriving entries from agent history

Use this when the user asks to log time spent working with a coding agent:
"log what I did this morning", "turn yesterday's agent work into hours".

## 1. Get the threads

In Amp, threads live on ampcode.com, not on local disk, so read them through the
CLI:

```sh
amp threads list            # recent threads with titles and times
amp threads search <query>  # find by text, label, project
amp threads markdown T-…    # one thread as Markdown, including tool calls
```

Ask for the date range before searching, and search inside that range only.
`amp threads list` is the cheap call; `markdown` the two or three threads that
actually match the day.

Other harnesses keep their own transcripts. If the user's harness has an export
command or a log directory, use it the same way. If it has neither, say so; do
not reconstruct a day from memory.

## 2. Read for time, not for content

A thread records what the agent did, not when the human worked. Use the
timestamps on messages and tool calls:

- Group activity into spans separated by more than 30 minutes of silence. That
  gap is almost always a meeting, lunch or a different task.
- A span starts at the first human message or tool call and ends at the last
  activity before the gap. Do not extend a span to "fill" the day.
- Threads that ran unattended (a scheduled run, a background agent) are not
  human work. Leave them out unless the user says otherwise.
- A thread that produced nothing usable still took time; let the user decide,
  but do not silently drop it either.

## 3. Map to projects

The thread's title, the repository it touched and the files it changed are the
evidence. Ask the user to map a thread to a project when the title could mean
two projects; propose the mapping for the whole set at once.

Write the note from the work, not from the agent's chatter: "OAuth callback and
the retry path", not "agent explored the codebase". The note describes what the
user got done.

## 4. Propose, then log

Show a table of date, project, start–end, timezone and note, plus hours per day
and the threads each row came from. Flag overlaps with existing entries from
`list_entries`, and flag any span that looks like idle time. Log only after the
user confirms; then report what was created and stop.

The workspace's timezone rules still apply: use the zone the human worked in,
not the zone of the machine that ran the agent, when they differ.
