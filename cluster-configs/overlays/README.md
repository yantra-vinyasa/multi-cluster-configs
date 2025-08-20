# Overlays

This directory contains cluster-specific Kustomize overlays. Each subdirectory corresponds to a specific cluster (e.g., `sandbox`) and contains patches and configurations that override or supplement the `base` configurations.

## Structure

- `overlays/<cluster_name>/`: Contains the configurations for a specific cluster. For example, the `sandbox` directory contains the configurations for the sandbox cluster.