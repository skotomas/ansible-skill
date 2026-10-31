# Cloud and Terraform

Choose one owner for each resource. Terraform plus Ansible is useful for resource lifecycle plus configuration, but Ansible can also provision through cloud modules. Ansible corrects declared state on a run without needing a Terraform state file.
Use least-privilege automation identities, existing credential providers and explicit account/region/environment selection. Protect Terraform state and plan artifacts as sensitive; use a backend with suitable concurrency protection for shared automation.
Consume actual outputs consistently via dynamic inventory or add_host. Wait for connection readiness and application readiness separately. Define health, rollback and teardown scope before executing an update.
Do not infer that replacing hosts is safe for local state: migrate/replicate data and verify backups. Resolve dependencies explicitly and do not let multiple tools manage the same object concurrently.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections/amazon/aws/index.html). Verify against the selected installed versions.
