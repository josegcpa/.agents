---
name: python-development-rest-api
description: Contains opinionated instructions for building REST APIs with FastAPI.
---

# Python REST API development skill

Always read `python-development-general` skill before developing Python code.

## Instructions

- Use `fastapi` for REST API development. Add `uv add fastapi[standard]` to include the recommended server and dependencies.
- Define request and response models with Pydantic. Reuse models across endpoints and keep them in a dedicated `schemas.py` or `models.py` module.
- Organise the project into routes, services, and repositories. Keep business logic out of route handlers; routes should validate input, call services, and return responses.
- Use FastAPI dependency injection for shared resources such as database sessions, configuration, and authentication. Define dependencies as reusable functions in a `dependencies.py` module.
- Return consistent error responses. Use FastAPI exception handlers or `HTTPException` with appropriate status codes and a structured error body.
- Add OpenAPI metadata and tags to routers for automatic documentation.
- Use `async` endpoints for I/O-bound work (databases, HTTP calls) and standard `def` endpoints for CPU-heavy work.
- Validate configuration via environment variables with `pydantic-settings`. Never hardcode secrets.
- Write tests with `httpx` and `fastapi.testclient.TestClient`. Override dependencies in tests to use in-memory or fake implementations.
- Add structured logging for all requests.
- To implement rate-limiting, always use `slowapi`. Please note that a `request: Request` should be implemented for the functions with rate-limiting
- For API session handling, always use cookies if there is no user registration. If there is user registration, please use JWT tokens.