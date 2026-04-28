# Testing Guide

## Test Scope

The test suite validates deterministic behavior for:

1. Relevance scoring order.
2. Redundancy pair detection.
3. Contradiction detection.
4. End-to-end filtering decisions.
5. LangChain wrapper filtering behavior.
6. Report rendering output.

## Run Tests

```bash
pytest -q
```

## Build Validation

```bash
python -m pip install --upgrade build
python -m build --sdist --wheel
```

## Test Design Principles

1. No network dependency in tests.
2. Fake embedding and contradiction models are used for deterministic outputs.
3. Assertions focus on behavior contracts, not implementation details.

## CI Expectation

Every pull request should pass the full test suite in CI before merge.
