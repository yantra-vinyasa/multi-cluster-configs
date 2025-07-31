# Base Configuration

This directory contains the base configurations for various components deployed across multiple clusters. Each component is organized into its own subdirectory, containing the necessary Kubernetes manifests, Helm charts, or Kustomize overlays.

## Components

### Applications
- [etherpad](./etherpad)
- [pacman](./pacman)

### Authentication & Authorization
- [authorino-operator](./authorino-operator)
- [keycloak](./keycloak)
- [openldap](./openldap)

### Cluster Management
- [rhacm](./rhacm)

### Core Components
- [apiserver](./apiserver)
- [image-registry](./image-registry)
- [ingress-controller](./ingress-controller)

### GitOps App of Apps
- [argocd-app-of-app-chart](./argocd-app-of-app-chart)

### Hardware & Node Management
- [gpu-operator-certified](./gpu-operator-certified)
- [infra-nodes](./infra-nodes)
- [nfd](./nfd)
- [timeout-ds](./timeout-ds)

### Logging & Monitoring
- [alertmanager-config](./alertmanager-config)
- [grafana](./grafana)
- [openshift-logging](./openshift-logging)

### Platforms
- [openshift-ai](./openshift-ai)
- [openshift-serverless](./openshift-serverless)
- [openshift-servicemesh](./openshift-servicemesh)

### Security & Compliance
- [gatekeeper-operator](./gatekeeper-operator)
- [openshift-compliance](./openshift-compliance)

### Storage
- [etcd-backup](./etcd-backup)
- [local-storage-operator](./local-storage-operator)
- [odf-operator](./odf-operator)
- [trident](./trident)

### Tooling
- [installplan-approver](./installplan-approver)
- [openshift-debug](./openshift-debug)
- [web-terminal-operator](./web-terminal-operator)