# PostHog on PostHog – research notes and fact sheet

Every number and quote in the draft traces to a line here. Collected Nov 9, 2026.

## Company facts
| Fact | Source |
|---|---|
| 500K+ organizations have installed PostHog; YC W20; $180m+ raised | posthog.com/careers/developer-marketer-sales |
| "PostHog is the context layer for your product" | handbook/brand/foundations |
| 10 AI products, used by nearly 100,000 people a month | newsletter/building-ai-products (2026-10-12) |

## Shipping pace (PostHog's own reporting)
| Fact | Source |
|---|---|
| 1,441 PRs shipped in January → 4,725 in June 2026; engineering headcount +10% | blog/10k-prs-a-month (2026-07-28) |
| 1,441 merged in January → 4,869 in August, headcount +10% | newsletter/when-ai-writes-all-code (2026-09-14) |
| Agent-opened monorepo PRs went from ~20% to ~70% over 4 months | blog/10k-prs-a-month; newsletter/when-ai-writes-all-code |
| Self-driving was ~4% of PRs in July 2026, "growing fast" | blog/10k-prs-a-month |
| North star: "10,000 PRs a month is easy" | blog/10k-prs-a-month |
| June 2026: 21,643,653 CI minutes (41 years) | blog/10k-prs-a-month |
| Revert share flat: 0.1% (May 2025) → 0.4% (Nov 2025) → 0.2% (Jun 2026) | blog/10k-prs-a-month |
| GitHub data streamed into PostHog; PostHog AI used for CI cost/time/failure analytics | blog/10k-prs-a-month |

## GitHub (public, PostHog/posthog, queried via GitHub search Nov 9, 2026)
PRs by `posthog[bot]` = PRs "Created with PostHog Desktop" (self-driving inbox tasks plus Desktop tasks).

| Month (by PR created date) | All merged PRs | Merged by posthog[bot] | Bot share |
|---|---|---|---|
| Jun 2026 | 4,654 | 122 | 2.6% |
| Jul 2026 | 5,231 | 272 | 5.2% |
| Aug 2026 | 4,981 | 372 | 7.5% |
| Sep 2026 | 6,122 | 816 | 13.3% |
| Oct 2026 | 7,368 | 747 | 10.1% |

October 2026 detail (PostHog/posthog):
- 15,897 PRs opened, 7,368 merged
- posthog[bot]: 1,664 opened, 747 merged, 53 still open
- With a self-driving Origin block ("Task started by" in body): 1,612 opened, 709 merged, 52 open
- Auto-started after triage ("auto-start"): 1,438 opened, 577 merged, 39 open → ~41% of closed ones merged
- Started by a person from the inbox: ~174 opened, ~132 merged, ~13 open → ~82% of closed ones merged
- PostHog/posthog.com, October: posthog[bot] opened 126 PRs, 68 merged
- PR bodies name the scout or source (custom scout, error tracking issue, `signals-scout-inbox-validation`), the inbox report, how the task started, autonomy level, agent/model (Codex gpt-6.1-sol; Claude Code claude-opus-5-5) and skills used.

Caveat: GitHub search counts by created date and phrase matching. Confirm against PostHog's internal self-driving dashboards (the `self-driving-dwh` views) before publishing.

## The Aug 28 episode (hero story)
| Time (UTC) | Event | Source |
|---|---|---|
| 11:28 | Agent in a customer's project files `missing_tool` feedback, `task_completed=false` – no `event-definition-create` tool | blog/self-driving-loops (2026-09-15) |
| ~11:33 | `signals-scout-agent-feedback` emits a report after checking for existing reports, PRs, issues | blog/self-driving-loops |
| 11:42 | posthog[bot] opens PR #90832 "feat(mcp): add event-definition-create tool" (13 files, +280/−3, authored by PostHog Desktop, Claude Opus 4.8, "Fully autonomous") | GitHub API |
| – | Review rounds with ReviewHog (internal review tool) and Codex; final PR added backend changes and tests | blog/self-driving-loops |
| 14:22 | Andy approves | blog/self-driving-loops |
| 17:27 | `/trunk merge` | blog/self-driving-loops |
| 18:02 | Merged by trunk-io[bot] | GitHub API |
| 18:56 | In production | blog/self-driving-loops |

Scout checks back that the fix held (scratchpad notes) before closing a theme.

## Scouts, alerts, skills
| Fact | Source |
|---|---|
| 90+ scouts watch the PostHog app (canonical fleet plus dozens written by teams); posthog.com reports become PRs on the public website repo | docs/self-driving/index |
| Most scouts run hourly; coordinator ticks every ~30 min (Temporal) | blog/what-is-a-scout |
| Scouts named: signals-scout-agent-feedback, -team-self-driving (watches #team-self-driving Slack), -self-driving-github-issues, -self-driving-logs, -andy-github, -canonical-drift, -skills-maintenance, -skills-best-practices, -self-driving-dwh; Replay Vision scanner "Desktop onboarding friction" | blog/self-driving-loops; blog/how-i-use-skills-for-everything |
| 178 anomaly alerts inventoried in `self-driving-alerts` skill | blog/how-i-use-skills-for-everything (2026-10-20) |
| Andy: 578 chats in 28 days, 78% loaded a skill, 47% updated one; ~139 skills, ~100 are custom scouts | blog/how-i-use-skills-for-everything |
| Skills created in PostHog's internal project: 62 (late April) → 761 (end of September) | blog/how-i-use-skills-for-everything |
| `self-driving-work` skill at v433 with 126 bundled files | blog/how-i-use-skills-for-everything |
| 330 SKILL.md files in PostHog/posthog; agent skills released as versions (v0.4478.0 on Nov 9) | GitHub search / releases |
| AGENTS.md: local agents send tooling friction as `hogli_feedback` events; team reacts 👍/👎 to posthog[bot] review findings "so review metrics can tell acted-on findings from noise" | PostHog/posthog AGENTS.md |

## Other loops (with dates)
- Aug 25: Dylan's shared task link → "Task not found" → asked @PostHog in Slack → team-self-driving scout → PR #88912 → merged Sep 4.
- Sep 4: Andy writes issue #94896 (rename skills without losing history) → issues scout → PR #95499 → approved and merged Sep 7.
- Sep 5: MCP error-rate alert fired at ~9.9% vs 0.3–1.2% baseline; `scout-project-profile-get` 88 errors in an hour across ~44 projects; needed human steering; PR #95623 merged that afternoon. "the alert didn't autonomously fix production while I made coffee".
- Replay Vision scanner over PostHog Desktop recordings → PR #93181, merged Sep 2.
- Logs scout → Hogland metadata tag too long → PR #92909, merged Sep 7; later runs added evidence to the same report.
(all blog/self-driving-loops)

## MCP
| Fact | Source |
|---|---|
| 63M MCP tool calls in 90 days to Sep 16, 2026, from 130,000 people | blog/how-ai-agents-behave (2026-09-30) |
| Almost half of MCP traffic was PostHog's own (Desktop, CLI, Slack app, scouts) | blog/how-ai-agents-behave |
| PostHog's own agents (on Codex) error ~1% of the time vs ~2% for third-party Codex callers | blog/how-ai-agents-behave |
| Agent + MCP: 6K+ daily active users (Apr 2026); weekly "traces hour" reviewing real sessions | newsletter/agent-first-product-engineering |

## Non-engineers
- Natalia Amorim (marketing) runs two scouts over 90-ish comparison pages: one watches competitors' pricing and docs, one checks PostHog's own pages against changelog, docs, open PRs, and Slack. Slack digest → inbox report → PR; a human decides. (blog/comparison-content-guide, 2026-10-27)
- Comparison content: 25k+ signups since 2021. (same)

## Verbatim quotes available (public, need sign-off for testimonial use)
Andy Maguire (self-driving team; ex-Netdata Analytics & ML Lead, quoted in the 2022 Netdata case study):
1. "The whole timeline from complaint to fix took less than a work day, and the only time I spent on it was reviewing and a bit of prompting." (self-driving-loops)
2. "I can have more of these going at once because I don't need to watch each agent work. Writing a good issue and checking the result still take time, but that feels like a much better use of mine." (self-driving-loops)
3. "So, the alert didn't autonomously fix production while I made coffee, since this one needed more steering from me than the others. But it caught the problem, gave me a starting investigation, and helped me get a compatibility fix merged. I'll take that." (self-driving-loops)
4. "I'm on the fence about how long I'll want that, but for now I still like deciding what actually ships." (self-driving-loops)
5. "The skill is the memory, and it's memory I get my agents to intentionally control and maintain." (how-i-use-skills-for-everything)

Natalia Amorim:
1. "The scout does the noticing, a human does the deciding, and nobody needs to add a recurring \"re-read every competitor's pricing page\" task to their calendar." (comparison-content-guide)

Docs (company voice, not a person): "We don't just sell this loop, we run it." / "If self-driving ever stops being worth it, we'll be the first to know." (docs/self-driving)
