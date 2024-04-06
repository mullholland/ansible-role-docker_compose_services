# [Ansible role docker_compose_services](#docker_compose_services)

Create docker-compose.yml and systemd services for various apps which do not need config files

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-docker_compose_services/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-docker_compose_services/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/docker_compose_services)](https://galaxy.ansible.com/mullholland/docker_compose_services)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-docker_compose_services.svg)](https://github.com/mullholland/ansible-role-docker_compose_services/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-docker_compose_services/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    docker_compose_services:
      - name: nginx
        state: present
        directories:
          - "data"
          - "config"
        compose:
          services:
            nginx:
              image: nginx:latest
              restart: always
              environment:
                PUID: "{{ docker_compose_services_uid }}"
                PGID: "{{ docker_compose_services_gid }}"
                TZ: "{{ docker_compose_services_timezone }}"
              volumes:
                - "{{ docker_compose_services_base_path }}/nginx/data:/usr/share/nginx/html"
                - "{{ docker_compose_services_base_path }}/nginx/config:/etc/nginx/conf"
              ports:
                - "9999:80"
      - name: nginx2
        state: present
        compose:
          services:
            nginx:
              image: nginx:latest
              restart: always
              environment:
                PUID: "{{ docker_compose_services_uid }}"
                PGID: "{{ docker_compose_services_gid }}"
                TZ: "{{ docker_compose_services_timezone }}"
              ports:
                - "9998:80"
      - name: nginx3
        state: absent
  roles:
    - role: "mullholland.docker_compose_services"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-docker_compose_services/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true
  vars:
    pip_packages:
      - "docker"

  roles:
    - role: mullholland.docker
    - role: mullholland.repository_epel
    - role: mullholland.pip
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-docker_compose_services/blob/master/defaults/main.yml):

```yaml
---
# General config
docker_compose_services_network_name: "web"
docker_compose_services_base_path: "/opt"
docker_compose_services_timezone: "Europe/Berlin"

# User/Group of the stack. Everything is mapped to this, instead of root.
docker_compose_services_user: "homelab"
docker_compose_services_uid: "900"
docker_compose_services_group: "homelab"
docker_compose_services_gid: "900"
docker_compose_services_user_system: true

docker_compose_services:
# - name: nginx (required)
#   state: present (required)
#   directories: (optional)
#     - "data"
#     - "config"
#   compose: (required)
#     services:
#       nginx:
#         image: nginx:latest
#         restart: always
#         environment:
#           PUID: "{{ docker_compose_services_uid }}"
#           PGID: "{{ docker_compose_services_gid }}"
#           TZ: "{{ docker_compose_services_timezone }}"
#         volumes:
#           - "{{ docker_compose_services_base_path }}/nginx/data:/usr/share/nginx/html"
#           - "{{ docker_compose_services_base_path }}/nginx/config:/etc/nginx/conf"
#         ports:
#           - "9999:80"
# - name: nginx2
#   state: absent
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-docker_compose_services/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.repository_epel](https://galaxy.ansible.com/mullholland/repository_epel)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-repository_epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-repository_epel/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel)|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-docker)|
|[mullholland.pip](https://galaxy.ansible.com/mullholland/pip)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-pip/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-pip/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-docker_compose_services/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|all|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-docker_compose_services/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-docker_compose_services/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
