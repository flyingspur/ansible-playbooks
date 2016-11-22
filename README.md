# Overview
This folder contains all the configuration to bring up a Consul+Vault cluster, load balanced by HAProxy using Vagrant (VirtualBox provider) and Ansible provisioner. 

## Usage
1. Run ```vagrant up```, to start provisioning all 3 Consult+Vault node clusters and a Web server node running HAProxy to load the 3 Vault nodes 
  * Sometimes, vagrant fails at the setting hostname step. Just re-run ```vagrant up``` to move forward. 
2. Once complete, the Consul Web UI can be accessed via ```http://localhost:8850/ui/``` and, Vault's HTTP end point can be accessed via ```http://localhost:8193/v1/sys/init```
3. HAProxy's admin UI can be accessed via ```http://localhost:11933/stats```. The credentials are admin/admin.

## Note
1. This configuration has been tested and validated in CentOS 7
2. To speed up, the provisioning process, the installers for Consul, Vault and Consul WebUI have been uploaded along with this project.
3. vault must be initialized first for HAProxy's health checks to work and route traffic
 

## Project Layout
```bash
.
├── README.md
├── Vagrantfile
├── playbook.yml
└── roles
    ├── base_install
    │   ├── files
    │   │   ├── consul.service
    │   │   ├── consul_0.7.1_linux_amd64.zip
    │   │   ├── consul_0.7.1_web_ui.zip
    │   │   ├── vault.service
    │   │   └── vault_0.6.2_linux_amd64.zip
    │   ├── handlers
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   └── templates
    │       ├── bootstrap_config.j2
    │       ├── server_config.j2
    │       └── vault_config.j2
    ├── common
    │   └── tasks
    │       └── main.yml
    └── haproxy
        ├── files
        │   └── haproxy.cfg
        ├── handlers
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        └── templates
```
