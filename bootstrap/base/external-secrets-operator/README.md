# Using GitLab Variables with External Secrets Operator (ESO)

This guide provides a streamlined workflow for syncing GitLab CI/CD variables into your Kubernetes cluster as native `Secret` objects using the External Secrets Operator (ESO).

**The process is simple:** A `SecretStore` configures the connection to your GitLab project, and an `ExternalSecret` specifies which variables to fetch and sync.


---

### Step 1: Configure GitLab

In your GitLab project, create an access token for authentication and a variable to act as your secret.

**1. Create a Project Access Token**
- Navigate to **Settings > Access Tokens**.
- **Name:** `eso-gitlab-token`
- **Role:** `Maintainer` (Sufficient for reading variables)
- **Scopes:** `api`
- **🚨 Important:** Save the generated token. You will not see it again.

**2. Create a CI/CD Variable**
- Navigate to **Settings > CI/CD > Variables**.
- **Key:** `my_key`
- **Value:** `dummy-Pa$$w0rd`


### Step 2: Configure Kubernetes

Now, create the Kubernetes resources to connect to GitLab.

**1. Store the GitLab Token in a Kubernetes Secret**

This secret will be used by the `SecretStore` to authenticate with the GitLab API.

```bash
kubectl create secret generic openshift-eso-token \
  --from-literal=token='<YOUR_GITLAB_ACCESS_TOKEN>'
```

## Link of interest 
* [External Secrets Operator integrates with GitLab](https://external-secrets.io/latest/provider/gitlab-variables/)
* [ESO Secret Types](https://external-secrets.io/latest/guides/common-k8s-secret-types/)
* [Red Hat Blog: How an External Secrets Operator fetches the GitLab CI variable](https://www.redhat.com/en/blog/how-to-feed-external-secrets-for-kubernetes-applications-with-the-external-secret-operator-and-gitlab-on-red-hat-openshift)



