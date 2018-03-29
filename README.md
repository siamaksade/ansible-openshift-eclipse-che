Ansible Role: Eclipse Che on OpenShift
[![Build Status](https://travis-ci.org/siamaksade/ansible-openshift-che.svg?branch=master)](https://travis-ci.org/siamaksade/ansible-openshift-che)
=========

Ansible Role for deploying (Eclipse Che)[https://www.eclipse.org/che/] web-based IDE in 
single-user mode on OpenShift. 


Role Variables
------------

| Variable                  | Default Value    | Description   |
|---------------------------|------------------|---------------|
|`che_version`              | latest           | Eclipse Che image version as available on [Docker Hub](https://hub.docker.com/r/eclipse/che/tags/) |
|`route_suffix`             | 127.0.0.1.nip.io | **Required**. Apps route suffix in the OpenShift cluster |
|`project_name`             | che              | OpenShift project name for the Gogs container  |
|`project_display_name`     | Eclipse Che IDE  | OpenShift project display name for the Gogs container  |
|`project_desc`             | Eclipse Che IDE  | OpenShift project description for the Gogs container |
|`project_annotations`      | -                | OpenShift project annotations for the Gogs container |
|`project_admin`            | -                | If set, the user to be assigned as project admin |
|`openshift_cli`            | oc               | OpenShift CLI command and arguments (e.g. auth)       | 


Example Playbook
------------

```
name: Example Playbook
hosts: localhost
tasks:
- import_role:
    name: siamaksade.openshift_che
  vars:
    project_name: "myproject"
    che_version: "6.3.0"
```