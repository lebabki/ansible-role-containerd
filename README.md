# Ansible Role: Containerd

[![CI](https://github.com/lebabki/ansible-role-containerd/workflows/CI/badge.svg?event=push)](https://github.com/lebabki/ansible-role-containerd/actions?query=workflow%3ACI)

An Ansible Role that installs [containerd](https://containerd.io) on Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    containerd_package: containerd.io
    containerd_package_state: present

Package name and state controls.

    containerd_service_state: started
    containerd_service_enabled: true

Service controls. You can install containerd but not have it running or enabled on boot by changing these defaults.

    containerd_config_default_write: true

Write containerd defaults to the containerd config.toml file.

    containerd_config_cgroup_driver_systemd: false

Set systemd as cgroup driver in config.toml. Only valid with `containerd_config_default_write: true`

    docker_apt_release_channel: stable
    docker_apt_arch: '{{ (ansible_architecture == "aarch64") | ternary("arm64", "amd64") }}'
    docker_apt_repository: "deb [arch={{ docker_apt_arch }}] https://download.docker.com/linux/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} {{ docker_apt_release_channel }}"
    docker_apt_ignore_key_error: true
    docker_apt_gpg_key: https://download.docker.com/linux/{{ ansible_distribution | lower }}/gpg

Set grpc socket owner group in config.toml. Only valid with `containerd_config_default_write: true`

    containerd_config_grpc_gid: 0

Apt installation paramemeters, useful if you want to switch from the stable channel releases, or install on a different CPU architecture (e.g. `arm64`).

    docker_yum_repo_url: https://download.docker.com/linux/{{ (ansible_distribution == "Fedora") | ternary("fedora","centos") }}/docker-ce.repo
    docker_yum_repo_enable_nightly: '0'
    docker_yum_gpg_key: https://download.docker.com/linux/centos/gpg

Experimental support for RHEL/Centos Stream 9. Use this until this [issue](https://github.com/docker/roadmap/issues/303) is resolved. By default it's enabled only for `ansible_distribution_major_version == 9` and includes workaround for changing `$releasever` to `8` for dnf repositories.

    docker_yum_repo_url_fallback_to_8: true
    docker_yum_repo_url_tmp_filepath: /tmp/ansible-role-containerd-docker-ce.repo
    docker_yum_repo_url_releasever_var_name: dockerreleasever

Yum/DNF installation parameters, useful if you want to switch from the stable repository.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - ansible-role-containerd
```

## License

MIT / BSD

## Author Information

This role was originally created in 2021 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
