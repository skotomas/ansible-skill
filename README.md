# Ansible Skill

Agent skill for writing, reviewing and troubleshooting Ansible inventories, playbooks, roles and automation workflows — with version-aware validation and safe handling of access and secrets.

Works with agents that support the [skills format](https://agentskills.io) (OpenCode, Claude Code, GitHub Copilot CLI, Amp, etc.).

## What it does

When loaded, the skill guides the agent through a disciplined Ansible workflow:

- Inspect existing inventory, roles, configuration and dependencies before changing anything
- Prefer state modules with fully qualified collection names (FQCN); scope privilege escalation to need
- Validate proportionally: parse/lint, syntax and host selection, representative staging, repeat-run behavior
- Treat `--check` as **not** a sandbox; protect secret diffs and logs
- Verify version-sensitive names and parameters against installed `ansible-doc` and official docs — not against old book snippets

## Structure

| File | Purpose |
|---|---|
| `SKILL.md` | Entry point: working method, operational invariants, topic index |
| `chapters/` | 21 topic chapters, read on demand (only what the task needs) |
| `cheatsheet.md` | Quick reference |
| `glossary.md` | Terminology |
| `patterns.md` | Common patterns |

### Chapters

01 Foundations · 02 Installation and configuration · 03 Inventory · 04 Modules and command execution · 05 Playbooks · 06 Variables and facts · 07 Control flow and rolling changes · 08 Roles and reuse · 09 Collections and dependencies · 10 Secrets without mandatory Vault · 11 Custom modules and plugins · 12 Troubleshooting · 13 Validation · 14 Performance · 15 Cloud and Terraform · 16 Containers · 17 Kubernetes · 18 Security and hardening · 19 CI, AWX and shared execution · 20 Office cluster workflow · 21 YAML file templates

## Installation

Clone into your agent's skills directory:

```sh
# OpenCode
git clone git@github.com:CKOTOMAC/ansible-skill.git \
  ~/.config/opencode/skills/ansible

# Claude Code
git clone git@github.com:CKOTOMAC/ansible-skill.git \
  ~/.claude/skills/ansible
```

Or install just the skill files (the `.git/` directory is not required).

## Usage

The skill triggers automatically when the agent works on Ansible-related tasks — playbooks, inventories, roles, collections, Vault, troubleshooting. You can also invoke it explicitly, e.g. `/ansible` in OpenCode.

## Notes

- This is maintained engineering guidance distilled from books and practice, not a verbatim reproduction of any book.
- Chapter 10 covers secrets handling that works without a mandatory HashiCorp Vault setup.
- Chapter 20 documents a specific office cluster workflow — adapt or remove it if it does not match your environment.
