# Collections and dependencies

Use FQCN for modules and plugins. The collections keyword affects module/action and role resolution but does not replace FQCN for lookups, filters or tests.
requirements.yml is a dependency manifest, not a complete lockfile. Pin reviewed direct dependencies and account for transitive collections, Python dependencies and the execution environment. Track upgrades deliberately rather than freezing vulnerable versions indefinitely.
Review third-party content as executable code with access to controller credentials and possibly target privilege. Namespace names alone are not proof of official ownership. Prefer trusted sources, integrity checks and available signature verification according to deployment policy.
Check collection requires_ansible and module execution location: Python dependencies must exist where the module/plugin executes. For reproducible shared runs consider a versioned EE; it is optional for a small local project.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections_guide/collections_using_playbooks.html). Verify against the selected installed versions.
