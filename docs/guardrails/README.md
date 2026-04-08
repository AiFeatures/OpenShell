# Operator Guardrails (experimental)

Minimum delighter slice for the "operator guardrails and policy UX" P1
backlog item. Ships three opinionated policy packs so admins can pick one and
preview how commands will be constrained before rollout.

## Policy packs

| Pack | Posture | Default command handling | Approval required |
|---|---|---|---|
| `observer` | least privilege | dry-run everything; block side effects | every `write` |
| `standard` | balanced | allow read + common ops; deny destructive | `delete`, `chmod`, network egress |
| `power-user` | broad | allow most commands | only for irreversible ops |

See `observer.yaml`, `standard.yaml`, and `power-user.yaml` for the full rule
shape. Each pack documents, in plain text, exactly which command categories
are allowed, denied, or quarantined for approval.

## Rollout preview

The rollout preview is currently docs-only: before enabling a pack in a real
environment, an admin can read the pack's YAML and the corresponding
`preview/*.txt` file to see a sample of how 20 representative commands would
be classified.

Wiring the preview into a runtime checker is tracked as the next pass.
