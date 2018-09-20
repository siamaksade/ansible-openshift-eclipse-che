Ansible Role: Eclipse Che on OpenShift
[![Build Status](https://travis-ci.org/siamaksade/ansible-openshift-eclipse-che.svg?branch=master)](https://travis-ci.org/siamaksade/ansible-openshift-eclipse-che)
=========

Ansible Role for deploying [Eclipse Che](https://www.eclipse.org/che/) web-based IDE in 
single-user mode on OpenShift. 

Role Variables
------------

| Variable                    | Default Value     | Required |  Description   |
|-----------------------------|--------------------|----------|----------------|
|`route_suffix`               | `127.0.0.1.nip.io` | Required | Apps route suffix in the OpenShift cluster |
|`che_version`                | `6.7.1`            | Optional | Eclipse Che image version as available on [Docker Hub](https://hub.docker.com/r/eclipse/che/tags/) |
|`project_name`               | ` eclipse-che`     | Optional | OpenShift project name for the Gogs container  |
|`project_display_name`       | `Eclipse Che IDE`  | Optional | OpenShift project display name for the Gogs container  |
|`project_desc`               | `Eclipse Che IDE`  | Optional | OpenShift project description for the Gogs container |
|`project_annotations`        | -                  | Optional | OpenShift project annotations for the Gogs container |
|`project_admin`              | -                  | Optional | If set, the user to be assigned as project admin |
|`multi_user`                 | `false`            | Optional | Multi-user or single-user mode |
|`multi_user_che_tls`         | `false`            | Optional | For multi-user mode, enable TLS  |
|`multi_user_che_protocol`    | `http`             | Optional | For multi-user mode, `http` or `https` protocol |
|`multi_user_che_ws_protocol` | `ws`               | Optional | For multi-user mode, `ws` or `wss` protocol |
|`keycloak_admin_user`        | `admin`            | Optional | For multi-user mode, Keycloak admin user to be created |
|`keycloak_admin_pwd`         | `admin`            | Optional | For multi-user mode, Keycloak admin password to be created |
|`che_generate_user_count`    | `0`                | Optional | For multi-user mode, number of users to be pre-created in che realm |
|`che_generate_user_format`   | `user%d`           | Optional | For multi-user mode, format for the usernames for the users to be pre-created in che realm |
|`che_generate_user_password` | `password`         | Optional | For multi-user mode, password for the users to be pre-created in che realm |
|`install_java_oc_stack`      | `false`            | Optional | Install a Java stack with Maven, OpenShift CLI and Ansible |
|`install_custom_stacks_json` | -                  | Optional | Install custom stacks. The value is a list of stack jsons |
|`install_custom_factory_json` | -                  | Optional | Install custom factories for every user. The value is a single factory in json format |
|`install_custom_factory_filename` | -              | Optional | Install custom factories for every user. It loads the value processing it as a template |
|`openshift_cli`              | `oc`               | Optional | OpenShift CLI command and arguments (e.g. auth) |


Example Playbook
------------

```
name: Example Playbook
hosts: localhost
tasks:
- import_role:
    name: siamaksade.openshift_che
  vars:
    project_name: "ide"
    che_version: "latest"
    install_custom_stacks_json: [ "{{ lookup('file','files/custom-stack.json') }}" ]
    install_custom_factory_json: "{{ lookup('template','che-factory.json.j2') }}"
```

__NOTE:__ In the previous example, the factory json template will be processed in the calling playbook, when loaded.

Test locally
------------
If you want to test this role locally:

```
ansible-playbook tests/test.yml \
        -e route_suffix=apps.example.com \
        -e multi_user=true \
        -e che_generate_user_count=2 \
        -e install_custom_factory_filename="che-factory.json.j2.example"
```

__NOTE:__ Add as many parameter variations from the defaults as you want
