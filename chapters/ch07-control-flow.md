# Control flow and rolling changes

Handlers coalesce notifications per host between flush points. They normally run after pre_tasks, roles/tasks, and post_tasks; failure can prevent execution unless force_handlers applies. A handler is preferred for config-triggered restart; an explicit maintenance restart can be an ordinary task.
rescue handles task failures, not every unreachable or invalid-task condition. always is not an unconditional finally across all failure classes. A successful rescue can let execution proceed; explicitly fail again if the rollout must stop.
Choose serial based on availability and quorum, starting with one canary where suitable. max_fail_percentage is evaluated per batch and must be exceeded; 0 stops after a failure is processed, not before concurrent work already underway. run_once executes once per serial batch. Use explicit orchestration for globally once-only migrations.
Use async only for independent work; poll and check completion before dependent tasks, clean async status files as appropriate. Avoid overlapping package-manager locks. Ordinary loop items are sequential; throttle caps task concurrency across hosts.
Store backups in a private directory with restricted permissions; use tempfile for unpredictable temporary paths. Use application-consistent backup procedures and test restoration.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_error_handling.html). Verify against the selected installed versions.
