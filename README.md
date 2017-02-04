Role Name
=========

An [Ansible] role to install [Redis]

Requirements
------------

None

Role Variables
--------------

```
---
# defaults file for ansible-redis
redis_bind_addresses:
  - '0.0.0.0'
  # - '127.0.0.1'
  # - '{{ ansible_default_ipv4.address }}'
  # - '{{ ansible_all_ipv4_addresses }}'
  # - '{{ ansible_all_ipv6_addresses }}'
  ## Use the method below if you define redis_bind_interfaces instead of
  ## configuring bind addresses using one of the methods above
  # - >
  #   {% for item in redis_bind_interfaces %}
  #   {{ hostvars[inventory_hostname]['ansible_' + item]['ipv4']['address'] }}
  #   {% if not loop.last %}{% endif %}{% endfor %}
redis_bind_interfaces: [] # Define interfaces to bind to if ip addresses are not known
  # - 'enp0s3'
  # - 'enp0s8'
redis_cluster: false # ***NOT WORKING YET
redis_config_redis: true # Defines if Redis is configured
redis_daemonize: true
redis_dl_info:
  dest_dir: '/usr/local/src'
  file: 'redis-{{ redis_version }}.tar.gz'
  url: 'http://download.redis.io/releases'
redis_install_dir: '/var/lib/redis'

# Defines if installed from source rather than package manager
redis_install_from_source: true

redis_max_clients: '10000' # Set the max number of connected clients at the same time
redis_port: '6379'

# Redis Replication Settings
#
# Enable replication (true|false)
redis_replication: false
# Define the master replication address
redis_replication_address: "{{ hostvars[inventory_hostname]['ansible_' + redis_replication_interface]['ipv4']['address'] }}"
# Define the Ansible group which the nodes belong to for replication
redis_replication_ansible_group: 'redis-replication'
# Define the interface which should be used for replication
# ex. eth1|enp0s3|enp0s8
redis_replication_interface: 'eth1'
# Define the node which should act as primary
redis_replication_master: "{{ groups[redis_replication_ansible_group][0] }}"

redis_sysctl_settings:
  - name: 'net.core.somaxconn'
    value: '{{ redis_tcp_backlog }}'
  - name: 'vm.overcommit_memory'
    value: '1'
redis_syslog_logging: false # enable logging to the system logger
redis_tcp_backlog: '511'
redis_user_info:
  home: '{{ redis_install_dir }}'
  name: 'redis'
redis_version: '3.2.7'
```

Dependencies
------------

None

Example Playbook
----------------

```
---
- hosts: all
  become: true
  vars:
  roles:
    - ansible-redis
  tasks:
```

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Ansible]: <https://www.ansible.com>
[Redis]: <https://redis.io>
