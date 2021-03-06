Ansible Role: rundeckres
========================

This role helps you to generate
[Rundeck](http://rundeck.org)
[node resource file in JSON/YAML format](http://rundeck.org/docs/man5/resource-yaml.html)
from Ansible inventory and facts.

It can generate tags for each node from groups defined in Ansible inventory,
as well as other useful attributes including
osArch, osName, ansibleDistribution, etc. from facts gathered by Ansible.

Ansible Galaxy Page:
[https://galaxy.ansible.com/list#/roles/3327](https://galaxy.ansible.com/list#/roles/3327)

Requirements
------------

None.

Role Variables
--------------

| Name                | Default                           | Description                         |
|---------------------|-----------------------------------|-------------------------------------|
| facts_dir           | {{inventory_dir}}/facts           | Temporary dir to store facts files  | 
| rundeckres_path     | {{inventory_dir}}/rundeckres.json | Output path of Rundeck resource     |
| rundeckres_username | {{ansible_user_id}}               | username in Rundeck resource        |

Dependencies
------------

None.

Example Playbook
----------------

rundeckres.yml (playbook):

```yaml
---
- hosts: all
  roles:
    - role: yaegashi.rundeckres
      rundeckres_username: rundecker
```

hosts (inventory):

```
[web]
host-a ansible_ssh_host=kvz02

[db]
host-b ansible_ssh_host=intense

[servers:children]
web
db
```

By running ansible-playbook with the files above,
you can generate rundeckres.json which is suitable for
Rundeck's node resource in JSON/YAML.

```
$ ansible-playbook -i hosts rundeckres.yml
$ cat rundeckres.json
[
  {
    "username": "rundecker", 
    "osFamily": "unix", 
    "nodename": "host-b", 
    "tags": "db,servers", 
    "hostname": "intense", 
    "osName": "Linux", 
    "osArch": "amd64", 
    "ansibleOsFamily": "Debian", 
    "ansibleDistributionRelease": "trusty", 
    "ansibleDistributionMajorVersion": "14", 
    "ansibleDistribution": "Ubuntu", 
    "osVersion": "3.13.0-39-generic", 
    "ansibleDistributionVersion": "14.04", 
    "description": "Ubuntu 14.04.2 LTS"
  }, 
  {
    "username": "rundecker", 
    "osFamily": "unix", 
    "nodename": "host-a", 
    "tags": "servers,web", 
    "hostname": "kvz02", 
    "osName": "Linux", 
    "osArch": "amd64", 
    "ansibleOsFamily": "Debian", 
    "ansibleDistributionRelease": "wheezy", 
    "ansibleDistributionMajorVersion": "7", 
    "ansibleDistribution": "Debian", 
    "osVersion": "2.6.32-openvz-042stab094.6-amd64", 
    "ansibleDistributionVersion": "7.8", 
    "description": "Debian 7.8"
  }
]
```

License
-------

GPLv3+

Author Information
------------------

[YAEGASHI Takeshi](https://github.com/yaegashi)
