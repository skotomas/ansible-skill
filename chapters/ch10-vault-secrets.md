# Secrets without mandatory Vault

This user's environment has no established Vault workflow. Do not deploy a secret manager or require Vault as part of ordinary Ansible work.
Choose the smallest suitable existing mechanism:
- SSH keys/agent for login; --ask-become-pass for interactive sudo when needed.
- Private vars_prompt for occasional interactive application secrets. Prompts do not work as an unattended CI secret source.
- Existing CI/AWX credential injection or an existing secret manager for automated runs.
- A local secrets file outside the repository, directory 0700 and file 0600, when acceptable for this workstation. Pass its path as a vars file, never its contents in command-line arguments. Plaintext at rest remains a tradeoff; protect disk, backups and account access.
Ansible Vault is an optional CLI file-encryption feature shipped with Ansible, requiring no server. HashiCorp Vault is a separate service. Offer Ansible Vault when encrypted files in Git are actually useful; do not conflate the two.
Protect secret-consuming tasks with no_log, secret file diffs with diff: false and restrictive file permissions. Do not dump hostvars or enable ANSIBLE_DEBUG with production secrets. Masking does not protect arbitrary later debug output or external applications' logs. Environment variables are not automatically safe storage.
If using Ansible Vault, use encrypt_string --prompt or protected stdin, not literal secret arguments. Keep the encryption password separate. rekey changes file encryption only; rotate/revoke underlying credentials after compromise and account for old Git ciphertext. Inline vaulted values need re-encryption rather than file rekey.
For Linux user passwords, store a suitable precomputed hash securely or use a deliberate stable hashing strategy. Random salt on every evaluation creates needless password changes. Choose update_password behavior according to the intended rotation policy.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/vault_guide/vault.html). Verify against the selected installed versions.
