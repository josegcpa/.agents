---
name: python-general
description: Contains opinionated general instructions and guidelines on how to develop software using Python.
---

# Python development skill

Always read `python-general` skill before developing Python code.

## Instructions

- Use `uv` to perform any and all package management. Always use `uv` commands and avoid at all costs editing `pyproject.toml` or `uv.lock`. Dependencies relating to formatting, linting and testing should be added as `--dev` dependencies
- Type-hinting is obligatory and documentation should follow the Google style, with clear `Args:`, `Returns:` and `Raises:` (if necessary). Each argument in the documentation should be followed with the type and whether it is optional in parenthesis. The first `"""` should always be followed by a paragraph. Type hinting should not use `typing` types which are usually available in Python (i.e. `list`, `dict`, `tuple`, etc). Example:
```
def function(test: int, test_optional: int = 1) -> bool:
    """
    Function description.
    
    Args:
        test (int): Description of test.
        test_optional (int, optional): Description of test_optional. 
            Defaults to 1.
    
    Returns:
        bool: Description of return value.

    Raises:
        ValueError: Description of when this error is raised.
    """
```
- Use a standard project layout: `src/<package>/`, `tests/`, `pyproject.toml`, and `README.md`. Keep package names short, lowercase, and unique.
- Pin dependencies by keeping `uv.lock` up to date. Use compatible version constraints in `pyproject.toml` and avoid hand-editing the lock file; let `uv` manage it.
- Never commit secrets or `.env` files. Load configuration via environment variables (e.g., `pydantic-settings` or `os.environ`) and validate required values at startup.
- Working with `csv` or other separator-based data types should use the `csv` module for simple read/write operations. Use `pandas` for complex tabular manipulation, exploratory analysis, Excel, or `parquet` (`pyarrow` backend). For large/very large datasets (>100,000 rows), prefer `polars` with lazy loading.
- When asked to implement a web platform, avoid high-level wrappers such as `streamlit`. For REST APIs, use `fastapi` (see the `python-rest-api` skill). For server-rendered or lightweight web apps, use `flask` (see the `python-flask` skill)
- To perform operations checking for geometric properties/intersections/etc make use of `shapely` (`uv add shapely`)
- Formatting will be performed using `black` with a line length of 80 (i.e. `black --line-length 80`) and linting will be performed using `ruff`. Before finishing any task, run `ruff check .`, `black --line-length 80 .`, and `pytest` (if tests exist) and fix all issues. 
- To reduce variability when formatting and linting, write these commands as a script using `uv tool` (i.e. `post-task.sh`) and execute that file in strict mode. No need to install any of these tools in the working `uv` environment.
- Always create the following `pre-commit` configuration (`.pre-commit-config.yaml`) if none is available:
```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    -   id: check-yaml
    -   id: check-added-large-files

-   repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
    -   id: isort
        args: [--profile, black, "--line-length", "80", --filter-files]

-   repo: https://github.com/psf/black
    rev: 22.12.0
    hooks:
    -   id: black
        args: ["--line-length", "80"]
```
- When developing CLIs always use `argparse`. If you are developing a complex CLI which might require multiple endpoints (i.e. `cli-name train`, `cli-name process`) use subparsers via `parser.add_subparsers`
- Always include the hatchling build system, for example in `pyproject.toml`:
```
[build-system]
requires = ["uv_build>=0.11.26,<0.12"]
build-backend = "uv_build"
```
- In-line comments/comments explaining code should be reduced to a minimum and for code bits which are genuinely hard to understand
- Use test-driven development but be mindful of the tests you implement: implementing tests which are too specific is useless and will significantly reduce development speed. For more information read the `python-tests` skill

## When to Use This Skill

Use `/python-general` when:
- Writing or setting up any Python project
- Managing dependencies and environments with `uv`
- Applying formatting, linting, and type-hinting standards
- Creating Python CLIs, scripts, or general-purpose packages
- The task does not specifically require another Python skill