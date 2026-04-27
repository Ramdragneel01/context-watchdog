# Deployment Guide

context-watchdog is distributed as a Python library and can be deployed inside application environments that perform retrieval-augmented generation.

## Prerequisites

1. Python 3.11+
2. pip

## Local Installation

```bash
pip install -r requirements.txt
```

## Build and Install Distribution Artifacts

```bash
python -m pip install --upgrade build
python -m build --sdist --wheel
pip install dist/context_watchdog-*.whl
```

## Recommended Runtime Practices

1. Pin dependency versions in production lock files.
2. Cache transformer models in the runtime image when internet access is restricted.
3. Use deterministic fallback mode for offline or air-gapped workloads.
4. Avoid logging raw chunk text when it may contain sensitive information.

## Containerized Usage (Optional)

1. Build an application image that vendors this repository and its dependencies.
2. Run tests during image build to prevent drift:

```bash
pytest -q
```

## Release Artifacts

Release workflow now publishes Python package artifacts:

1. source distribution (`.tar.gz`)
2. wheel (`.whl`)

## Optional Configuration

The library itself does not require environment variables. Wrapper applications may externalize thresholds:

1. `CONTEXT_WATCHDOG_RELEVANCE_MIN`
2. `CONTEXT_WATCHDOG_REDUNDANCY_MAX`
3. `CONTEXT_WATCHDOG_CONFLICT_MIN`

## CI and Release

1. CI workflow: `.github/workflows/ci.yml`
2. Release workflow: `.github/workflows/release.yml`
