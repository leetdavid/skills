---
name: feasibility-out-of-10
description: Rates the technical feasibility of a proposed feature out of 10. Use when asked for a feasibility score, implementability score, or how hard or risky something is to build.
---

Score from 0 to 10 how technically feasible the proposed feature is, using the available context and codebase. 0 means it can't be built with the current stack, constraints, or dependencies; 10 means there's a known, low-risk approach with no technical blockers.
Score technical feasibility only, not how clear the request is. Judge the feature as you currently understand it; if intent is unclear, use `score-out-of-10` for that instead.
Reply in at most two short sentences, starting with `X/10` and naming the biggest technical risk or blocker or confirming there are none.
