# General instructions

Use the parent model primarily for planning, routing, review, and synthesis.
Optimize for low wall-clock latency, high correctness, low usage, and a clean
parent context.

The parent should avoid routine execution when a bounded subagent can perform
it without materially increasing latency. However, delegate coherent
workstreams rather than individual phases or tool calls.

## Parallelism

Split genuinely independent workstreams into parallel subagents when doing so
is likely to shorten the critical path. Parallelism is encouraged for independent
research, repository investigation, analysis, and non-conflicting implementation.

Do not parallelize work merely because it contains multiple steps. Avoid
parallel workstreams that mutate the same files/state, substantially duplicate
investigation, contend for expensive shared resources, or require one another's
results.

Prefer one worker for a tightly coupled sequence such as:
inspect -> implement -> focused test -> diagnose -> fix.

Choose fan-out based on independence and expected latency benefit rather than
a fixed worker count.

## Model routing

Model tiers are a local routing policy, not a ranking of providers.

### Model index

| Tier | Purpose | Claude | OpenAI |
| --- | --- | --- | --- |
| weak | Simple read-only search, log reading, summaries, and extraction | Claude Haiku 5.5 @ `medium` effort | GPT-6 Luna @ `medium` effort |
| medium | Scoped bug fixes and spec-driven implementation | Claude Sonnet 5.5 @ `medium` effort | GPT-6.1 Sol @ `medium` effort |
| strong | Main-agent engineering, uncertain work, and complex reasoning | Claude Opus 5.5 @ `medium` effort | GPT-6.1 Sol @ `xhigh` effort |

In Codex, select from the OpenAI column. In Claude Code, select from the Claude
column. An explicit user selection takes precedence. For other agents, use
the configured provider and select an available model that fits the tier.

Choose the tier that fits the task from the start. Move from weak to medium
when the work needs implementation or judgment beyond simple reading. Move
from medium to strong when ambiguity, architectural decisions, difficult
debugging, or repeated failures require deeper reasoning. Skip directly to the
required tier when the need is clear.

Before escalating a failed attempt, ensure the worker can verify its results
with an appropriate test, build, or other concrete check. If the current tier
still cannot make reliable progress, move up rather than repeating the same
attempt. Use the model and effort setting listed for the new tier; escalation
may change the effort setting on the same model.

When escalation requires a new worker, pass a concise handoff with the task,
findings, failed checks, and remaining problem.

Respect the user's chosen provider and model, the models that are available,
and the supported reasoning settings. If the user chose a provider, do not
switch to another provider automatically.

## Workstream lifetime

Give every subagent a distinct, bounded objective.

A worker owns its workstream through completion, including necessary
exploration, tool use, implementation, focused verification, retries, and
tightly related fixes.

Keep the same worker on its bounded workstream through inspection,
implementation, focused verification, and related fixes so it can retain useful
local context.

Once a workstream is complete and its result has been returned, close that
worker. Do not repurpose completed workers for unrelated work. Create a fresh
worker for a new workstream.

## Context isolation

Give workers only the context necessary for their objective:
- objective and acceptance criteria
- relevant constraints
- relevant prior findings
- likely files/symbols/commands
- required verification

Do not copy the full parent transcript, unrelated project history, large logs,
or unrelated worker results into a subagent unless necessary.

Workers may accumulate detailed local context while executing. Keep that
detail inside the worker whenever possible.

Workers should return only:
- conclusions
- essential evidence
- changed paths
- verification results
- unresolved issues or risks

Do not return progress narration, full logs, tool transcripts, failed attempts,
or large file contents unless they are necessary to understand an unresolved
problem.

## Verification

Workers should perform focused verification appropriate to their mutations.

Do not have multiple workers redundantly run the same expensive full build,
full test suite, server lifecycle, or integration validation unless independent
verification is specifically justified.

Prefer cheap/focused tests during implementation and one integration-level
verification after relevant workstreams have converged.

Once appropriate verification passes, do not broaden or repeat it without a
concrete reason.

## Parent review

The parent MUST reconcile completed worker results before finalizing.

Review the relevant changes and evidence for correctness, maintainability,
unnecessary complexity, duplication, edge cases, security where relevant, and
adequate verification.

Make review proportional to the scope and risk of the change. Do not redo
worker exploration or successful verification without a specific concern.

If review finds a trivial, bounded correction that is clearly faster to perform
directly, the parent may make it. Delegate substantial or uncertain revisions
as focused workstreams.

## Waiting and event handling

Avoid repeated parent status checks.

Do not repeatedly call status tools, wait tools, or equivalents merely to
observe unchanged state. Polling creates unnecessary work, context growth, and
latency.

Use available delegation tools when they fit the task. If delegation tools are
unavailable, do the work directly. After assigning work, continue any useful
independent planning or review that does not duplicate worker execution.

When otherwise idle, prefer event-driven waiting and use the longest supported
wait.

Do not shorten waits merely to check whether workers are still running normally.

When a wait returns, process all completed results already available before
waiting again. Schedule newly unblocked work immediately.

If required workers remain active and nothing else is useful, wait for an event.

Only perform a status check when there is evidence of abnormal delay, failure,
deadlock, or a worker waiting on something the parent may need to resolve. Do
not check status merely because time has elapsed normally.

The parent MUST NOT finalize while required workstreams are still running.

## Hierarchy

Prefer a shallow hierarchy:

parent
  -> parallel bounded workers
  -> tools

Use a stronger model tier or additional hierarchy only when it clearly
improves the critical path or result quality.

## Code discovery

Before broad source reading, choose the matching skill, then inspect relevant code:

- If behavior or its location is unclear, use `semble` to find likely code.
- If likely files are known and you need a map of declarations, imports, exports, or members, use `outline`.
- If the search depends on syntax or code shape, use `ast-grep`.

Use `rg` for known names or exhaustive literal searches. Read directly when the file and needed range are already clear.

## Attribution

I claim all your work. Do not append yourself to commit messages or pull requests.

Use conventional commit messages.
