---
description: Inventory device-specific executables and scripts on a supplied device target
argument-hint: [device target or access]
---

# Device executable inventory

Target or access: $ARGUMENTS

Inspect only the supplied target or access. If neither is usable, stop and ask. Correlate independent observations when available.

Identify executable binaries and scripts using metadata, content, and startup, service, launcher, scheduled-task, package, symlink, and configuration references. Use imports and strings only as supporting evidence. Classify an item as device-specific only when evidence ties it to the device's hardware, configuration, services, or installed functions; do not infer its role from a path or name alone. Record the evidence for each candidate.

Classify candidates as device-specific executables/scripts, libraries, routine system components, or uncertain. Keep device-specific libraries classified as libraries and weak candidates uncertain.

Return only concise Markdown in this order:

- `## Device-specific executables and scripts`: table `Path | Kind | Evidence | Confidence`; kind is `binary` or `script`.
- `## Uncertain candidates`: same table, or `none`.
- `## Excluded`: counts of observed unique library paths and routine system executable/script paths; write `unknown` if counts are unreliable. Briefly note coverage gaps or links to device-specific libraries when relevant.

Use `High` for multiple independent direct indicators of device-specific role, `Medium` for one strong indicator, and `Low` for tentative candidates in the uncertain table. Sort rows by path; use `unknown` for missing fields. Add no introduction or conclusion.
