# Red Hat Advanced Cluster Management for Kubernetes (RHACM)

RHACM gives you visibility into your OpenShift and Kubernetes clusters from a single user interface.
RHACM provides built-in:
- governance
- cluster lifecycle management
- application lifecycle management
- observability features

## RHACM components on the Hub cluster

The **MultiClusterHub** object is part of the RHACM operator installation and is responsible for managing, upgrading, and installing the components of RHACM.

The RHACM operator creates the following namespaces in the Hub cluster:
- **open-cluster-management**
  - contains the deployments for the main features of RHACM
- **open-cluster-management-hub**
  - contains the deployments that receive data from the managed clusters
- **open-cluster-management-observability**
  - Only if you enable the RHACM observability engine
  - contains all the Thanos deployments that store, create and visualize the observability data

## RHACM components on the Managed cluster

The RHACM operator creates the following namespaces in the Managed clusters:
- **open-cluster-management-agent** 
  - contains the deployments for the klusterlet agent. The Hub cluster controlls managed clusters by communicating with the klusterlet agent.
- **open-cluster-management-agent-addon**
  - contains the application's deployments, search engine, and policies engines
- **open-cluster-management-agent-addon-observability**
  - Only if you enable the RHACM observability engine
  - contains the observability engine's deployments

## Links

- [A collection of policy examples for Open Cluster Management](https://github.com/open-cluster-management-io/policy-collection)
- [Policy Generator](https://github.com/open-cluster-management-io/policy-generator-plugin?tab=readme-ov-file)
- [PolicyGenerator reference YAML](https://github.com/open-cluster-management-io/policy-generator-plugin/blob/main/docs/policygenerator-reference.yaml)
- [RHACM Template Processing](https://docs.redhat.com/en/documentation/red_hat_advanced_cluster_management_for_kubernetes/2.13/html/governance/governance#template-processing)