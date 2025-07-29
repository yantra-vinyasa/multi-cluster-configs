# multi-cluster-configs

## Top level directories:

* bootstrap: Contains configuration for bootstrapping the GitOps System.

* cluster-configs/base: Components that need to be installed or configured in the cluster.

* cluster-configs/overlay: Selects components to be installed into the cluster.

* tools : Helper scripts (e.g. setting up CI for gitlab & linting ).


### GitOps:
* [What is GitOps](https://opengitops.dev/)
* [App of Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/)
* [Red Hat CoP GitOps Catalog](https://github.com/redhat-cop/gitops-catalog): A collection of curated GitOps patterns and examples.
  
### Bootstrap process:

Configuring a new cluster (e.g. here dev cluster ) requires the execution of the following commands :

**Note 🚨:** Before proceeding, ensure you have created the necessary Kubernetes secrets with the correct permissions for both the ArgoCD (GitOps) Operator to access the Git repository and for the External Secrets Operator to authenticate with your chosen secret store (e.g., Gitlab,Vault).

1. **Install the OpenShift GitOps Operator**

The OpenShift GitOps Operator is the foundation of our GitOps setup. It installs and manages ArgoCD on the cluster.
```
kustomize build bootstrap/overlays/dev/gitops-operator | oc apply -f - 
```

2. **Install the External Secrets Operator**

The External Secrets Operator is used to fetch secrets from a secret store (like Vault or Gitlab) and create Kubernetes secrets from them. This allows us to keep sensitive data out of Git repository.

```bash
kustomize build bootstrap/overlays/dev/external-secrets-operator | oc apply -f -
``` 

3. **App of Apps Pattern is executed**

Once these bootstrap components are installed, the `appofapp.yaml` in `bootstrap/overlays/dev/gitops-operator` will deploy the cluster configurations defined in `cluster-configs`.

## Contributing

Please see the [development and testing guide](docs/develop_and_test_changes.md) for details on how to contribute to this project.