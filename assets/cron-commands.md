# Cron jobs — copy-paste starters

Cron runs in a **fresh, self-contained session**. No conversation history, no
implicit CWD context. Every prompt must carry everything it needs.

Set the delivery target first:

```
/sethome
```

## The ladder applies

New job → **read-only + `--deliver local`** → watch it for a day → then let it
write things and message you.

---

## Morning PR digest

```
/cron add "0 8 * * 1-5" "Check ~/projects/api for pull requests opened since yesterday. Summarize each in one line: author, title, files touched. If none, reply [SILENT]." --name morning-prs --deliver telegram
```

## Disk / health monitor (quiet unless broken)

```
/cron add "0 * * * *" "Check disk usage on this machine. If any volume is above 85% full, report which and the top 5 largest directories. Otherwise reply [SILENT]." --name disk-watch --deliver telegram
```

## Weekly memory hygiene

```
/cron add "0 9 * * 0" "Review your memory files. Merge duplicates, drop anything stale or superseded, and report what you changed in under 10 lines." --name memory-hygiene --deliver local
```

## Nightly note summarizer (with a skill attached)

```
/cron add "0 22 * * *" "Summarize today's new notes in ~/notes and file the summary as a daily digest." --name note-digest --skill obsidian-digest --deliver local
```

## Preflight script feeding context in

```
/cron add "0 7 * * *" "Using the metrics in your context, flag anything that moved more than 20% versus yesterday. If nothing did, reply [SILENT]." --name metrics-watch --script ~/bin/pull_metrics.py --deliver slack
```

`--script` runs the Python file first; its stdout is injected into the agent's
context before the prompt runs.

---

## Managing jobs

```
/cron list
/cron run <name>       # run once, now — always do this before trusting a schedule
/cron pause <name>
/cron edit <name>
/cron remove <name>
```

## Flags

| Flag | Does |
|---|---|
| `--name` | Identifier for list/run/pause/edit/remove |
| `--deliver` | `telegram` / `discord` / `slack` / `origin` / `local` / chat-id |
| `--script` | Python preflight; stdout → agent context |
| `--skill` | Attach a skill (stackable) |
| `[SILENT]` | In the prompt body: suppress the message on a no-op |
