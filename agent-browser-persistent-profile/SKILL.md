me: agent-browser-persistent-profile
description: Uses the persistent, isolated browser profile for web work done at explicit direction. Use when a task needs the user's authenticated browser session for a website or personal account.
---

# `agent-browser` Persistent Profile

Use the persistent browser profile at `~/.agent-browser/profiles/user` only for work the user has explicitly authorized. It retains the user's authenticated state between sessions without exposing their everyday Chrome profile.

## Start A Session

```sh
agent-browser --profile ~/.agent-browser/profiles/user --session user open <url>
```

Whenever the user must see or interact with the browser, always add `--headed`:

```sh
agent-browser --headed --profile ~/.agent-browser/profiles/user --session user open <url>
```

## Guardrails

- Act only on tasks the user has explicitly requested.
- Treat submissions, purchases, messages, edits, assignments, and state transitions as external writes; perform them only with explicit user authorization.
- Do not use this profile when a purpose-built CLI or API is available. For Linear, use the `linear` CLI.
- Never expose credentials, session cookies, or profile contents in tool output or responses.
