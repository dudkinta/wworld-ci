# wworld-ci

Public CI helper repository for **WWorld**.

Existing only to host GitHub Actions workflows that benefit from unlimited free minutes on public repositories. Contains no source code, infrastructure, or credentials in plain text.

## Workflows

### `reindex-codegraph.yml`

Triggers a reindex of the CodeGraph (Memgraph) instance running on the WWorld Ops server.

**Triggers:**
- `repository_dispatch` event of type `reindex-codegraph` — sent by the 9 private WWorld service repositories after a successful merge to `main`.
- `workflow_dispatch` — manual run from the Actions tab.

**Action:** SSH into Ops and execute `/opt/wworld/infrastructure/scripts/reindex-codegraph.sh`.

## Secrets

The workflow requires three repository secrets:

| Name | Purpose |
|---|---|
| `OPS_DEPLOY_HOST` | Ops server hostname/IP |
| `OPS_DEPLOY_USER` | SSH user on Ops |
| `OPS_SSH_PRIVATE_KEY` | Private key for SSH authentication. Recommended: dedicated deploy-key restricted to a single command via `command="..."` in `~/.ssh/authorized_keys` on Ops. |

## Why a separate public repo?

Public repositories on GitHub have unlimited free Actions minutes for standard runners. The reindex job is fully self-contained (single SSH call), so isolating it here keeps minute consumption out of private quotas without exposing any source code or sensitive configuration.
