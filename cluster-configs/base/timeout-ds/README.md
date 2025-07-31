# Timeout DS

This component deploys a privileged DaemonSet (`timeout-ds`) that runs a simple smoke test script on each node every hour.

## Purpose

The primary purpose of this DaemonSet is to execute a test script in a privileged context to verify access to host-level resources. This can be useful for continuous monitoring and validation of node configurations.

## Configuration

- **DaemonSet:** The `daemonset.yaml` defines the DaemonSet that runs on each node.
- **Script:** The `custom-script.yaml` contains a ConfigMap with the shell script that is executed by the DaemonSet.
- **RBAC:** The `rbac.yaml` file sets up the necessary permissions for the DaemonSet to run with a privileged security context.
- **Kustomization:** The `kustomization.yaml` file ties all of these resources together.
