# Ansible Training Examples

A collection of Ansible playbooks, roles, and examples used for training delivery.
All examples target **RHEL / CentOS / AlmaLinux / Rocky Linux** and require **Ansible 2.12+**.

---

## Prerequisites

```bash
# Install required collections
ansible-galaxy collection install community.docker community.mysql ansible.posix
```

---

## Examples

### `localhost.yml` — Hello World

The simplest possible playbook. Good starting point for explaining inventory, plays, and tasks.

```bash
ansible-playbook localhost.yml
```

---

### `docker.yml` — Install Docker (simple playbook)

Installs Docker and starts the service on hosts in the `centos` inventory group.
Demonstrates `ansible.builtin.yum` and `ansible.builtin.service`.

```bash
ansible-playbook docker.yml -i your_inventory
```

---

### `docker-using-role.yml` — Install Docker via a role

Same outcome as `docker.yml` but using the reusable `roles/docker` role.
Good for showing the difference between ad-hoc tasks and roles.

```bash
ansible-playbook docker-using-role.yml
```

---

### `play_on_reachable_host.yml` — Run a play only on reachable hosts

Demonstrates:
- `delegate_to` — run a task on a different host (localhost) than the target
- `register` — capture task output into a variable
- `ignore_errors` — continue even if a task fails
- `group_by` — dynamically create groups at runtime
- `when` — conditional task execution

```bash
ansible-playbook play_on_reachable_host.yml -i your_inventory
```

---

### `create-docker-container/` — Create a Docker container with systemd management

End-to-end example: pull an image, create a container, and register it as a systemd service.

Demonstrates:
- `vars_files` — loading variables from an external file
- `community.docker.docker_container` — declarative container management
- `ansible.builtin.template` — Jinja2 templates
- `ansible.builtin.systemd` — systemd service and daemon-reload
- `when` — OS-family conditionals

```bash
cd create-docker-container/
ansible-playbook playbook.yml
```

See [create-docker-container/README.md](create-docker-container/README.md) for full details.

---

### `lamp-example/` — Deploy a full LAMP stack

A multi-tier playbook that deploys Apache + PHP on web servers and MariaDB on database servers.

Demonstrates:
- Multi-play playbooks (preflight, webservers, dbservers)
- `group_vars/` — automatic variable loading per host group
- `ansible.builtin.dnf` — package management
- `ansible.posix.firewalld` — firewall rules
- `ansible.posix.seboolean` — SELinux management
- `community.mysql.mysql_db` / `community.mysql.mysql_user` — database provisioning
- `handlers` — conditional service restarts
- Jinja2 templates with loops (`for host in groups['dbservers']`)

```bash
cd lamp-example/
# Edit inventory to match your environment
ansible-playbook playbook.yml
```

**Note:** Sensitive values like `upassword` should be stored in an Ansible Vault file in production:
```bash
ansible-vault create group_vars/vault.yml
```

---

### `roles/docker/` — Reusable Docker role

A structured Ansible role following Galaxy conventions.
Good for explaining role directory layout (`tasks/`, `defaults/`, `handlers/`, `meta/`, `tests/`).

```bash
ansible-galaxy install -r requirements.yml   # if published to Galaxy
# or use directly from the roles/ directory
```

---

## Key concepts index

| Concept | Where demonstrated |
|---|---|
| FQCN (Fully Qualified Collection Names) | All playbooks |
| `loop` (modern replacement for `with_items`) | `lamp-example/playbook.yml` |
| `vars_files` | `create-docker-container/playbook.yml` |
| `group_vars` | `lamp-example/group_vars/` |
| `register` + `debug` | `play_on_reachable_host.yml`, `create-docker-container/playbook.yml` |
| `when` conditionals | `create-docker-container/playbook.yml` |
| `delegate_to` | `play_on_reachable_host.yml` |
| `group_by` | `play_on_reachable_host.yml` |
| `handlers` | `lamp-example/playbook.yml` |
| `template` (Jinja2) | `create-docker-container/`, `lamp-example/` |
| Roles | `roles/docker/`, `docker-using-role.yml` |
| Collections | `community.docker`, `community.mysql`, `ansible.posix` |
| Ansible Vault (concept) | `lamp-example/group_vars/dbservers` |

---

## Author

Swapnil Jain — swapnil@linux.com
