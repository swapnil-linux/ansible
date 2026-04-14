# ansible-role-docker

Ansible role to install Docker CE on RHEL/CentOS/AlmaLinux/Rocky Linux (EL8/EL9).

## What it does

1. Installs prerequisite packages (`yum-utils`, `device-mapper-persistent-data`, `lvm2`)
2. Adds the official Docker CE repository
3. Installs `docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-buildx-plugin`, and `docker-compose-plugin`
4. Starts and enables the Docker service

## Requirements

- RHEL / CentOS / AlmaLinux / Rocky Linux 8 or 9
- Ansible 2.12+
- `become: true` (the role requires root privileges)

## Role variables

Defined in `defaults/main.yml`:

| Variable          | Default value                                                      | Description                    |
|-------------------|--------------------------------------------------------------------|--------------------------------|
| `docker_repo_url` | `https://download.docker.com/linux/centos/docker-ce.repo`         | URL of the Docker CE repo file |

## Example playbook

```yaml
---
- name: Install Docker
  hosts: all
  become: true
  roles:
    - docker
```

Or with a custom repo URL:

```yaml
---
- name: Install Docker
  hosts: all
  become: true
  roles:
    - role: docker
      vars:
        docker_repo_url: https://download.docker.com/linux/centos/docker-ce.repo
```

## License

GPLv2

## Author

Swapnil Jain — swapnil@linux.com
