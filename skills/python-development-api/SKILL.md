---
name: python-development-api
description: Contains guidelines for designing and implementing Python package APIs.
---

Always read `python-development-general` skill before developing Python code.

## Instructions

- Design a public API intentionally. Expose only stable, well-named classes, functions, and constants through `__init__.py`. Keep internal modules private by prefixing them with an underscore.
- Use semantic versioning for releases. Avoid breaking changes in minor or patch versions; deprecate features before removing them.
- Keep backwards compatibility where possible. If a breaking change is necessary, document migration steps and provide a deprecation period with `warnings.warn`.
- Use clear, consistent naming conventions. Function names should describe actions; class names should describe concepts. Avoid abbreviations.
- Accept flexible inputs where appropriate but produce stable output types. Prefer Pydantic models or dataclasses for complex configurations.
- Document all public APIs with Google-style docstrings, including type hints for arguments and return values.
- Keep dependencies minimal and explicit. Declare optional features as extras in `pyproject.toml`.
- Provide runnable examples in the README and, for larger packages, dedicated API documentation.
- Add type hints throughout the public interface. Run `mypy` or `pyright` on the package before finishing.
- Make the package easy to test: keep side effects at the boundaries, inject dependencies, and avoid hidden global state.
- Define a clear error hierarchy with descriptive exception classes that users can catch.
