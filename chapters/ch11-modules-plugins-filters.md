# Custom modules and plugins

Prefer existing maintained modules before writing custom ones. Use AnsibleModule for argument validation, no_log on sensitive arguments, and structured exit_json/fail_json results.
For state-changing modules inspect current state, compute whether change is needed, report predicted change in check mode, and mutate only outside check mode. Merely setting supports_check_mode does not implement these semantics.
Provide DOCUMENTATION, EXAMPLES and RETURN and test real state transitions, repeat runs, check mode and failure behavior. Avoid logging secrets in exceptions or returning them as unrelated result fields.
Lookups and filters execute on the controller; review their side effects, permissions and data disclosure. Do not send infrastructure data to external APIs without task-specific authorization. Use collection plugin directories for maintained shared code.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/dev_guide/developing_modules_general.html). Verify against the selected installed versions.
