---
description: Survey a device's listening ports and report device-specific services.
argument-hint: [SSH connection or serial port]
---

# Survey

Connection details: $ARGUMENTS

1. Stop unless the user supplied either SSH access details (including credentials) or a serial port. Connect only through the supplied access method.

2. Run `netstat -tulpn` or the device's equivalent, such as `ss -tulpn`. Record each listener's process, protocol, and address:port. Resolve full, untruncated names through PID metadata such as `/proc/<pid>/exe` or `/proc/<pid>/cmdline`. Never guess missing names; mark them unknown with their PID.

3. Keep device-specific and uncertain processes. Omit clearly ordinary system daemons and processes; record their full names.

4. Return only Markdown with two tables headed `Non-loopback`, then `Loopback`, both with columns `Process | Protocol | Binding`. Preserve all IPv4/IPv6 addresses and ports. Put wildcard and unspecified bindings in the first table. Include processes with both binding types in both tables.

5. End with `OMITTED: - process1, process2`, listing unique omitted names separated by commas. Use `OMITTED: - none` if empty. Add no prose or credentials.
