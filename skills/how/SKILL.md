---
name: how
description: "Use when you need to explain how code works, walk through a code path, or clarify where behavior belongs in a system. Build a practical mental model of architecture and runtime flow."
---

# How

Answer codebase questions such as “how does this work?”, code walkthroughs before a change, and placement, ownership, or layering questions. Explain the architecture and runtime flow at the level of a senior engineer onboarding to the subsystem: enough to build a working mental model without annotating every line.

## 1. Assess scope

If the question is ambiguous, state your interpretation and explore; the user can redirect.

- **Simple:** One module, a small utility, or a narrow question. If delegation is available and appropriate under the user's instructions, use one explainer to inspect and answer in one pass, using `references/explainer-prompt.md` without the explorer-findings section. Otherwise, inspect and answer directly.
- **Complex:** A subsystem spanning multiple files or services, a cross-cutting feature, or a broad architecture question. When delegation is available and appropriate, split the work into 2 to 4 distinct angles, have explorers investigate them in parallel using `references/explorer-prompt.md`, then have one explainer synthesize their findings using `references/explainer-prompt.md`. Ask explorers and explainers to keep their work read-only. If delegation is unavailable or inappropriate, explore and synthesize directly.

When unsure, use the simple path. Do not delegate if the user or applicable instructions prohibit it.

## 2. Present

Present the explanation clearly, with light edits for clarity or relevant context. Include specific files and functions so the reader knows where to continue. Use the sections in `references/explainer-prompt.md` that apply: Overview, Key Concepts, How It Works, Where Things Live, and Gotchas.
