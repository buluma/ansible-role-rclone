# Ansible role [rclone](https://galaxy.ansible.com/ui/standalone/roles/buluma/rclone/documentation)

Install rclone on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-rclone/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-rclone/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-rclone.svg)](https://github.com/buluma/ansible-role-rclone/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/rclone)](https://galaxy.ansible.com/ui/standalone/roles/buluma/rclone/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true

  pre_tasks:

    - name: Update apt cache.
      apt: update_cache=true cache_valid_time=600
      when: ansible_os_family == 'Debian'

    - name: Wait for systemd to complete initialization.     # noqa 303
      command: systemctl is-system-running
      register: systemctl_status
      until: >
        'running' in systemctl_status.stdout or
        'degraded' in systemctl_status.stdout
      retries: 30
      delay: 5
      when: ansible_distribution == 'Fedora'
      changed_when: false

    - name: Show ansible version
      ansible.builtin.debug:
        msg: "Ansible Version: {{ ansible_version.full }}"

  roles:
    - role: buluma.rclone
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
  - role: buluma.bootstrap
  # - role: buluma.openssl
  - role: buluma.core_dependencies
  - role: buluma.ca_certificates
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-rclone/blob/master/defaults/main.yml):

```yaml
---
# rclone_arch can be defined as an architecture (e.g. arm, mips, 386) listed at https://github.com/ncw/rclone/releases
rclone_arch: amd64

# release of rclone to use. 'default' or 'beta' are accepted
rclone_release: stable

rclone_version: '{{ ansible_local.rclone.version | d("0.0.0") }}'

install_manpages: true

# Defaults in case no variables for OS are chosen
rclone_setup_tmp_dir: "/tmp/rclone_setup"

# The location to install the config file if configured
rclone_config_location: "/root/.config/rclone/rclone.conf"

rclone_packages:
  - unzip

rclone_man_pages:
  OWNER: root
  GROUP: root
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-rclone/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Ansible Molecule](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-openssl.svg)](https://github.com/shadowwalker/ansible-role-openssl)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ca_certificates.svg)](https://github.com/shadowwalker/ansible-role-ca_certificates)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-rclone/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|8|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|
|[Kali](https://hub.docker.com/r/buluma/kali)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-rclone/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-rclone/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-rclone/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)
