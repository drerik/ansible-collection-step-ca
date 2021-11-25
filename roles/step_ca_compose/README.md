step-ca_compose
=========

Deploys a step-ca ( <https://smallstep.com/docs/step-ca> ) Certificate Authority docker-compose deployment on the server. Persisten data is located inside a volume called `{{ step_ca_project_name }}_step`.

Requirements
------------

Requires the role `drerik.container.docker` to be installed

Role Variables
--------------

- `step_ca_project_name`: ( Default: step_ca ) Docker-compose project name
- `step_ca_home_dir`: ( Default: /srv/{{ step_ca_project_name }} ) Where the docker-compose project lives
- `step_ca_containername`: ( Default: step-ca )
- `step_ca_init_name`: ( Default: my_ca_name )
- `step_ca_init_dns_names`: ( Default: localhost,{{ inventory_hostname }} )
- `step_ca_config`:
  ```yaml
  authority:
    claims:
      maxTLSCertDuration: 2160h
  ```

- `step_ca_compose_conf`:
  ```yaml
  version: '3'
    services:
      step-ca:
        container_name: "{{ step_ca_containername }}"
        image: smallstep/step-ca
        restart: unless-stopped
        environment:
          DOCKER_STEPCA_INIT_NAME: "{{ step_ca_init_name }}"
          DOCKER_STEPCA_INIT_DNS_NAMES: "{{ step_ca_init_dns_names }}"
        ports:
          - 9000:9000
        volumes:
          - step:/home/step
    volumes:
      step: {}
  ```


Dependencies
------------

- drerik.container.docker

Example Playbook
----------------

```yaml
- name: Deploy the step_ca_compose role to step_ca group with default parameters
  hosts: step-ca
  roles:
    - drerik.container.docker
    - drerik.step_ca.step-ca_compose
```

License
-------

Apache-2.0

Author Information
------------------

- Erik Kaareng-Sunde <erik@eriksunde.com>
