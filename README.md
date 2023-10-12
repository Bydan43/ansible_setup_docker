## Ansible role: Ansible_setup_docker

An Ansible Role that installs [Docker](https://www.docker.com) on Linux.

- Install Docker (editions, channels and version pinning are all supported)
- Install Docker Compose v2
- Install the `docker` PIP package so Ansible's `docker_*` modules work

## Supported platforms

- Ubuntu 20.04 LTS (Focal Fossa)
- Ubuntu 22.04 LTS (Jammy Jellyfish)
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)

---

## Quick start

### What's configured by default?

The latest stable release of Docker CE and Docker Compose v2 

### Example playbook

```yml
---

- hosts: linux

  become: true

  pre_tasks:

    - name: Pre_tasks | Check for Python
      ansible.builtin.raw: which /usr/bin/python3
      changed_when: false
      failed_when: false
      register: check_python

    - name: Pre_tasks | Install Python
      ansible.builtin.raw: which /usr/bin/apt && apt-get -y update && apt-get install -y python3
      when: check_python.rc != 0

    - name: Pre_tasks | install lsb-release
      ansible.builtin.apt:
        name: lsb-release
        update_cache: true
        state: present


  roles:
    - ansible_setup_docker
```