# Changelog

All notable changes to this project are documented in this file.

## [0.2.0] - 2026-04-28

### Added

1. Policy recipe module in `src/policies.py` with three adoption contexts: `realtime-support`, `regulated-evidence`, and `offline-batch`.
2. Policy recipe guide in `docs/POLICY-RECIPES.md` with copy-paste adoption snippets and override examples.
3. Compatibility matrix and benchmark table in `docs/COMPATIBILITY-BENCHMARKS.md`.
4. Reproducible benchmark utility in `scripts/benchmark_policies.py` plus compact artifact `docs/reports/policy-benchmark-summary.json`.

### Changed

1. API reference updated with policy helper functions and recipe context coverage.
2. Deployment and testing docs expanded with packaging and build validation commands.
3. Package version metadata aligned to `0.2.0` in `pyproject.toml` and `src/__init__.py`.

## [0.1.1] - 2026-04-24

### Added

1. Production baseline governance files: `ARCHITECTURE.md`, `CONTRIBUTING.md`, `SECURITY.md`, and `CODEOWNERS`.
2. Repository collaboration context in `.claude/CLAUDE.md`.
3. Documentation pack: `docs/API.md`, `docs/DEPLOYMENT.md`, and `docs/TESTING.md`.
4. CI and release workflows in `.github/workflows`.
5. README sections for production evidence, limitations, and roadmap.
