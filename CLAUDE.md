# CLAUDE.md

## Project: OpenShell

**Organization:** AiFeatures (iAiFy Enterprise)
**Language:** Rust + Python
**Description:** Safe, private sandboxed runtime for autonomous AI agents with declarative YAML policy enforcement

## Build & Test

```bash
# Install dependencies (Python)
uv sync --all-extras --group dev

# Install CLI binary
uv pip install .

# Build Rust crates
cargo build

# Test (Python)
uv run pytest -v --tb=short

# Test (Rust)
cargo test

# Pre-commit checks (lint + format + license headers)
mise run pre-commit

# Full local CI
mise run ci

# Lint (Python)
uv run ruff check

# Format (Python)
uv run ruff format

# E2E tests (requires running cluster)
mise run e2e
```

## Architecture

```text
crates/
  openshell-cli/          # User-facing CLI binary
  openshell-server/       # Gateway control-plane API
  openshell-sandbox/      # Container supervision, policy-enforced egress
  openshell-policy/       # Filesystem, network, process, inference constraints
  openshell-router/       # Privacy-aware LLM routing
  openshell-bootstrap/    # K3s cluster setup, mTLS PKI
  openshell-ocsf/         # OCSF v1.7.0 structured logging
  openshell-core/         # Common types, config, error handling
  openshell-providers/    # Credential provider backends
  openshell-tui/          # Ratatui terminal dashboard
python/openshell/         # Python SDK and CLI packaging
proto/                    # Protobuf / gRPC service contracts
deploy/                   # Docker, Helm, K8s manifests
.agents/skills/           # Agent workflow automation
.agents/agents/           # Sub-agent definitions
architecture/             # Design decisions and component docs
```

## Conventions

- Conventional commits: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `ci:`, `perf:`
- Kebab-case file names
- Branch protection on main -- PRs required
- Always use `mise` commands over direct docker/cargo builds when available
- Always use `uv` for Python commands
- Run `mise run pre-commit` before every commit
- OCSF structured logging for observable sandbox events; plain `tracing` for internal plumbing
- Never mention AI agents in commit messages (no Co-Authored-By)

## Shared Resources

| Asset | Location |
| --- | --- |
| CI/CD workflows | Ai-road-4-You/enterprise-ci-cd@v1 |
| Composite actions | Ai-road-4-You/github-actions@v1 |
| Governance | Ai-road-4-You/governance |

## Fork Info

- Upstream: NVIDIA/OpenShell
- Do NOT create PRs to upstream
- Sync managed by Ai-road-4-You/fork-sync
- Divergence is intentional where documented in README iAiFy Fork Notes
