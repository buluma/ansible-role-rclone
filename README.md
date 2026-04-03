# [Ansible role rclone](#ansible-role-rclone)

Install rclone on your system.

|GitHub|Issues|Pull Requests|Version|Downloads|
|------|------|-------------|-------|---------|
|[![github](https://github.com/buluma/ansible-role-rclone/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-rclone/actions/workflows/molecule.yml)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/pulls/)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/releases/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/rclone)](https://galaxy.ansible.com/ui/standalone/roles/buluma/rclone/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- become: true
  hosts: all
  name: Converge
  pre_tasks:
    - apt: update_cache=true cache_valid_time=600
      name: Update apt cache.
      when: ansible_os_family == 'Debian'
    - changed_when: false
      command: systemctl is-system-running
      delay: 5
      name: Wait for systemd to complete initialization.
      register: systemctl_status
      retries: 30
      until:
        "'running' in systemctl_status.stdout or 'degraded' in systemctl_status.stdout

        "
      when: ansible_distribution == 'Fedora'
    - ansible.builtin.debug:
        msg: "Ansible Version: {{ ansible_version.full }}"
      name: Show ansible version
  roles:
    - role: buluma.rclone
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/molecule/default/prepare.yml):

```yaml
---
- become: true
  gather_facts: false
  hosts: all
  name: Prepare
  roles:
    - role: buluma.bootstrap
    - role: buluma.core_dependencies
    - role: buluma.ca_certificates
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/defaults/main.yml):

```yaml
---
install_manpages: true
rclone_arch: amd64
rclone_config_location: /root/.config/rclone/rclone.conf
rclone_man_pages:
  GROUP: root
  OWNER: root
rclone_packages:
  - unzip
rclone_release: stable
rclone_setup_tmp_dir: /tmp/rclone_setup
rclone_version: '{{ ansible_local.rclone.version | d("0.0.0") }}'
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-rclone/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub |
|-------------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Build Status GitHub](https://github.com/buluma/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Build Status GitHub](https://github.com/buluma/ansible-role-ca_certificates/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-rclone/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/robertdebock/enterpriselinux)|all|
|[Debian](https://hub.docker.com/r/robertdebock/debian)|all|
|[Fedora](https://hub.docker.com/r/robertdebock/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/robertdebock/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-rclone/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-rclone/blob/master/LICENSE).

## [Author Information](#author-information)

[Michael buluma](https://buluma.github.io/)

