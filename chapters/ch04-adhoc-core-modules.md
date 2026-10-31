# Modules and command execution

Use FQCN and consult ansible-doc for parameters, return values, dependencies and check/diff support. Package names and capabilities still vary by OS behind the package wrapper.
Prefer command with argv for external programs; use shell only when shell syntax is needed. Quote interpolated shell arguments with the quote filter and avoid untrusted command text. command avoids shell expansion, but not dangerous program arguments. raw is useful for bootstrap without Python.
Use creates/removes only when the marker actually represents completion. Use changed_when and failed_when to describe genuine behavior. Do not conceal updates with changed_when: false.
Set explicit owner/group and quoted file modes where access matters. Validate configurations before replacement where the module supports validate. Ad-hoc operations should have explicit targets; use a playbook for reviewable repeated work.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/command_module.html). Verify against the selected installed versions.
