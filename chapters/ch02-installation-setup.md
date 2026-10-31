# Installation and configuration

Identify `ansible --version`, controller/target Python, collection versions and project conventions before choosing dependencies. Check the supported release matrix; do not copy historical book version pins.
Use a supported distro package, project venv, pipx or execution environment as appropriate. Avoid sudo pip into system Python. Pin a compatible tested toolchain and schedule updates.
Ansible uses the first configuration file found: ANSIBLE_CONFIG, current directory, home, /etc/ansible. Environment settings can override file values. Inspect `ansible-config dump --only-changed`; a committed config alone does not guarantee identical environments.
Keep host key checking enabled. Provision independently verified known_hosts or host certificates. Use dedicated SSH identities and privilege escalation only where needed.
For YAML-like output use ansible.builtin.default with callback result_format=yaml where supported. The enablement key is callbacks_enabled; install the collection providing each callback.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/reference_appendices/release_and_maintenance.html). Verify against the selected installed versions.
