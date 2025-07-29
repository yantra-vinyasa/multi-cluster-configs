# Develop and Test Changes in multi-cluster-configs

This document describes how to develop and test changes in the `multi-cluster-configs` repository using GitOps.

## Development and Testing Workflow

The repository supports a complete development lifecycle:

1.  **Branch**: Developers create feature branches for infrastructure changes.
2.  **Local Testing**: Kustomize allows local validation before deployment via GitOps.

## Prerequisites

- Access to the `multi-cluster-configs` repository.
- `oc` CLI tool configured for cluster access.
- `kustomize` CLI tool.
- `git` for version control.

## Development Process

### 1. Feature Branch

Create a fork of the repository and work on feature branches:

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/multi-cluster-configs.git
cd multi-cluster-configs

# Create a feature branch
git checkout -b feature/your-change-description
```

### 2. Creating a New Component

To add a new component to be managed by GitOps:

1.  **Create a directory for the new component** under `cluster-configs/base/`.
2.  **Add your Kubernetes manifests** to this new directory.
3.  **Create a `kustomization.yaml`** file in the component's directory, listing all the Kubernetes manifests.
4.  **Add the new component to an overlay** by adding a reference to it in the `kustomization.yaml` of the overlay (e.g., `cluster-configs/overlays/dev/kustomization.yaml`).

```bash
# Commit and push your tested changes
git add .
git commit -m "feat: describe your changes"
git push origin feature/your-change-description
```

## Best Practices

### Configuration Management

- Use Kustomize overlays for environment-specific configurations.
- Keep base configurations generic and environment-neutral.
- Use patches for environment-specific modifications.

### Testing Strategy

- Always test in the development cluster before promoting to production.
- Validate configurations locally with `kustomize build`.


### GitOps Workflow

- Make small, incremental changes.
- Use descriptive commit messages.
- Create feature branches for new functionality.
- Review changes in pull requests before merging.

## Monitoring Changes

- Monitor the ArgoCD dashboard for application sync status.
- Check cluster resources after deployments.
- Verify application functionality in both environments.
- Review logs for any deployment issues.

This workflow ensures that safe and tested changes are deployed through GitOps, maintaining cluster stability and compliance.
