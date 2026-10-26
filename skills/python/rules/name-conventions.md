# name-conventions

> Use names that match Python conventions and explain a value's role.

## Why it matters

Consistent names make code easier to scan and help readers distinguish types, callables, constants, and internal details.

## Guidance

Use `snake_case` for functions, methods, variables, and modules; `CapWords` for classes and exceptions; and `UPPER_SNAKE_CASE` for constants. Prefix implementation details with a single underscore when they are not part of the public interface. Avoid shadowing built-ins and unexplained abbreviations. Keep established external API names when compatibility requires them.

## Bad

```python
class userData:
    def Get(self, id):
        return list[id]
```

## Good

```python
class UserData:
    def get(self, user_id: int) -> User:
        return self._users[user_id]
```

## Caveats

Short names are appropriate for conventional indices, mathematical notation, and tightly scoped comprehensions. Keep public names descriptive enough to make sense outside their defining module.
