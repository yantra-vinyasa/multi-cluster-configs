# App Factory

It uses the "app of apps" pattern to deploy and manage all the applications in the cluster.

## How it works

The `kustomization.yaml` file uses a Helm chart (`argocd-app-of-app-chart`) to generate the ArgoCD `Application` resources defined in the `values.yaml` file. Each entry in the `applications` section of the `values.yaml` file corresponds to an application that will be deployed and managed by ArgoCD.

The `projects` section defines the ArgoCD projects that the applications belong to.

