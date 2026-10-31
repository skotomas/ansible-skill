# Troubleshooting

Identify whether failure is parsing, inventory, connection, escalation, module execution or application health. Inspect the smallest relevant result and redact secrets. Use ansible-doc and actual resolved configuration before guessing.
Start with normal verbosity; use -vvv/-vvvv when connection detail is needed. Avoid ANSIBLE_DEBUG and complete hostvars dumps with secrets. A task debugger may be useful interactively but can re-run side effects and must not block unattended jobs.

Verbosity ladder: `-v` task results, `-vv` data and playbook parsing, `-vvv` connection/SSH detail, `-vvvv` per-connection protocol debug. Escalate one level at a time; higher levels log more secrets.

Task debugger (enable per task with `debugger: on_failed`; values: `always`, `never`, `on_failed`, `on_unreachable`, `on_skipped`):

| Command | Action |
|---|---|
| `p task` | print the current task and its module/args |
| `p task.args` | print task arguments |
| `p task.args['key'] = value` | rewrite an argument in place |
| `p vars` / `p host` / `p result` | inspect variables, target host, last result |
| `redo` | re-run the task with current state |
| `continue` (`c`) | resume the play |
| `quit` (`q`) | abort the run |

Fix the source, not only the session: debugger edits die with the run.
On a changed SSH host key, verify the new fingerprint through an independent trusted source before replacing known_hosts. ssh-keyscan retrieves a key; it does not authenticate it.
Narrow retries to documented transient conditions with bounded retries/delay. Preserve unexpected failures. After an interactive fix, update the source and validate the intended rerun path.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_debugger.html). Verify against the selected installed versions.
