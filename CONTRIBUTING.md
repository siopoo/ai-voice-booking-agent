# Contributing

Thanks for helping improve AI Voice Booking Agent.

## Development setup

1. Fork and clone the repository.
2. Create and activate a Python 3.11+ virtual environment.
3. Install development dependencies with `python -m pip install -r requirements-dev.txt`.
4. Copy `.env.example` to `.env`. API keys are optional for tests and reliable mode.

## Running checks

Before opening a pull request, run:

```bash
ruff check .
pytest -q
python -m evals.run_evals
npm test
```

Tests and deterministic evaluations must pass without real LLM or STT credentials.

## Coding style

- Target Python 3.11+ and keep changes compatible with the existing layered architecture.
- Put business rules in services and persistence concerns in repositories.
- Treat model output as untrusted; booking mutations must remain behind LangGraph guards.
- Add focused tests for behavior changes and avoid drive-by refactors.

## Pull requests

Keep pull requests focused, describe the behavior and trade-offs, list verification commands, and update documentation when public behavior changes. Never commit `.env`, databases, recordings, API keys, tokens, or customer data.

Security reports should follow [SECURITY.md](SECURITY.md), not public issues.
