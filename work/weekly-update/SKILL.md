---
name: weekly-update
description: Drafts a concise, CEO-readable weekly work update from local repos, GitHub, Gmail, and meeting notes. It explains business outcomes and why they matter in plain language. Use when the user asks to write a weekly update, weekly recap, status update, or team update from local work sources.
---

# Weekly Update

## Trigger

Need a weekly recap of shipped work for status updates, retros, or planning. Default to a CEO or other non-technical reader unless the user requests a technical update.

## Executive Framing

- Organize the update around 1-3 objectives, not repositories, commits, or implementation tasks.
- For each material outcome, say what changed and, in one short sentence, why it matters to customers, revenue, delivery speed, risk, or the team's ability to execute.
- Use everyday language. Translate implementation terms into their practical effect; name the technology only when it clarifies the decision or outcome.
- Do not force a "why" onto small fixes. Group minor work under its larger objective instead.

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
3. For active repos, inspect commit stats and subjects enough to identify user-facing outcomes.
4. Check `git status --short` for relevant in-progress work, but never modify, revert, or stage anything.
5. Use `gh` to collect PRs, issues, reviews, and contribution activity involving the user in the date window.
6. Use Gmail/mail tooling to collect sent mail, important inbox threads, and prior weekly updates.
7. Read the most recent sent weekly update preceding the reporting window with `--preview`; turn its reported outcomes into an exclusion list.
8. Read selected email bodies only when subjects indicate work relevance; use `--preview` with `himalaya message read` to avoid marking messages seen.
9. Look for Granola notes through local app data, a Granola CLI, or web access if available. If unavailable, state that separately instead of inventing meeting content.
10. Synthesize the work by objective, not by source or repository. For each objective, identify the outcome and its brief business reason, then merge duplicate signals and remove outcomes already reported last week.
11. Retain a follow-up only when it has a material new outcome, decision, release, or resolution; describe only the delta.
12. Produce a sendable draft first, then a short source-coverage note outside the email body.

For each git project, map meaningful changes to an objective. Keep bug-fix, tech-debt, and net-new classifications in source coverage only; do not put them in the email unless they help a non-technical reader understand the outcome.

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

**Objective: Make [product or service] more useful**
- Outcome. This matters because [brief business reason].

**Objective: Reduce [delivery, customer, or business risk]**
- Outcome. This matters because [brief business reason].

**What's remaining / next**
1. Next item.
2. Next item.

Best,
David
```

## Guardrails

- Keep the recap short and executive-readable. Prefer two to five objectives and one to two bullets per objective.
- Lead with the outcome, not the implementation. Avoid unexplained acronyms, code names, ticket IDs, commit names, and infrastructure details.
- State why an outcome matters only when the connection is material and clear. Keep it to one plain-language sentence.
- Base claims only on collected sources; do not invent outcomes or work.
- Do not include confidential details, tokens, invoice IDs, private links, or personal email contents unless needed for the update.
- If a source requires login, network access, or unavailable local files, ask the user to provide access. Do NOT continue.
- If prior weekly updates exist, preserve the user's voice and formatting conventions.
- Do not repeat a completed outcome from the most recent sent weekly update. Include it only if the current week produced a material change, decision, release, or resolution, and state only that change.
- Do not mention meetings or 'had a discussion with X' unless the meeting produced a material outcome or decision. If it did, summarize the outcome, not the discussion.
