# Security and hardening

Select a baseline appropriate to OS/version and workload. Review generated OpenSCAP remediation, test on a disposable representative host and preserve recovery access before changing SSH, firewall, authentication or boot settings.
Verify provenance and version of remediation content. Do not automatically download latest content and execute it with root privileges. Re-scan and archive restricted reports off-host after application.
Handle scanner exit codes explicitly: distinguish findings from execution failures. Check that each report is fresh and complete. Use no_log for secrets, not to conceal all diagnostics, and avoid blanket ignore_errors.
Run only authorized web scans against specified targets. Choose passive/active scanner mode deliberately. Scan output can contain sensitive data and needs access controls.
For bootstrap confirm a working alternate admin session and sudo before disabling existing access; validate sshd configuration and firewall rules before applying them. Least privilege should reflect actual operations, not broad service-wide administrator policies.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_privilege_escalation.html). Verify against the selected installed versions.
