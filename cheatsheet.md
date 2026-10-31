# Operator reference

Read project configuration before running commands. Supply the actual inventory and playbook paths; these are illustrative names.

```bash
ansible --version
ansible-doc ansible.builtin.template
ansible-playbook -i inventories/staging site.yml --syntax-check
ansible-playbook -i inventories/staging site.yml --list-hosts
ansible-playbook -i inventories/staging site.yml --check --diff
```

Review secret diff suppression and check_mode overrides before the last command. A nonempty intended host list and supported module behavior matter more than a green exit code. Use actual execution and health checks in an authorized test environment before production.

For secret input and optional Ansible Vault see [secrets](chapters/ch10-vault-secrets.md). For parallelism and callbacks see [performance](chapters/ch14-performance.md).

## Thresholds

| Setting | Baseline | Rationale |
|---|---|---|
| `forks` | 20+ (≈ controller cores) | parallelism ceiling |
| `ssh pipelining` | true | removes per-task sftp transfer |
| `ControlPersist` | 60s | reuses SSH connections across tasks |
| `serial` in production | 2 hosts or ≤25% | blast-radius control |
| `retries` / `delay` | 3–5 / 5–15s | transient infrastructure only |
| `gather_facts` | false unless needed | ~1s per host per play |
| `max_fail_percentage` | 20 | tolerate bounded batch failure |
| `timeout` on wait/uri | explicit, ≤60s default | avoid indefinite hangs |

## Tells and smells

- `ignore_errors: true` scattered through a playbook → hiding real failures; replace with `failed_when` on a precise condition.
- Long `shell` chains where a state module exists → scripting, not declaring state.
- The same variable defined in several places "because it works" → precedence misunderstanding; keep one source per setting.
- A playbook only ever run with `--check` → untested; check mode cannot exercise handlers, service restarts or ordering.
- Unpinned `requirements.yml` or no committed `ansible.cfg` → each operator runs a different automation stack.
- Hand-edited configs and fear of reboots on managed hosts → configuration drift; the fleet is no longer reproducible from the repository.
- Debug tasks left with `no_log` absent and secret output in logs → secrets leak through verbosity.
