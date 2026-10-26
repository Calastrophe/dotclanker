---
name: why
description: Use when you need to investigate why code or a design decision took its current shape by tracing source history and relevant connected issue trackers, documents, chat, observability, error tracking, or analytics. Return a cited account that separates documented reasons from inference and gaps.
---

# Why

Investigate the motivation and intent behind code. The companion `how` skill answers what the code does and how it works. `why` answers what forces led to its shape.

Use the tools and connected apps exposed in the current Codex session. When subagents are available and useful, delegate source investigations to them; otherwise search the sources directly. Keep investigation read-only. The parent investigator synthesizes and spot-checks the findings.

## Operating Posture

Operate as a **careful, cautious, and precise investigator**. Be honest about what you know vs what you're inferring. Read `references/epistemics.md` for the full confidence framework and phrasing guide. The synthesizer must follow it.

## Step 1. Understand the Target and the Question

The **target** is usually a chunk of code, a pattern, a feature, or a named design decision. The **question** is usually a design rationale, a tradeoff, a motivating edge case, an external constraint, dead code, or a broad history sweep.

If the target is vague ("why do we do it this way?" with no clear referent), make your best guess from the conversation and repository context (open files, recent edits, what was just discussed). State your interpretation briefly so the user can redirect if you're off, then proceed.

## Step 2. Establish the Code Anchor

Before delegating, build the seed context inline: file paths and line ranges, key symbols (functions, classes, constants), relevant commits, PR numbers from merge commits (pattern `(#1234)` in the subject line), and any linked ticket IDs. Use `git blame -L <start>,<end> <file>` for last-touch commits, `git log --follow -p -- <file>` for history through renames, and `git log --oneline -20 -- <file>` to find relevant commits. Extract PR numbers with `git log -1 --format=%B <commit>` and inspect PR details with `gh pr view` when GitHub CLI is available and authenticated. Share the anchor with each investigator and the synthesizer.

## Step 3. Spawn Parallel Investigators

Investigate the evidence sources that are available and plausibly relevant to the question. Prioritize sources that could explain this target's history; broaden the search when the rationale is significant or initial findings are thin. Record searches that return nothing and explain any meaningful gaps. A complete sweep of every category is not required for every question.

### Discovery

Before delegating, inspect the available tools and connected apps in this Codex session. Classify relevant integrations by the evidence they can provide; do not assume a connector is available just because it is named below.

Map each available MCP to one evidence category:

1. Source control history
2. Issue / ticket tracker
3. Long-form documents
4. Real-time team chat
5. Infrastructure observability
6. Error / exception tracking
7. Product analytics warehouse

Search local git history when a repository is in scope, and use `gh` only when installed and authenticated. For connected sources, classify tools by their names, descriptions, and schemas. Treat tool names in the playbooks as examples; map them to available equivalents. If no equivalent is available, record that category as a gap. If an integration could fit more than one category, choose the one matching its primary evidence and note ambiguity.

When delegation is supported and useful under the current `AGENTS.md` and task constraints, use one investigator per selected evidence category, with each worker owning a coherent, bounded source search. Dispatch independent searches concurrently when the agent tools allow it. Let workers use the configured default model unless the user or local project instructions specify otherwise. Otherwise investigate directly. Avoid duplicating searches or spawning workers for categories that are unavailable or immaterial.

Each investigator's prompt is `references/investigator-prompt.md` filled in, plus:
- the category playbook `references/sources/<source>.md` for the selected MCP, adapted from the examples in `references/source-playbook.md`
- `references/sources/incident-postmortem.md` **if the target code looks defensive** (null checks, retry logic, timeout handling, rate limiting, feature flags, egress guards, OOM handlers)
- the code anchor and the user's original question

### Evidence categories

Each category below can surface a different kind of "why." Use it to identify relevant sources and to describe meaningful gaps.

1. **Source control** (git history, `gh` for PRs when available, code comments, tests). Best at *implementation-time rationale captured during review*.
2. **Issue / ticket tracker** (e.g. Linear, Jira, GitHub Issues, Plane, Shortcut MCP). Best at *the product or business forcing function*. Strongest when the why is external to engineering.
3. **Long-form documents** (e.g. Notion, Confluence, Google Docs, Coda MCP). Best at *long-form design rationale*, written out before it becomes code.
4. **Real-time team chat** (e.g. Slack, Discord, Microsoft Teams, Mattermost MCP). Best at *real-time deliberation that never reached a doc*. Especially important when the source control, ticket, and doc paper trail is thin.
5. **Infrastructure observability** (e.g. Datadog, New Relic, Honeycomb, Grafana, Splunk MCP). Best at *infrastructure and runtime reality that motivated the code*. Strongest when the target reacts to an infra signal (timeouts, retries, rate limits, circuit breakers).
6. **Error / exception tracking** (e.g. Sentry, Rollbar, Bugsnag, Airbrake MCP). Best at *the specific exceptions and error trajectories that motivated defensive or corrective code*. Strongest for catch blocks, null guards, type checks, retries, and other defenses.
7. **Product analytics warehouse** (e.g. Databricks, Snowflake, BigQuery, ClickHouse, dbt, Redshift MCP). Best at *product and data reality that shaped the code*. Strongest for flag-gated code, experiment-driven ships, data migrations, and "where did this number come from" questions.

### When to skip an investigator

Record a concise reason for each material source that was not searched, and include meaningful gaps in Sources Consulted. Common reasons include:

- **No connected source is available** in this environment. Flag this as a gap, for example: "Real-time team chat was unavailable, so the conversational record was not searchable."
- **The source is outside the question's scope or immaterial** to the target. Briefly explain why, for example: "Error tracking was not searched because the target is a build-time script with no runtime code path."

For a small question with an explicit answer in a directly relevant source, keep the investigation proportionate and state what evidence supports the answer.

## Step 4. Synthesize

Synthesize the findings in the parent conversation. Include any null results and categories skipped with justification, the code anchor, the user's original question, and the guidance in `references/epistemics.md` and `references/synthesizer-prompt.md`. Spot-check citations in the repository or connected source when possible.

## Step 5. Present

Present the synthesizer's output to the user. You may lightly edit for clarity or add context from the conversation, but **do not rewrite the confidence language**.

## Output Format

The structure is the one in `references/synthesizer-prompt.md`: The Question, The Code in Question, What We Found, What We Can Reasonably Infer, Competing Hypotheses, What We Don't Know, Sources Consulted, Confidence Summary. Adapt as needed, but keep the confidence separation intact, and keep Sources Consulted as one line per investigator, including the ones that returned nothing or were skipped, with the reason.

After Sources Consulted, if the user's `why` question is a precursor to changing this code, convert the lineage findings into a Preserve / Change / Avoid / Risk constraint set for planning the change.

## Common Failure Modes to Avoid

- **Recency bias**. Assuming the most recent commit is authoritative. The current shape is often the accretion of many earlier decisions. Trace back.
