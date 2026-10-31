# Roles and reuse

Use roles for cohesive reusable components; task files, include_tasks, import_tasks and import_playbook are also valid reuse mechanisms. Preserve a small project's existing structure unless complexity warrants change.
Expose inputs through prefixed role defaults; validate required inputs with argument_specs or assertions. Role vars are high-priority implementation data, not an access-control boundary.
Use only needed directories. Files/templates resolve relative to the role context. Declare dependencies when unconditional coupling is intended; otherwise compose roles explicitly in a playbook.
Document required inputs, supported systems, effects and recovery constraints. Verify the actual installed state as well as repeat-run behavior.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_reuse_roles.html). Verify against the selected installed versions.
