# CLAUDE.md — OpenShell (AiFeatures fork)

Upstream: NVIDIA/OpenShell — safe, private runtime for autonomous AI agents with sandboxed execution and declarative YAML policies. Built agent-first in Rust. See AGENTS.md for the full agent instruction surface.

## Quick Start

```bash
# Install binary (recommended)
curl -LsSf https://raw.githubusercontent.com/NVIDIA/OpenShell/main/install.sh | sh

# Or via PyPI (requires uv)
uv tool install -U openshell

# Create a sandbox for Claude (Docker must be running)
openshell sandbox create -- claude
```

## Development

```bash
# Full local CI (lint + compile + tests) — run before opening a PR
mise run ci

# Unit tests only
mise run test

# End-to-end (requires running cluster)
mise run e2e

# Pre-commit checks (lint, format, license headers)
mise run pre-commit
```

Always use `uv` for Python commands. Always prefer `mise` commands over direct docker/cargo invocations.

## Architecture

Rust workspace with crates in `crates/`:

| Crate | Purpose |
| --- | --- |
| `openshell-cli` | User-facing CLI binary |
| `openshell-server` | Control-plane API, sandbox lifecycle, auth boundary |
| `openshell-sandbox` | Container supervision, policy-enforced egress routing |
| `openshell-policy` | Filesystem, network, process, and inference constraints |
| `openshell-router` | Privacy-aware LLM routing |
| `openshell-ocsf` | OCSF v1.7.0 structured logging (security events) |
| `openshell-bootstrap` | K3s cluster setup, image loading, mTLS PKI |
| `openshell-core` | Common types, config, error handling |

Additional: `python/openshell/` (Python SDK + PyPI packaging), `proto/` (gRPC contracts), `deploy/` (Docker, Helm, K8s manifests).

## Conventions

- Conventional commits: `feat(scope):`, `fix:`, `docs:`, `chore:`, `ci:`, `perf:`
- Never mention Claude or AI agents in commit messages
- Scope changes to the issue at hand — no unrelated changes in a branch
- Security findings: dual-emit OCSF events (domain event + DetectionFinding)
- Never log secrets or query parameters in OCSF messages

## Shared Resources

| Resource | Location |
| --- | --- |
| Reusable CI workflows | `Ai-road-4-You/enterprise-ci-cd` |
| Agent skills | `.agents/skills/` |
| AgentHub | `~/AgentHub/` (central hub, 12 MCP servers) |

## AgentHub

- Central hub: `~/AgentHub/`
- Skills: `.agents/skills/` (symlinked to AgentHub shared skills)
- MCP: 12 servers synced across all agents
- Agents: 14 shared agents available
- Hooks: Safety, notification, and logging hooks
