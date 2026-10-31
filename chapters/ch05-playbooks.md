# Playbooks

Make host selection explicit and ensure inventory group names match across environments. Scope become to operations that need it; do not assume every task requires root.
For configuration updates: render or copy with ownership/mode, validate, notify a handler, then check service health after handlers have run. Use flush_handlers when a dependent task needs the new configuration immediately.
Use lowercase booleans and quote values starting with Jinja expressions and octal modes. Use ansible_facts dictionary access for facts; gather only what the play needs.
Check mode is a simulation with exceptions, not a transaction or sandbox. A failed play does not automatically roll back previous changes. Plan recovery appropriate to the change.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_intro.html). Verify against the selected installed versions.
