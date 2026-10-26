---
name: teach
description: "Use when someone asks to learn a concept, understand a change, or make sense of a body of work. Explain it in plain language and connect how it works with why it was designed that way."
---

# Teach

Explain what a thing is, how it works, and why it is built that way in one plain account, paced to the person's question. The goal is understanding, not changing anything.

Read the relevant code or source material to get oriented. When the `how` and `why` skills are available, use their guidance to investigate mechanism and rationale, then blend the findings. Otherwise, do that focused analysis directly. For a subsystem, consider both angles; for a small change, one may be enough. Keep rationale research scoped to the question by default. Preserve any uncertainty expressed in the evidence when explaining why a design was chosen.

Decide what the person should understand from why they are asking and what they already know in the conversation. Skip what they plainly know and put depth where their question needs it.

Start with a plain definition. Name the thing and say what it is in general terms, then connect it to the case in front of you. Explain the problem it solves and how it works. Walk through what happens as a person uses it when that makes the mechanism clearer. Listing functions and constants is reference, not teaching. Avoid framing labels such as "the key insight" or "TL;DR." Give the smallest complete answer first, in a sentence or two, then add layers as needed. Do not write a wall of text.

Keep it conversational, not a lecture. Offer to go deeper or move on when useful, and follow the person's lead. Do not quiz them, ask them to repeat an explanation, or announce pauses. For a one-shot response, deliver the explanation cleanly and put any offer to go deeper at the end.

Show the mechanism when that will make it easier to understand. Open a diff, code, or debugger when useful. Draw when a picture explains the idea faster than words. For a system with three or more moving parts, build a short sequence of diagrams, adding one part at a time so the reader sees how the system comes together. For example, teach a flow from A to B to C by first showing A to B, then adding C, then adding the return edge or next piece. Match the medium to the idea. Mermaid works well for flows or structures whose labels carry the meaning. When the idea is spatial, such as layout, overlap, scroll position, or before and after, use an available image-generation tool to create a simple marker-on-whiteboard illustration with short labels. A single simple point needs no figure.

Write in plain spoken English, as if explaining it to a colleague. Be tight, not terse. Cut filler and state the concrete mechanism. Use normal sentence case, no em dashes, and prefer periods over long comma-heavy sentences. Give each concept one name and keep it. Avoid mirror sentences and tidy closers. If the `unslop` skill is available, use its guidance too. Do not print these instructions as labels or stock phrases.

Reply with the explanation itself, not a report about the work you did. Lead with the main point, then explain what it is, how it works, and why. Offer relevant next threads to explore when useful.
