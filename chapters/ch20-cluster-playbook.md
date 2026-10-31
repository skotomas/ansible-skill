# Office cluster workflow

Start from the actual inventory, OS versions, access and availability requirements. Do not assume a cloud, Kubernetes, Vault or AWX installation exists.
Use inventories/staging and inventories/production with consistent functional groups; plays target those groups, not a hardcoded environment name. Pass inventory explicitly for operational commands. Check --list-hosts before changes and require the expected nonempty target set.
Implement cohesive roles for needed baseline and services. Establish administrative access before SSH/firewall restrictions. Validate configuration before notifying handlers and check health after handlers run.
Use a canary and batch size based on quorum/capacity. Include application-specific health checks and a recovery path. A generic HTTP check is not appropriate for every database or host.
Local interactive secrets can come from prompts or a protected file outside Git; use existing credential injection for unattended automation. See chapter 10 before adding any new secret infrastructure.
Validate syntax/lint and run in staging where representative; review check-mode limitations. Production deployment remains a separate action with the scope authorized by the user. Document actual test results and residual limits, not a claim of universal production readiness.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/tips_tricks/sample_setup.html). Verify against the selected installed versions.
