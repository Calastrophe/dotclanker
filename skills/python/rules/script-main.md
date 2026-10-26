# script-main

> Put executable work behind a function and guard the module entry point.

## Why it matters

Importing a module should usually define its API without starting a command, opening resources, or changing external state.

## Guidance

Keep module-level work limited to definitions and safe constants. Put command behavior in `main()` and call it under the `__main__` guard. Return an exit status from `main()` when useful, then pass it to `sys.exit()` at the entry point.

## Bad

```python
print(load_configuration())
run_batch()
```

## Good

```python
def main() -> int:
    configuration = load_configuration()
    return run_batch(configuration)


if __name__ == "__main__":
    raise SystemExit(main())
```

## Caveats

Frameworks and package entry points may invoke a callable without a `__main__` block. Keep initialization in the framework's explicit startup hook and avoid import-time work in either case.
