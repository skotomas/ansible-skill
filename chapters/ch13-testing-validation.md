# Validation

Choose checks proportional to the change: YAML parsing and ansible-lint, syntax-check with actual dependencies, intended-host inspection, staging execution and observable health checks. Check repeated execution for persistent configuration roles.
--check may skip unsupported modules, fail on dependencies absent in the simulation, or execute real changes via check_mode: false. Lookups and plugins can have controller-side effects. --diff may expose secret files; disable sensitive diffs.
Use Molecule for meaningful role scenarios on the project's supported versions; consult its current configuration rather than copying old lint/driver options. Container tests do not validate every kernel, reboot or systemd behavior; use disposable VMs where required.
Use reviewed supported test images, pinned by version/digest as appropriate. Avoid privileged containers on a controller holding credentials. If privileged tests are necessary, isolate them in disposable dedicated runners without production access.
Security scanners supplement review. Distinguish findings from scanner failure and verify fresh reports; suppress only understood findings with reasons. A green scan does not prove safety.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_checkmode.html). Verify against the selected installed versions.
