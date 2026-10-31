# Inventory

Prefer separate production and staging inventories when that reduces accidental targeting. Give both the same functional groups, such as webservers, so the same playbook works unchanged. `-i` changes the inventory; it does not replace the play hosts pattern. `--limit` intersects that pattern and cannot widen it.
Keep environment variables adjacent to their inventory in group_vars/host_vars. Verify nonempty intended targets with --list-hosts and inspect group structure with ansible-inventory --graph. --list may expose secrets.
Use aliases with ansible_host and ansible_port for endpoints sharing an IP. YAML supports structured data; INI inline host variables can also contain Python literal types, while group :vars values have different parsing rules.
Use inventory plugins for authoritative cloud data. add_host affects later plays; refresh_inventory can refresh a dynamic source. A YAML value $NODE1IP is literal text, not automatic shell substitution.
Use interpreter discovery or an explicit verified interpreter path appropriate to the host.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/inventory_guide/intro_inventory.html). Verify against the selected installed versions.
