# Performance

Measure first with an installed timing callback such as ansible.posix.profile_tasks enabled through callbacks_enabled. There is no standard ansible-playbook --profile-tasks flag.
Batch package lists, disable unused fact gathering, and consider bounded caching with private storage and expiry. Do not cache secrets. Inspect existing SSH multiplexing settings before changing them; pipelining can help but depends on connection and become behavior.
Tune forks against controller capacity, target load and API limits. Keep linear for coordinated steps. Use free only for independent hosts; host_pinned allows up to forks active hosts and keeps each host's work progressing until its slot is released.
serial controls batches; throttle limits concurrency for a task. async requires later result checks when correctness depends on completion. Preserve SSH host verification while tuning performance.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/host_pinned_strategy.html). Verify against the selected installed versions.
