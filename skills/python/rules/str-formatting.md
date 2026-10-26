# str-formatting

> Choose string construction that keeps values, escaping, and cost clear.

## Why it matters

Readable formatting prevents accidental representation changes and avoids repeated copying when assembling many fragments.

## Guidance

Use f-strings for short interpolation in ordinary output. Use `!r` when a debugging or error message needs an unambiguous representation. Collect repeated fragments and join them rather than repeatedly growing a string in a loop. Preserve required delimiters, including a trailing separator when the output format calls for one. Use the formatting API required by localization or a library.

## Bad

```python
message = ""
for name in names:
    message += name + ", "
```

## Good

```python
message = "".join(f"{name}, " for name in names)
```

## Caveats

Do not use string interpolation to construct SQL or shell commands. Use the library's parameter binding or argument-list API. F-strings require Python 3.6 or later.
