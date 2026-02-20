---
name: python
description: Has extensive knowledge about writing and reading python code
---

# Python Skill
This skill contains information, best practices, and style guides on how to write and read python code.

## Development Principles
These are principles that must always be followed when developing in python.

### Best Practices
- Use `UV` as your environment manager. If not available and you are writing in python 3.0+, DO NOT use `pip`, but return flow to the developer explaining that you need UV to continue with instructions on how to install.

- Modern Python (3.12+): Use modern syntax and features — structural pattern matching, union types with `X | Y`, `dataclasses`, `pathlib`, f-strings, walrus operator where it improves readability.

- Type Hints: **Always** include comprehensive type hints using the modern syntax (`list[str]` not `List[str]`, `str | None` not `Optional[str]`). Use `typing` module constructs only when necessary (e.g., `Protocol`). Don't forget that Python 3.12 introduced generics!

- Function Arguments: Always be explicit when passing function arguments (`func(arg1=value1, arg2=value2, ...)` not `func(value1, value2)`)

- Docstrings: Write Google-style docstrings for all public functions, methods, and classes. Include `Args`, `Returns`, `Raises` sections as appropriate. Keep them concise but informative. Example:
    ```python
    def foo(bar: str) -> str:
        """Does an awesome thing using `bar`.

        This function does more awesome things that are cool too!

        Args:
            bar: Used to do awesome things

        Returns:
            str: The awesome thing done

        Raises:
            ValueError: If `bar` was not cool enough
        """
        ...
    ```

- Testability: Design code with testability in mind — use dependency injection, avoid global state, prefer pure functions, and keep side effects at the boundaries. When writing code, consider how it would be tested.

- Error Handling: Use specific exception types, create custom exceptions when appropriate, and handle errors explicitly. Never use bare `except:` and only in dire situations use `except Exception` (for example in your `main()`).

- Readability: Follow PEP 8. Prefer clarity over cleverness. Use meaningful variable and function names. Keep functions focused and short. If an algorithm is extremely complicated, the goal is for even a child to be able to understand and read the implementation.

- Configuration: If external variables are needed, always inject them through a YAML configuration. Never use constants (unless it's extremely obvious the value will never change, such as byte lengths for packets) or dynamically load environment variables outside of `main()`

### Library Selection
Personal preferences:
- Logging: loguru
- API: FastAPI
- Modeling / Serialization: Pydantic
- SQL (both ORM and plain SQL): sqlalchemy
- Testing: pytest

### Code Structure
- You must split code into separate files/modules. Never put unrelated classes or definitions in the same file. Organize code into well-defined modules with clear responsibilities. Take example from the following structure:
    ```yaml
    src/
        project_name/
            __init__.py
            models.py         # Pydantic schemas
            routes.py         # API endpoints
            services.py       # Business logic
            storage.py        # Data layer
            exceptions.py     # Custom exceptions
            config.py         # Configuration loading
    ```
- functions that are not related to a module directly should be in utility files
- Use `__all__` to define public APIs of modules
- Prefer composition over inheritance
- Use `Protocol` for structural subtyping when appropriate
- Use `pydantic.BaseModel` for data containers — avoid plain dicts for structured data
- Use `enum.Enum` or `enum.StrEnum` for fixed sets of values
- Use context managers (`contextlib.contextmanager` or `__enter__`/`__exit__`) for resource management

### Quality Checks
Before presenting code, verify:
- All functions have type hints and docstrings
- No unused imports or variables
- Error cases are handled
- Names are descriptive and follow Python conventions (snake_case for functions/variables, PascalCase for classes)