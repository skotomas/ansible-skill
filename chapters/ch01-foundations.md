# Foundations

Ansible executes ordered tasks using connection plugins (SSH, local, WinRM/PSRP, network APIs and others). Python requirements depend on the target and modules. Agentless does not mean dependency-free.
Prefer modules expressing desired state. Idempotence is a property of the implementation and inputs, not a guarantee of every module. `changed_when: false` only changes reporting; it does not prevent side effects.
Ansible can provision resources and correct drift on a run. Choose Terraform for tracked resource lifecycle when appropriate; avoid having two tools own the same resource. Declarative tools still have dependencies; imperative execution does not guarantee first-run success.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_intro.html). Verify against the selected installed versions.
