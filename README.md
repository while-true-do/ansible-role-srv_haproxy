<!--
name: README.md
description: This file contains important information for the repository.
author: while-true-do.io
contact: hello@while-true-do.io
license: BSD-3-Clause
-->

<!-- github shields -->
[![Github (tag)](https://img.shields.io/github/tag/while-true-do/ansible-role-srv_haproxy.svg)](https://github.com/while-true-do/ansible-role-srv_haproxy/tags)
[![Github (license)](https://img.shields.io/github/license/while-true-do/ansible-role-srv_haproxy.svg)](https://github.com/while-true-do/ansible-role-srv_haproxy/blob/master/LICENSE)
[![Github (issues)](https://img.shields.io/github/issues/while-true-do/ansible-role-srv_haproxy.svg)](https://github.com/while-true-do/ansible-role-srv_haproxy/issues)
[![Github (pull requests)](https://img.shields.io/github/issues-pr/while-true-do/ansible-role-srv_haproxy.svg)](https://github.com/while-true-do/ansible-role-srv_haproxy/pulls)
<!-- travis shields -->
[![Travis (com)](https://img.shields.io/travis/com/while-true-do/ansible-role-srv_haproxy.svg)](https://travis-ci.com/while-true-do/ansible-role-srv_haproxy)
<!-- ansible shields -->
[![Ansible (min. version)](https://img.shields.io/badge/dynamic/yaml.svg?label=Min.%20Ansible%20Version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fwhile-true-do%2Fansible-role-srv_haproxy%2Fmaster%2Fmeta%2Fmain.yml&query=%24.galaxy_info.min_ansible_version&colorB=black)](https://galaxy.ansible.com/while_true_do/srv_haproxy)
[![Ansible (platforms)](https://img.shields.io/badge/dynamic/yaml.svg?label=Supported%20OS&url=https%3A%2F%2Fraw.githubusercontent.com%2Fwhile-true-do%2Fansible-role-srv_haproxy%2Fmaster%2Fmeta%2Fmain.yml&query=galaxy_info.platforms%5B*%5D.name&colorB=black)](https://galaxy.ansible.com/while_true_do/srv_haproxy)
[![Ansible (tags)](https://img.shields.io/badge/dynamic/yaml.svg?label=Galaxy%20Tags&url=https%3A%2F%2Fraw.githubusercontent.com%2Fwhile-true-do%2Fansible-role-srv_haproxy%2Fmaster%2Fmeta%2Fmain.yml&query=%24.galaxy_info.galaxy_tags%5B*%5D&colorB=black)](https://galaxy.ansible.com/while_true_do/srv_haproxy)

# Ansible Role: srv_haproxy

An Ansible Role to install and configure haproxy.

## Motivation

[HAProxy](http://www.haproxy.org/) is a reliable, high performance TCP/HTTP
Load Balancer. In the moment, you want to scale your applications, you will
need to have a look at load balancing.

## Description

This role installs and configure HAProxy.

- install packages
- enable service
- configure frontends
- configure backends
- configure listeners
- configure stats page
- setup firewalling

## Requirements

Used Modules:

-   [Ansible Package Module](https://docs.ansible.com/ansible/latest/modules/package_module.html)
-   [Ansible Firewalld Module](https://docs.ansible.com/ansible/latest/modules/package_module.html)
-   [Ansible Service Module](https://docs.ansible.com/ansible/latest/modules/package_module.html)
-   [Ansible Template Module](https://docs.ansible.com/ansible/latest/modules/package_module.html)

## Installation

Install from [Ansible Galaxy](https://galaxy.ansible.com/while_true_do/srv_haproxy)
```
ansible-galaxy install while_true_do.srv_haproxy
```

Install from [Github](https://github.com/while-true-do/ansible-role-srv_haproxy)
```
git clone https://github.com/while-true-do/ansible-role-srv_haproxy.git while_true_do.srv_haproxy
```

## Usage

### Role Variables

```
---
# defaults file for while_true_do.srv_haproxy

## Package Management
wtd_srv_haproxy_package: "haproxy"
# State can be present|latest|absent
wtd_srv_haproxy_package_state: "present"

## Configuration Management

# Global Settings
# The global settings configure parameters that apply to all servers running
# HAProxy.
wtd_srv_haproxy_conf_global:
  log: "127.0.0.1 local2"
  chroot: "/var/lib/haproxy"
  pidfile: "/var/run/haproxy.pid"
  maxconn: 4000
  user: "haproxy"
  group: "haproxy"
  daemon: true

# Default Settings
# The default settings configure parameters that apply to all proxy subsections
# in a configuration (frontend, backend, and listen).
wtd_srv_haproxy_conf_defaults:
  mode: "http"
  log: "global"
  options:
    - httplog
    - dontlognull
    - "forwardfor except 127.0.0.0/8"
    - redispatch
  maxconn: 3000
  retries: 3
  timeout_http_request: "10s"
  timeout_http_keep_alive: "10s"
  timeout_connect: "10s"
  timeout_queue: "60s"
  timeout_client: "60s"
  timeout_server: "60s"
  timeout_check: "10s"

# Stats Settings
# The stats settings configure the statistics page for monitoring purposes.
wtd_srv_haproxy_conf_stats:
  enabled: false
# bind_address: "*"
# bind_port: "8080"
# uri: "/haproxy?stats"
# realm: "<optional realm name>"
# auth: "<user>:<password>"
# admin: false
# refresh: "30s"
# hide_version: true
# show_desc: "<optional description>"
# show_legends: false

# Frontend Settings
# The frontend settings configure the servers' listening sockets for client
# connection requests.
# wtd_srv_haproxy_conf_frontends:
#   - name: ""
#     bind_address: "*"
#     bind_port: ""
#     default_backend: ""

# Backend Settings
# The backend settings specify the real server IP addresses as well as the
# load balancer scheduling algorithm.
# wtd_srv_haproxy_conf_backends:
#   - name: ""
#     balance: "roundrobin"
#     servers:
#       - name: ""
#         address: ""
#         port: ""
#         options: "check weight 1"

# Listen Settings
# The Listen settings configure a complete proxy (combination of frontend and
# backend).
# wtd_srv_haproxy_conf_listens:
#   - name: ""
#     bind_address: "*"
#     bind_port: ""
#     balance: "roundrobin"
#     servers:
#       - name: ""
#         address: ""
#         port: ""
#         options: "check weight 1"

## Service Management
wtd_srv_haproxy_service: "haproxy"
# State can be started|stopped
wtd_srv_haproxy_service_state: "started"
wtd_srv_haproxy_service_enabled: true

## Firewalld Management
wtd_srv_haproxy_fw_mgmt: true
# Ports will be generated dynamically from
# - wtd_srv_haproxy_conf_frontends
# - wtd_srv_haproxy_conf_stats
# - wtd_srv_haproxy_conf_listens
wtd_srv_haproxy_fw_ports: []
# State can be enabled|disabled
wtd_srv_haproxy_fw_state: "enabled"
# Zone can be according to defined zones on your machine.
wtd_srv_haproxy_fw_zone: "public"
```

### Example Playbook

Running Ansible
[Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)
can be done in a
[playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html).

#### Simple

A simple load balancer for 2 backends can be achieved with the below example.

```
---
- hosts: all
  roles:
    - role: while_true_do.srv_haproxy
      wtd_srv_haproxy_conf_listens:
        - name: "app"
          bind_address: "*"
          bind_port: "80"
          servers:
            - name: "app01"
              address: "192.168.0.101"
              port: "80"
            - name: "app02"
              address: "192.168.0.102"
              port: "80"
```

#### Advanced

A load balancer with 2 frontends and enabled stats page.

```
- hosts: all
  roles:
    - role: while_true_do.srv_haproxy
      wtd_srv_haproxy_conf_stats:
        enabled: true
        auth: "admin:myPassword%0815"
      wtd_srv_haproxy_conf_frontends:
        - name: "fe_blog"
          bind_address: "192.168.0.11"
          bind_port: "80"
          default_backend: "be_blog"
        - name: "fe_shop"
          bind_address: "192.168.0.12"
          bind_port: "80"
          default_backend: "be_shop"
      wtd_srv_haproxy_conf_backends:
        - name: "be_blog"
          balance: "roundrobin"
          servers:
            - name: "blog01"
              address: "192.168.10.101"
              port: "80"
            - name: "blog02"
              address: "192.168.10.102"
              port: "80"
        - name: "be_shop"
          balance: "roundrobin"
          servers:
            - name: "shop01"
              address: "192.168.20.201"
              port: "8080"
            - name: "shop02"
              address: "192.168.20.202"
              port: "8080"
            - name: "shop03"
              address: "192.168.20.203"
              port: "8080"
              options: "check weight 1 backup"
```

## Known Issues

1.  RedHat Testing is currently not possible in public, due to limitations
    in subscriptions.
2.  Some services and features cannot be tested properly, due to limitations
    in docker.
3.  SSL is currently not supported.
4.  ACLs are currently not supported.

## Testing

Most of the "generic" tests are located in the
[Test Library](https://github.com/while-true-do/test-library).

Ansible specific testing is done with
[Molecule](https://molecule.readthedocs.io/en/stable/).

Infrastructure testing is done with
[testinfra](https://testinfra.readthedocs.io/en/stable/).

Automated testing is done with [Travis CI](https://travis-ci.com/while-true-do).

## Contribute

Thank you so much for considering to contribute. We are very happy, when somebody
is joining the hard work. Please fell free to open
[Bugs, Feature Requests](https://github.com/while-true-do/ansible-role-srv_haproxy/issues)
or [Pull Requests](https://github.com/while-true-do/ansible-role-srv_haproxy/pulls) after
reading the [Contribution Guideline](https://github.com/while-true-do/doc-library/blob/master/docs/CONTRIBUTING.md).

See who has contributed already in the [kudos.txt](./kudos.txt).

## License

This work is licensed under a [BSD-3-Clause License](https://opensource.org/licenses/BSD-3-Clause).

## Contact

-   Site <https://while-true-do.io>
-   Twitter <https://twitter.com/wtd_news>
-   Code <https://github.com/while-true-do>
-   Mail [hello@while-true-do.io](mailto:hello@while-true-do.io)
-   IRC [freenode, #while-true-do](https://webchat.freenode.net/?channels=while-true-do)
-   Telegram <https://t.me/while_true_do>
