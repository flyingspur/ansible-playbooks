# Overview
This folder contains all the configuration to bring up a Consul+Vault cluster, load balanced by HAProxy using Vagrant (VirtualBox provider) and Ansible provisioner. 

## Usage
1. Run ```vagrant up```, to start provisioning the cluster of 3 Consult+Vault nodes and a Web server node running HAProxy to load balance the 3 Vault nodes 
2. Upon completion, access the Consul Web UI can be accessed via ```http://192.168.33.39:9080/```, using creds admin/admin12E* and, Vault's HTTP end point can be accessed via ```http://localhost:8193/v1/sys/init```
3. HAProxy's admin UI can be accessed via ```http://192.168.33.39:1936/stats```. The credentials are admin/Admin123*.

## Note
1. This configuration has been tested and validated using CentOS/7 base box
3. vault must be initialized first for its API and HAProxy's health checks to work.
 

## Project Layout
```bash
.
└── consul_vault_haproxy
    ├── consul_vault_haproxy.yml
    ├── roles
    │   ├── consul_vault_common
    │   │   └── tasks
    │   │       └── main.yml
    │   ├── consul_vault_haproxy
    │   │   ├── files
    │   │   │   ├── haproxy
    │   │   │   └── haproxy.conf
    │   │   ├── handlers
    │   │   │   └── main.yml
    │   │   ├── tasks
    │   │   │   └── main.yml
    │   │   └── templates
    │   │       └── haproxy.j2
    │   └── consul_vault_install
    │       ├── files
    │       │   ├── consul.service
    │       │   └── vault.service
    │       ├── handlers
    │       │   └── main.yml
    │       ├── tasks
    │       │   └── main.yml
    │       └── templates
    │           ├── bootstrap_config.j2
    │           ├── server_config.j2
    │           └── vault_config.j2
    ├── Vagrantfile
    └── vars
        └── vars.yml

15 directories, 16 files

```