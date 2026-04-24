# Contributing

Thanks for contributing to context-watchdog.

## Development Setup

1. Use Python 3.11 or newer.
2. Create and activate a virtual environment.
3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Quality Gates

1. Run tests before opening a pull request:

```bash
pytest -q
```

2. Keep tests deterministic; avoid adding network-dependent tests.
3. Add test coverage for every behavior change in filtering logic.

## Pull Request Guidelines

1. Keep changes focused and small.
2. Update documentation when behavior or defaults change.
3. Include test evidence in the PR description.
4. Do not commit secrets or credentials.

## Code Style

1. Prefer readable, typed Python.
2. Use concise docstrings on public classes and functions.
3. Keep fallback behavior deterministic for reproducible scoring.
