# Cluster Monitoring

This component manages the `cluster-monitoring-config` _ConfigMap_ in the `openshift-monitoring` _Project_.

This _ConfigMap_ is used by the `cluster-monitoring-operator` in the `openshift-monitoring` _Project_ to manage, deploy and configure the [OpenShift Monitoring Stack](https://docs.openshift.com/container-platform/latest/monitoring/understanding-the-monitoring-stack.html) in the same project.

It deploys the Monitoring stack components on _Nodes_ with the `infra` role set.

If User Workload Monitoring is enabled, then the Operator will also manage, deploy and configure a monitoring stack in the `openshift-user-workload-monitoring` _Project_.

The `openshift-user-workload-monitoring` _ConfigMap_ then relates to the Prometheus instance that monitors user-defined projects only. [Creating a user-defined workload monitoring config map](https://docs.openshift.com/container-platform/latest/monitoring/configuring-the-monitoring-stack.html#creating-user-defined-workload-monitoring-configmap_configuring-the-monitoring-stack)

 
## Configuration
Changes to the configmap is done in the overlays.

## Troubleshooting

The `cluster-monitoring-operator` _Deployment_ in the `openshift-monitoring` _Project_ will hold logs and events on deploying and configuring the Monitoring stack.

Each of the following components of the Monitoring stack has their own logs and events to troubleshoot:
- AlertManager
- Prometheus Operator
- Prometheus
- Thanos Querier
- Telemeter
- Cluster-monitoring-operator
- Kube-state-metrics
- Node-exporter
- Openshift-state-metrics
- Prometheus-adapter
- Prometheus-operator-admission-webhook

The user-workload-monitoring is deployed in the `openshift-user-workload-monitoring` _Project_ with the following components deployed as _StatefulSets_:
- Prometheus
- Thanos Ruler

Make sure that the cluster has enough storage to fulfill any _PersistentVolumeClaims_.

[OpenShift Documentation on Troubleshooting Monitoring](https://docs.openshift.com/container-platform/latest/monitoring/troubleshooting-monitoring-issues.html)
