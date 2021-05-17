[![tests](https://github.com/boutetnico/ansible-role-telegraf/workflows/Test%20ansible%20role/badge.svg)](https://github.com/boutetnico/ansible-role-telegraf/actions?query=workflow%3A%22Test+ansible+role%22)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-boutetnico.telegraf-blue.svg)](https://galaxy.ansible.com/boutetnico/telegraf)


ansible-role-telegraf
=====================

This role installs and configures [Telegraf](https://docs.influxdata.com/telegraf/latest/).

Requirements
------------

Ansible 2.7 or newer.

Supported Platforms
-------------------

- [Debian - 9 (Stretch)](https://wiki.debian.org/DebianStretch)
- [Debian - 10 (Buster)](https://wiki.debian.org/DebianBuster)
- [Ubuntu - 18.04 (Bionic Beaver)](http://releases.ubuntu.com/18.04/)
- [Ubuntu - 20.04 (Focal Fossa)](http://releases.ubuntu.com/20.04/)

Role Variables
--------------

| Variable                                     | Required | Default                       | Choices | Comments                 |
|----------------------------------------------|----------|-------------------------------|---------|--------------------------|
| telegraf_dependencies                        | yes      | `[apt-transport-https,gnupg]` | list    |                          |
| telegraf_package_state                       | yes      | `true`                        | bool    | Use `latest` to upgrade. |
| telegraf_config_path                         | yes      | `/etc/telegraf`               | string  |                          |
| telegraf_global_tags                         | yes      | `{}`                          | dict    |                          |
| telegraf_agent_interval                      | yes      | `10s`                         | string  |                          |
| telegraf_agent_round_interval                | yes      | `true`                        | bool    |                          |
| telegraf_agent_metric_batch_size             | yes      | `1000`                        | int     |                          |
| telegraf_agent_metric_buffer_limit           | yes      | `10000`                       | int     |                          |
| telegraf_agent_collection_jitter             | yes      | `0s`                          | string  |                          |
| telegraf_agent_flush_interval                | yes      | `10s`                         | string  |                          |
| telegraf_agent_flush_jitter                  | yes      | `0s`                          | string  |                          |
| telegraf_agent_precision                     | yes      | ``                            | string  |                          |
| telegraf_agent_debug                         | yes      | `false`                       | bool    |                          |
| telegraf_agent_quiet                         | yes      | `false`                       | bool    |                          |
| telegraf_agent_logtarget                     | yes      | `file`                        | string  |                          |
| telegraf_agent_logfile                       | yes      | ``                            | string  |                          |
| telegraf_agent_logfile_rotation_interval     | yes      | `0d`                          | string  |                          |
| telegraf_agent_logfile_rotation_max_size     | yes      | `0MB`                         | string  |                          |
| telegraf_agent_logfile_rotation_max_archives | yes      | `5`                           | int     |                          |
| telegraf_agent_log_with_timezone             | yes      | ``                            | string  |                          |
| telegraf_agent_hostname                      | yes      | ``                            | string  |                          |
| telegraf_agent_omit_hostname                 | yes      | `false`                       | bool    |                          |
| telegraf_outputs                             | yes      | `[]`                          | list    | See `defaults/main.yml`. |
| telegraf_inputs                              | yes      | `[]`                          | list    | See `defaults/main.yml`. |


Dependencies
------------

None

Example Playbook
----------------

    - hosts: all
      roles:
        - role: ansible-role-telegraf

          telegraf_outputs:
            - name: influxdb_v2
              config:
                - urls = ["unix:///var/run/influxdb.sock"]
                - token = "abcd1234"
                - organization = "MyOrg"
                - bucket = "telegraf"

          telegraf_inputs:
            - name: cpu
              config:
                - percpu = false
              tags:
                - tag1 = "foo"
                - tag2 = "bar"
            - name: disk
              config:
                - mount_points = ["/", "/data"]

            - name: cloudwatch
              config:
                - tagexclude = ["region", "instance_id"]
                - region = "us-east-1"
                - access_key = "ACCESS_KEY"
                - secret_key = "SECRET_KEY"
                - period = "5m"
                - delay = "5m"
                - interval = "5m"
                - namespace = "AWS/EC2"
                - metrics:
                  config:
                    - names = ["CPUCreditBalance"]
                    - dimensions:
                      config:
                        - name = "InstanceId"
                        - value = "i-1234"

Testing
-------

    molecule test

License
-------

MIT

Author Information
------------------

[@boutetnico](https://github.com/boutetnico)
