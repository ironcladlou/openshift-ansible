# OpenShift Cluster Monitoring Operator

The OpenShift Cluster Monitoring Operator role manages the [Tectonic Prometheus Operator](https://github.com/coreos-inc/tectonic-prometheus-operator) deployment.
TPO is an operator that deploys our monitoring stack (Prometheus, AlertManager) with out-of-the-box alerts and metrics.

# Component integration

The following sections are to guide component owners to shipping new integrations with the monitoring stack.

## Developing

To develop new component integrations, try the following.

1. [Create a GCE cluster](https://github.com/openshift/release/tree/master/cluster/test-deploy) with the monitoring stack enabled by editing your profile prior to launch (e.g. `gcp-dev/vars.yaml`):

    ```yaml
    openshift_openshift_monitoring_deploy: true
    ```

1. Clone the [Tectonic Prometheus Operator repository](https://github.com/coreos-inc/tectonic-prometheus-operator).

1. To register a new component for metrics scraping:

    1. Follow the Tectonic Prometheus Operator [instructions](https://github.com/coreos-inc/tectonic-prometheus-operator) to register a new builtin component (*Note: the Go code portions can be skipped while prototyping but must be completed before a PR is submitted*).
    1. Create the new `ServiceMonitor` manually with:
    
        ```shell
        oc apply -n openshift-monitoring -f assets/prometheus-k8s/prometheus-k8s-service-monitor-$COMPONENT.yaml`
        ```

1. To add a new alerting rule:
  
    1. Follow the Tectonic Prometheus Operator [instructions](https://github.com/coreos-inc/tectonic-prometheus-operator) to add a new alerting rule.
    1. Rebuild the rules `ConfigMap` manually with:
    
        ```shell
        hack/generate-rules-configmap.sh k8s | oc apply -n openshift-monitoring -f -
        ```

## Shipping

To ship a new component integration, the following things must happen:

1. The component must be accepted into [Tectonic Prometheus Operator](https://github.com/coreos-inc/tectonic-prometheus-operator) and available in a new Tectonic Prometheus Operator image.

1. The `openshift_cluster_monitoring_operator` role must be updated to use the new Tectonic Prometheus Operator image containing the new component integration.

# Installation

See the [openshift-monitoring playbook](../../playbooks/openshift-monitoring) for installation options.

## Role Variables

For default values, see [`defaults/main.yaml`](defaults/main.yaml).

- `openshift_cluster_monitoring_operator_install`: true - install/update. false - uninstall.
- `openshift_cluster_monitoring_operator_image`: TPO image to use
- `openshift_cluster_monitoring_operator_prometheus_operator_image`: Prometheus Operator image to use 
- `openshift_cluster_monitoring_operator_prometheus_image`: Prometheus image to use
- `openshift_cluster_monitoring_operator_alertmanager_image`: AlertManager image to use
- `openshift_cluster_monitoring_operator_prometheus_reloader_image`: Prometheus Reloader image to use
- `openshift_cluster_monitoring_oeprator_configmap_reloader_image`: ConfigMap reloader image to use

# Requirements

Ansible 2.4

# Dependencies

- lib_openshift
- lib_utils
- openshift_facts

# License

Apache License, Version 2.0
