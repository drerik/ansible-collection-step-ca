drerik.step-ca.certificates
=========

A ansible role that creates SSL certificates signed with a step-ca Certificate Authority deployed with the role `drerik.step_ca.step_ca_compose` using the `step` commandline utility.

Requirements
------------

It uses the `community.docker.docker_container_exec` module to get a onetime token from the step-ca container

Role Variables
--------------

- `step_ca_host`: step-ca host to sign certificates with.
- `step_ca_containername`: ( Default: step-ca ) Name of the container the step-ca service is deployed in. This variable is also used for other roles in the `drerik.step-ca` collection.
- `ssl_certificate_path`: ( Default: `/etc/ssl/{{ inventory_hostname}}` ) Where to store the certificates.
- `ssl_certificate_common_name`: ( Default: `{{ inventory_hostname }}` ) The common name in the certificate.
- `ssl_certificate_key_size`: ( Default: `4096` ) Certificate keysize.
- `ssl_certificate_subject_alt_names`: List over dns and ip adresses that should be in "subject alt names". Example:
  ```yaml
  - "{{ inventory_hostname }}"
  - 127.0.0.1
  - "{{ ansible_default_ipv4.address }}"
  ```
- `ssl_certificate_not_after`: ( Default: `2160h` or 90 days ) How long a certificate is valid.

- `step_renew_cert_expires_in`: ( Default: `72h`)
- `step_renew_cert_cmd:`: ( For default, see `defaults/main.yml` ) Command to run for certificate renewal
- `step_renew_cert_log:`: ( Default: `/var/log/step_renew_cert_{{ ssl_certificate_common_name }}.log` )
- `step_renew_cert_post_script`: Script to run after a successfull renewal



Dependencies
------------

- drerik.step_ca.step_cli

Example Playbook
----------------

```yaml
- name: Create and sign sertificates on the server
  hosts: server
  vars:
    step_ca_containername: step-ca-{{ inventory_hostname }}
    step_ca_host: "{{ inventory_hostname }}"
    ssl_certificate_path: /etc/ssl/{{ inventory_hostname }}
    ssl_certificate_common_name: "{{ inventory_hostname }}"
    ssl_certificate_key_size: 4096
    ssl_certificate_subject_alt_names:
          - "{{ inventory_hostname }}"
          - 127.0.0.1
          - "{{ ansible_default_ipv4.address }}"
    ssl_certificate_not_after: 2160h
  roles:
    - drerik.step_ca.certificate

- name: Create and sign sertificates on the server win minimal configuration
  hosts: server
  vars:
    step_ca_host: "{{ inventory_hostname }}"
  roles:
    - drerik.step_ca.certificate
```

License
-------

Apache-2.0

Author Information
------------------

- Erik Kaareng-Sunde <erik@eriksunde.com>
