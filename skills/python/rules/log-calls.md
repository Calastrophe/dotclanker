# log-calls

> Pass variable values as logging arguments instead of formatting the message first.

## Why it matters

The logging system can defer formatting until it emits a record, and it can retain the original message template and values.

## Guidance

For the standard `logging` API, pass a format string and its values separately. Use `logger.exception()` inside an exception handler when the traceback belongs in the record. Log at the layer that can add useful context, and avoid logging the same failure at every layer that re-raises it.

## Bad

```python
logger.info(f"Loaded {count} records")
```

## Good

```python
logger.info("Loaded %s records", count)
```

## Caveats

Some structured logging libraries use a different API. Follow that library's documented parameter conventions. Do not put secrets or sensitive data in log arguments.
