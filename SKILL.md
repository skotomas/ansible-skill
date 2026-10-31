---
name: ansible
description: "Write, review and troubleshoot Ansible inventories, playbooks, roles and automation workflows, with version-aware validation and safe handling of access and secrets."
---

# Ansible engineering

Use project conventions and the user's scope. Read only the relevant chapters below. This is maintained engineering guidance, not a verified verbatim synthesis of books.

## Working method

1. Inspect existing inventory, roles, configuration and dependencies. Establish intended hosts, controller/target versions and execution location. Use ansible --version, ansible-doc and configuration inspection; avoid output containing secrets.
2. Prefer appropriate state modules with FQCN. Use tasks/includes or roles according to cohesion and reuse. Specify ownership/modes and validate configurations where relevant. Scope privilege escalation to need.
3. Preserve host key verification. For replacement keys verify fingerprints independently. Choose rollout size from availability/quorum, with health checks and a recovery path.
4. Use existing authentication and secret handling. This user has no established Vault setup: do not require or deploy one. Prompts, protected out-of-repository files or existing CI credentials can suffice. Ansible Vault is optional serverless file encryption; HashiCorp Vault is a separate service. Read chapter 10 for secrets.
5. Validate proportionally: parse/lint, syntax and host selection, representative staging, actual state and repeat-run behavior. --check is not a sandbox; overrides and plugins can perform work. Protect secret diffs and logs. State which checks actually ran.
6. Verify version-sensitive names, parameters and dependencies against installed ansible-doc and official documentation/porting guides. Pin compatible dependencies and update them deliberately. Do not use old book snippets as current API documentation.

## Operational invariants

- Role defaults are overridable inputs; role vars override inventory values and are not immutable. Consult chapter 6 for scope/precedence conflicts.
- Handlers are appropriate for restarts triggered by config changes; explicit maintenance restarts may be ordinary tasks. Flush before dependent health checks.
- changed_when controls reporting, not idempotence. Handle expected nonzero results precisely; preserve unexpected failures.
- No_log, diff suppression and file permissions protect different channels. Avoid full hostvars dumps, secret CLI arguments and secret fact caching.
- Check actual target groups: --limit only narrows hosts, and changing -i does not rewrite a play's hosts pattern.
- Do not add infrastructure, broad upgrades, deployments or external notifications merely because a chapter describes them.

## Topic references

- [01: Foundations](chapters/ch01-foundations.md)
- [02: Installation and configuration](chapters/ch02-installation-setup.md)
- [03: Inventory](chapters/ch03-inventory.md)
- [04: Modules and command execution](chapters/ch04-adhoc-core-modules.md)
- [05: Playbooks](chapters/ch05-playbooks.md)
- [06: Variables and facts](chapters/ch06-variables-facts.md)
- [07: Control flow and rolling changes](chapters/ch07-control-flow.md)
- [08: Roles and reuse](chapters/ch08-roles.md)
- [09: Collections and dependencies](chapters/ch09-collections-galaxy.md)
- [10: Secrets without mandatory Vault](chapters/ch10-vault-secrets.md)
- [11: Custom modules and plugins](chapters/ch11-modules-plugins-filters.md)
- [12: Troubleshooting](chapters/ch12-debugging-troubleshooting.md)
- [13: Validation](chapters/ch13-testing-validation.md)
- [14: Performance](chapters/ch14-performance.md)
- [15: Cloud and Terraform](chapters/ch15-iac-cloud.md)
- [16: Containers](chapters/ch16-containers.md)
- [17: Kubernetes](chapters/ch17-kubernetes.md)
- [18: Security and hardening](chapters/ch18-security-hardening.md)
- [19: CI, AWX and shared execution](chapters/ch19-ci-cd-automation.md)
- [20: Office cluster workflow](chapters/ch20-cluster-playbook.md)
- [21: YAML file templates](chapters/ch21-yaml-templates.md)

## Topic index

- **ad-hoc commands** → ch04
- **ansible.cfg** → ch02, ch14, cheatsheet
- **become / privilege escalation** → ch02, ch05
- **block / rescue / always** → ch07, ch21
- **check mode (`--check --diff`)** → ch05, ch13, cheatsheet
- **collections / FQCN** → ch09, ch21
- **conditionals (`when`), loops (`loop`)** → ch07, ch21
- **debugger** → ch12
- **dynamic inventory** → ch03
- **facts / `ansible_facts`** → ch06, ch21
- **handlers / `notify` / `flush_handlers`** → ch05, ch07
- **inventory groups / patterns / `--limit`** → ch03, ch05
- **Kubernetes / Kubespray** → ch17
- **Molecule** → ch13
- **performance (`forks`, pipelining, `ControlPersist`)** → ch14, cheatsheet
- **precedence of variables** → ch06
- **register / `changed_when` / `failed_when`** → ch06, ch07, ch21
- **roles / defaults vs vars** → ch08
- **rolling updates (`serial`, `max_fail_percentage`)** → ch07, cheatsheet
- **secrets / Vault / `no_log`** → ch10, SKILL.md §Working method
- **Terraform + Ansible** → ch15
- **templates / Jinja2** → ch06, ch21
- **validation (`--syntax-check`, ansible-lint)** → ch13, cheatsheet
- **variables (group_vars, host_vars)** → ch03, ch06

## Sources and maintenance

Official version-specific documentation takes precedence over examples in older books. Useful background: *Ansible: Up and Running*, 3rd edition (Bas Meijer, Lorin Hochstein, René Moser, 2022); *Learn Ansible*, 2nd edition (Russ McKendrick, 2024); *The Ansible Workshop* (Aymen El Amri, 2024); *Infrastructure as Code for Beginners* (Russ McKendrick, 2023). Full-text fidelity to those books is not claimed.
