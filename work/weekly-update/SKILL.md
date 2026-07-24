---
name: weekly-update
description: Drafts a weekly work update by synthesizing local repos, GitHub activity, Gmail, meeting notes, and Granola when available. Use when the user asks to write a weekly update, weekly recap, status update, or team update from local work sources.
---

# Weekly Update

## Trigger

Use this when the user wants a sendable weekly update from work signals across code, GitHub, email, meeting notes, or Granola.

## Defaults

- Time window: current work week if obvious, otherwise the last 7 days.
- Primary local workspace: `~/dev` unless the user specifies another path.
- Local Git author: `git config --global user.email`, plus repo-local config if needed.
- GitHub actor: `gh api user` or the authenticated `gh` account.
- Email account: configured local mail CLI first, especially `himalaya`; read messages in preview mode only.
- Output style: mirror the user's most recent sent weekly updates when available.

## Workflow

1. Discover sources without changing state.
2. List Git repos under `~/dev` and collect authored, non-merge commits in the date window.
3. For active repos, inspect commit stats and subjects enough to group user-facing outcomes.
4. Check `git status --short` for relevant in-progress work, but never modify, revert, or stage anything.
5. Use `gh` to collect PRs, issues, reviews, and contribution activity involving the user in the date window.
6. Use Gmail/mail tooling to collect sent mail, important inbox threads, and prior weekly updates.
7. Read the most recent sent weekly update preceding the reporting window with `--preview`; turn its reported outcomes into an exclusion list.
8. Read selected email bodies only when subjects indicate work relevance; use `--preview` with `himalaya message read` to avoid marking messages seen.
9. Look for Granola notes through local app data, a Granola CLI, or web access if available. If unavailable, state that separately instead of inventing meeting content.
10. Synthesize the work by outcome, not by source. Merge duplicate signals from commits, PRs, and email, then remove outcomes already reported in last week's update. Retain a follow-up only when it has a material new outcome, and describe the delta rather than repeating the original work.
11. Produce a sendable draft first, then a short source-coverage note outside the email body.

## Useful Commands

```sh
git config --global user.email
gh auth status
gh api user --jq '.login + " " + (.name // "") + " " + (.email // "")'
himalaya -o json account list
himalaya -o json folder list
```

For local repo collection, prefer a small script that loops over immediate children of `~/dev` and runs `git log --since=<date> --until=<date> --no-merges --author=<email> --date=short --pretty=format:'%h%x09%ad%x09%an%x09%s'` in each repo.

For Gmail, prefer date-bounded envelope searches such as:

```sh
himalaya -o json envelope list --folder '[Gmail]/Sent Mail' --page-size 100 'after YYYY-MM-DD and before YYYY-MM-DD order by date desc'
himalaya -o json envelope list --folder '[Gmail]/Sent Mail' --page-size 100 'subject "Weekly Update" before YYYY-MM-DD order by date desc'
himalaya message read --preview --no-headers --folder '[Gmail]/Sent Mail' <ids>
```

## Email Draft Shape

```text
Subject: Weekly Update

Hi everyone,

Here's my weekly update.

**Area 1**
1. Outcome.
2. Outcome.

**Area 2**
1. Outcome.
2. Outcome.

**What's remaining / next**
1. Next item.
2. Next item.

Best,
David
```

## Guardrails

- Keep claims grounded in collected sources.
- Do not include confidential details, tokens, invoice IDs, private links, or personal email contents unless needed for the update.
- Keep the sendable draft concise and executive-readable.
- Mention unavailable sources in a source note, not inside the sendable email body.
- If prior weekly updates exist, preserve the user's voice and formatting conventions.
- Do not repeat a completed outcome from the most recent sent weekly update. Include it only if the current week produced a material change, decision, release, or resolution, and state only that change.
- If a source requires login, network access, or unavailable local files, continue with available sources and note the gap.
