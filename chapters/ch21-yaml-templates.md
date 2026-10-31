# YAML file templates

Canonical, currently valid YAML for every file type in an Ansible project. Copy the shape, adjust values. Rules that the examples below follow:

- FQCN for every module and plugin.
- Lowercase booleans (`true`/`false`).
- Quote octal modes (`'0644'`) and any string starting with a Jinja expression (`"{{ ... }}"`).
- Facts via dictionary access (`ansible_facts['eth0']['ipv4']['address']`), not deprecated dotted form.
- Lists of maps for `loop`; `with_*` loops are legacy.
- One concern per file; playbooks declare state, not procedure.

## Inventory — `inventory/production.yml`

```yaml
all:
  children:
    webservers:
      hosts:
        web01:
          ansible_host: 10.0.1.11
        web02:
          ansible_host: 10.0.1.12
    dbservers:
      hosts:
        db01:
          ansible_host: 10.0.2.11
    office:
      children:
        webservers:
        dbservers:
      vars:
        ansible_user: deploy
        ansible_python_interpreter: /usr/bin/python3
```

Ad-hoc check: `ansible-inventory -i inventory/production.yml --list --yaml`.

## group_vars — `group_vars/webservers.yml`

```yaml
---
nginx_worker_processes: "{{ ansible_facts['processor_count'] }}"
app_version: "1.4.2"
listen_port: 8080
```

Host-specific overrides go to `host_vars/web01.yml` with the same shape. Secrets belong out of the repository or in encrypted files (chapter 10).

## Minimal playbook — `site.yml`

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: true
  gather_facts: true

  tasks:
    - name: Install nginx
      ansible.builtin.package:
        name: nginx
        state: present

    - name: Render site configuration
      ansible.builtin.template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: '0644'
        validate: /usr/sbin/nginx -t -c %s
      notify: Reload nginx
```

`validate:` runs before the file is placed; `notify` fires the handler once at the end of the play.

## Handlers — `handlers/main.yml`

```yaml
---
- name: Reload nginx
  ansible.builtin.service:
    name: nginx
    state: reloaded
```

## Role layout

```text
roles/webapp/
├── defaults/main.yml      # overridable inputs
├── files/                 # static files (copy: src)
├── handlers/main.yml
├── meta/main.yml          # dependencies, metadata
├── tasks/main.yml         # entry point
├── templates/             # Jinja2 (template: src)
├── vars/main.yml          # internal, wins over inventory
└── README.md
```

Generate the skeleton with `ansible-galaxy init roles/webapp`. Role file names are fixed by convention; do not rename them.

## Role tasks — `roles/webapp/tasks/main.yml`

```yaml
---
- name: Create application user
  ansible.builtin.user:
    name: "{{ app_user }}"
    system: true
    shell: /usr/sbin/nologin

- name: Deploy application archive
  ansible.builtin.unarchive:
    src: "app-{{ app_version }}.tar.gz"
    dest: /opt/webapp
    owner: "{{ app_user }}"
    remote_src: false
  notify: Restart webapp

- name: Wait for application to answer
  ansible.builtin.wait_for:
    port: "{{ listen_port }}"
    timeout: 60
```

## defaults — `roles/webapp/defaults/main.yml`

```yaml
---
app_user: webapp
app_version: "1.0.0"
listen_port: 8080
```

Every default must be overridable; callers set these from group_vars or play vars.

## meta — `roles/webapp/meta/main.yml`

```yaml
---
dependencies:
  - role: common
    when: ansible_facts['os_family'] in ['Debian', 'RedHat']
```

Keep dependencies to genuine prerequisites; implicit coupling through variables is a smell.

## Requirements — `requirements.yml`

```yaml
---
roles:
  - name: geerlingguy.nginx
    version: 3.1.4

collections:
  - name: community.general
    version: ">=9.0.0,<10.0.0"
  - name: ansible.posix
    version: 1.5.4
```

Install: `ansible-galaxy role install -r requirements.yml -p roles/` and `ansible-galaxy collection install -r requirements.yml`. Commit the file; never install unpinned in CI.

## ansible.cfg (project root, committed)

```ini
[defaults]
inventory = inventory/production.yml
roles_path = roles
host_key_checking = true
forks = 20
callbacks_enabled = ansible.posix.profile_tasks

[ssh_connection]
pipelining = true
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
```

## Conditionals, loops, and error handling

```yaml
- name: Install packages per OS family
  ansible.builtin.package:
    name: "{{ item }}"
    state: present
  loop: "{{ packages[ansible_facts['os_family']] | default([]) }}"
  when: ansible_facts['os_family'] in packages

- name: Register and inspect a result
  ansible.builtin.command: /usr/bin/appctl status
  register: status
  changed_when: false
  failed_when: status.rc not in [0, 3]

- name: Retry a transient remote fetch
  ansible.builtin.get_url:
    url: "{{ mirror }}/app-{{ app_version }}.tar.gz"
    dest: /tmp/app.tar.gz
    mode: '0644'
  retries: 3
  delay: 10
  until: fetch_result is succeeded
  register: fetch_result

- name: Guarded section with cleanup
  block:
    - name: Migrate database
      ansible.builtin.command: /opt/webapp/migrate
      changed_when: true
  rescue:
    - name: Report failure with context
      ansible.builtin.debug:
        msg: "Migration failed: {{ ansible_failed_result }}"
  always:
    - name: Remove staging directory
      ansible.builtin.file:
        path: /tmp/stage
        state: absent
```

## Template — `templates/nginx.conf.j2`

```jinja
worker_processes {{ nginx_worker_processes }};

server {
    listen {{ listen_port }};
    server_name {{ inventory_hostname }};

    location /static/ {
        alias /opt/webapp/static/;
    }
}
```

Variables resolve at render time; `inventory_hostname` and facts are always available without definition.

## Validation of YAML itself

- `ansible-playbook site.yml --syntax-check` — parse and structure check.
- `ansible-lint` — style, deprecated keys, risky patterns.
- `ansible-inventory -i ... --list` — resolves inventory and group_vars merge; the fastest way to verify variable scoping.

Common YAML failures: tabs instead of spaces; unquoted `{{` at the start of a value (parsed as a mapping); unquoted octal modes turned into decimal; `yes`/`no` treated as booleans in older parsers — write `true`/`false`.
