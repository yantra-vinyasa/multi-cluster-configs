# ArgoCD Configuration

This document outlines the key components and custom configurations for this ArgoCD instance.

## Core Components

- **Repo Server**: Fetches resources from Git repositories and generates Kubernetes manifests.
- **Application Controller**: Manages the reconciliation and synchronization of manifests to the cluster.
- **API Server**: Powers the Web UI and external API access.
- **Redis**: Provides caching for performance.
- **ApplicationSet Controller**: Manages `ApplicationSet` resources for automating application creation.

## Custom Configurations

### Resource Tracking

- **Method**: Changed from the default `label` to `annotation`.
- **Benefit**: Avoids conflicts with other operators that use the common `app.kubernetes.io/instance` label. ArgoCD will now use the `argocd.argroj.io/tracking-id` annotation to track resources.

### Controller Performance Optimizations

- **Client QPS/Burst**: Increased to `100/200` from the default `50/100` to improve reconciliation and sync performance.
- **Repo Server Timeout**: Extended to 2 minutes (`120s`) to prevent `Context deadline exceeded` errors during manifest generation for complex applications.

### RBAC Configuration

- **Default Policy**: Deny all access by default.


### Additional Settings

- **Kustomize Options**: Enabled `--enable-alpha-plugins` (for policy generation) and `--enable-helm` (for Helm chart integration).
- **OpenShift SSO**: Integrated with OpenShift login via Dex for seamless user authentication.
- **Resource Exclusions**: Tekton `TaskRun` and `PipelineRun` resources are ignored, as they are managed by other controllers.
- **Custom Health Checks**: Utilizes Lua scripts to define custom resource health assessments, which is particularly important for managing dependencies in an app-of-apps pattern.
