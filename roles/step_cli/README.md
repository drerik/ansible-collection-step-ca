drerik.step-ca.step-cli
=========

Installs and configures step-cli ( <https://smallstep.com/docs/step-cli> )package to work against a container based step-ca server deployed with the `drerik.step-ca.step-ca_compose` role. Works on debian based distros

Requirements
------------

Uses the `community.docker.docker_container_exec` module to get cert

Role Variables
--------------

- `step_ca_containername`: Name of the container the step-ca service is deployed in. This variable is also used for other roles in the `drerik.step_ca` collection.
- `step_ca_host`: step-ca host to fetch fingerprint from under configuration. 
- `step_ca_url`: ( Default: `https://{{ step_ca_host }}:9000` ) Defines the url the tool uses to access the CA. 

Dependencies
------------

The `step_ca_containername` variable is the same variable that is defined in the `drerik.step_ca.step-ca_compose` role.

Example Playbook
----------------

```yaml
  hosts: step_ca
  vars:
    step_ca_host: "{{ inventory_hostname }}"
  roles:
    - drerik.step_ca.step_cli
```

License
-------

Apache-2.0

Author Information
------------------

- Erik Kaareng-Sunde <erik@eriksunde.com>
