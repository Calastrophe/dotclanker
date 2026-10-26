# fn-closures

> Use closures when captured state makes a helper simpler, and keep captured names easy to trace.

## Why it matters

A nested function retains bindings from its enclosing scope. Accidental rebinding or late lookup can make callbacks observe a different value than the author expects.

## Guidance

Keep a closure small and make its captured values apparent. When callbacks are created in a loop, bind the current value explicitly or use a factory so every callback does not read the same final loop value.

## Bad

```python
handlers = []
for route in routes:
    handlers.append(lambda request: dispatch(route, request))
```

## Good

```python
handlers = []
for route in routes:
    handlers.append(lambda request, route=route: dispatch(route, request))
```

## Caveats

The default-argument pattern is suitable for simple callbacks. Use a named factory or callable object when captured state is substantial or needs a clear lifecycle.
