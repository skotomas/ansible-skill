# Terms with operational consequences

- **Idempotence:** repeating an operation with stable desired inputs preserves the desired state. Reporting changed=false alone does not establish it.
- **Check mode:** module-dependent simulation; not an execution sandbox.
- **Handler:** notified task executed at a flush point, normally only after a change.
- **Serial:** host batching; does not independently provide rollback or guarantee availability.
- **Role defaults / vars:** low-priority public inputs / higher-priority variables; neither is a security boundary.
- **Ansible Vault:** optional local file/value encryption, no server required.
- **HashiCorp Vault:** separate secret-management service, not required for Ansible Vault.
- **Execution environment:** packaged automation runtime with selected dependencies.
- **Requirements manifest:** requested dependencies; not automatically a fully resolved lockfile.

See [topic index](SKILL.md) for implementation guidance.
