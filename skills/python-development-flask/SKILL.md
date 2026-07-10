---
name: python-development-flask
description: Contains opinionated instructions for building server-rendered and lightweight web applications with Flask.
---

# Python Flask development skill

Always read `python-development-general` skill before developing Python code.

## Instructions

- Use `flask` for server-rendered or lightweight web apps. Add it with `uv add flask` and `gunicorn` as a production WSGI server with `uv add --dev gunicorn`.
- Structure the project using the application factory pattern: create `create_app()` in a `factory.py` or top-level `app.py` and keep configuration separate.
- Organise routes with Blueprints. Register each blueprint in the factory.
- Place templates under `templates/` and static assets under `static/`. Use Jinja2 inheritance with a base layout and named blocks.
- Keep business logic out of route handlers. Handlers should parse requests, call services or helpers, and return responses or rendered templates.
- Validate configuration with environment variables using `pydantic-settings` or `os.environ`, and fail fast on missing required values. Never hardcode secrets.
- Use `flask.flash` and `flask.redirect` sparingly for user feedback, and prefer explicit error pages and structured logging for unexpected failures. If the feedback is relating to client-side form validation, use `flask_wtf` and `wtforms`
- Return consistent error responses. Register `errorhandler` callbacks for `404`, `500`, and other expected exceptions, and render templates or JSON depending on the request type.
- Use `flask_wtf` and `wtforms` for form handling and CSRF protection when accepting user input. Add `uv add flask-wtf` and keep `SECRET_KEY` set in production.
- Use the `g` object or Flask extensions for request-scoped shared resources, and avoid global mutable state across requests.
- Write tests with `pytest` and `flask.test_client`. Use the application factory to create test app instances and override configuration via `app.config`.
- Add a `/health` endpoint that returns a simple success payload and a request-logging middleware or `before_request`/`after_request` hooks for observability.
- Use a production WSGI server such as `gunicorn` or `uwsgi` instead of the built-in development server. Run with `gunicorn -w 4 -b 0.0.0.0:8000 'app:create_app()'`.
- Implement rate-limiting only if requested. If so, use `flask-limiter`