# OpenShift Cluster Monitoring Operator

OpenShift Cluster Monitoring Operator manages the TPO deployment.

# Requirements

Ansible 2.4

# Role Variables

| Name                                      | Default                |                  |
|-------------------------------------------|------------------------|------------------|
|                                           |                        |                  |

# Dependencies

- lib_openshift
- lib_utils
- openshift_facts

# Example Playbook

```
---
- name: Deploy Cluster Monitoring Operator
  hosts: oo_first_master
  roles:
  - role: openshift_cluster_monitoring_operator
```

# License

Apache License, Version 2.0
