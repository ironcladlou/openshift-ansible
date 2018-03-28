# OpenShift Cluster Monitoring Operator

The OpenShift Cluster Monitoring Operator role manages the TPO deployment.
TPO is an operator that deploys our monitoring stack (Prometheus, AlertManager)
with out-of-the-box alerts and metrics.

# Requirements

Ansible 2.4

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
