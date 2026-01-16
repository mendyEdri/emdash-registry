# Python Coding Standards

## Style

- Follow PEP 8 for formatting
- Use 4 spaces for indentation
- Maximum line length: 88 characters (Black default)
- Use type hints for function signatures

## Naming Conventions

- snake_case: Variables, functions, methods, modules
- PascalCase: Classes
- UPPER_SNAKE_CASE: Constants
- _private: Single underscore for internal use
- __mangled: Double underscore for name mangling

## Type Hints

```python
def process_items(items: list[str], limit: int = 10) -> dict[str, int]:
    ...
```

- Use `Optional[T]` or `T | None` for nullable types
- Use `TypeVar` for generics
- Use `Protocol` for structural typing

## Imports

- Group: stdlib, third-party, local
- Use absolute imports
- Avoid `from module import *`
- Use `if TYPE_CHECKING:` for type-only imports

## Best Practices

- Use context managers (`with`) for resource management
- Prefer list/dict/set comprehensions over loops when readable
- Use dataclasses or Pydantic for data structures
- Use `pathlib.Path` over `os.path`
- Use f-strings for formatting

## Error Handling

- Be specific with exception types
- Don't catch bare `Exception`
- Use custom exceptions for domain errors
- Always include context in error messages

## Testing

- Use pytest as the test framework
- Name tests descriptively: `test_user_can_login_with_valid_credentials`
- Use fixtures for setup/teardown
- Aim for behavior testing over implementation testing
