# CI, AWX and shared execution

A small team can use controlled CLI runs; CI/AWX/AAP are choices based on audit, scheduling and access needs, not a mandatory consequence of adding a second user.
Use tested environments and dependencies, reviewed code, minimal token permissions and existing secret injection. Keep host key checking enabled and supply verified known_hosts. Do not expose production credentials to untrusted pull-request code.
Separate validation and deployment permissions. Preserve command failures through log pipelines, for example with an explicit bash shell and pipefail. Restrict artifacts containing logs or resolved configuration. Cache keys must include dependency/toolchain inputs; caches do not replace installation checks.
For AWX follow the supported Operator upgrade path. Back up database and associated secrets and test restoration; an external database is an architectural choice, not an automatic prerequisite. Do not tear down the cluster as a standard upgrade step.
Use compatible versioned execution environments and requirements. Full-file encrypted vars work in AWX with suitable credentials; inline Vault strings are not required. Existing AWX/CI credentials can eliminate the need for Vault files.

Reference: [official documentation](https://docs.ansible.com/projects/awx-operator/en/latest/upgrade/upgrading.html). Verify against the selected installed versions.
