# Manual GitOps Sync Policy

In a GitOps workflow managed by ArgoCD, applications can be configured with different synchronization policies. While auto-sync is convenient for keeping your cluster state aligned with your Git repository, manual sync provides an extra layer of control, which is particularly useful in certain scenarios.

## What is Manual Sync?

Manual sync requires a user to explicitly trigger the synchronization of an application from the ArgoCD UI or CLI. ArgoCD will detect that the live state of the application in the cluster is out of sync with the desired state in the Git repository, but it will not take any action until a manual sync is initiated. The application status will be shown as `OutOfSync`.

## Why Use Manual Sync?

Manual synchronization is often preferred for:

-   **Preventing Accidental Changes:** It prevents accidental or untested commits from being automatically deployed to a sensitive environment. Changes are only applied after explicit approval.

## Configuration

To configure an ArgoCD application for manual sync, you need to omit the `automated` policy in the `syncPolicy` of the Application manifest. If `syncPolicy` is not defined, it defaults to manual sync.

Here is an example of an `Application` resource configured for manual sync:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-critical-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/your-org/your-repo.git'
    path: path/to/your/app
    targetRevision: HEAD
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: my-app-namespace
  # No syncPolicy.automated field means it's manual
  syncPolicy: {}
```

To be explicit, you can also write it as:

```yaml
...
  syncPolicy:
    automated: null
```

In contrast, an auto-sync configuration would look like this:

```yaml
...
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## The Manual Sync Process

1.  **Commit and Push:** Make your changes to the Kubernetes manifests and push them to the Git repository.
2.  **ArgoCD Detects Drift:** ArgoCD will compare the state of the cluster with the updated manifests in Git and report the application as `OutOfSync`.
3.  **Review the Diff:** In the ArgoCD UI, you can inspect the differences between the live state and the desired state. This shows you exactly what will be created, updated, or deleted.
4.  **Initiate Sync:** If the changes are acceptable, click the "Sync" button in the ArgoCD UI or use the `argocd` CLI to trigger the synchronization.



This manual intervention ensures that changes are deployed deliberately and safely.
