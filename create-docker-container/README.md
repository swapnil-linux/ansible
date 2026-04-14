# Create Docker Container using Ansible

This example demonstrates how to use Ansible to create a Docker container and manage it as a systemd service.

## What it does

1. Installs the `python3-docker` package (required by the `community.docker.docker_container` module)
2. Creates a Docker container from the image defined in `vars.yml`
3. Generates a systemd unit file from the `systemd.j2` template
4. Enables and starts the container as a systemd service

## Requirements

- Docker must already be installed on the target host
- The `community.docker` collection must be installed:

```bash
ansible-galaxy collection install community.docker
```

## Usage

Edit `vars.yml` to set your container configuration, then run:

```bash
ansible-playbook playbook.yml
```

## Variables (`vars.yml`)

| Variable   | Description                        | Default                  |
|------------|------------------------------------|--------------------------|
| `image`    | Docker image to use                | `swapnillinux/apache-php`|
| `name`     | Container name                     | `myweb`                  |
| `src_port` | Host port                          | `8081`                   |
| `dest_port`| Container port                     | `80`                     |
| `src_vol`  | Host volume path                   | `/mnt/www`               |
| `dest_vol` | Container volume mount point       | `/var/www/html`          |
| `privileged` | Run container in privileged mode | `true`                   |

## Key concepts demonstrated

- `vars_files` — loading variables from a separate file
- `community.docker.docker_container` — managing containers declaratively
- `ansible.builtin.template` — generating config files from Jinja2 templates
- `ansible.builtin.systemd` — managing services and reloading the daemon
- `when` — conditional task execution based on OS family
- `register` + `debug` — capturing and printing command output
