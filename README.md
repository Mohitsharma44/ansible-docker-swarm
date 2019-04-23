Role Name
=========

Ansible role to install Docker CE (or EE, configurable by setting `docker_edition` variable) and setup swarm for
servers grouped under `docker_swarm_manager` and `docker_swarm_worker`

Requirements
------------

Debian based OS.

Role Variables
--------------
### variables for installing docker

``` yaml
docker_edition: 'ce'
docker_package: "docker-{{ docker_edition }}"
docker_package_state: present
docker_service_state: started
docker_service_enabled: true
docker_users: ['vagrant'] # change this to include users that you want to have access to docker client
```

> You can leave the below variables untouched
>
> ``` yaml
> docker_apt_release_channel: stable
> docker_apt_arch: amd64
> docker_apt_repository: "deb [arch={{ docker_apt_arch }}] https://download.docker.com/linux/{{ ansible_distribution|lower }} {{ ansible_distribution_release }} {{ docker_apt_release_channel }}"
> docker_apt_ignore_key_error: true
> ```

### Variables for installing docker-compose

``` yaml
docker_install_compose: true
docker_compose_version: "1.22.0"
docker_compose_path: /usr/local/bin/docker-compose
```

### For Swarm

``` yaml
docker_swarm_port: 2377
docker_swarm_network_interface: "eth0" # this should be the primary network interface over which the workers and masters will communicate
```

> You can leave the below variabled untouched
> ``` yaml
> docker_swarm_primary_manager_name: "{{ groups['docker_swarm_manager'] | first }}" # for selecting first manager as the primary manager
> ```

Dependencies
------------

N/A

Example Playbook
----------------
### This playbook will install docker and setup swarm

``` yaml
- hosts: all
  become: True
  vars:
    - docker_swarm_network_interface: enp0s8
  tasks:
    - name: Setup Docker and Docker Swarm
      include_role:
        name: docker-swarm
```


License
-------

MIT

Author Information
------------------

Developed for SONYC/ NYU by Mohit Sharma (Mohitsharma44@gmail.com)
