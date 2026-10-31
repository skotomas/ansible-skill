# Containers

Use a compatible community.docker collection and verify each module's execution dependencies with ansible-doc. For Compose use community.docker.docker_compose_v2; the old docker_compose module was removed. Verify the required Docker CLI/Compose version on the execution host.
Use reviewed images and explicit versions/digests, persistent volumes where needed, minimal published ports and supported TLS trust. Avoid privileged mode and unnecessary Docker socket access; Docker administration can confer host-level privilege.
Keep credentials out of playbooks and build arguments. Use existing credential injection and no_log for secret-consuming tasks; environment secrets remain visible to users with Docker inspection access.
For builds use immutable tags/digests and test the resulting artifact. For runtime changes account for recreation and downtime. Prefer container-native configuration; choose Packer for VM/golden-image requirements rather than a blanket complexity rule.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections/community/docker/docker_compose_v2_module.html). Verify against the selected installed versions.
