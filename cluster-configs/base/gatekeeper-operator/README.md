# Gatekeeper Operator

The Gatekeeper Operator provides policy-based admission control for Kubernetes clusters using the Open Policy Agent (OPA). It allows you to define and enforce custom resource constraints using the Rego policy language.

## Introduction

Gatekeeper integrates Open Policy Agent (OPA) with Kubernetes as a validating admission webhook. It provides:

- **Policy as Code**: Define policies using Rego language
- **Admission Control**: Validate and potentially modify resources before they are persisted
- **Audit Capabilities**: Continuously monitor cluster resources for policy violations
- **Constraint Templates**: Reusable policy definitions that can be parameterized
- **Constraints**: Instances of constraint templates with specific parameters

## How it works

The Gatekeeper operator deploys several components:

### Core Components

- **Gatekeeper Controller Manager**: Manages constraint templates and constraints
- **Validating Admission Webhook**: Intercepts resource creation/update requests
- **Audit Controller**: Periodically scans existing resources for violations
- **Constraint Template Controller**: Manages the lifecycle of constraint templates

### Workflow

1. **Policy Definition**: Create ConstraintTemplate resources defining policy logic
2. **Policy Instantiation**: Create Constraint resources from templates with specific parameters
3. **Admission Control**: Webhook validates incoming requests against active constraints
4. **Audit**: Controller scans existing resources and reports violations


### Key Configuration Options

- **auditInterval**: How often the audit controller scans for violations
- **violationsToReport**: Maximum number of violations to report per constraint
- **failurePolicy**: How to handle webhook failures (Ignore/Fail)
- **webhookTimeout**: Timeout for webhook calls in seconds
- **logLevel**: Logging verbosity level



## Useful Commands

Check operator installation:
```bash
oc get subscription -n openshift-gatekeeper-operator
oc get csv -n openshift-gatekeeper-operator
```

Check Gatekeeper instance:
```bash
oc get gatekeeper -n openshift-gatekeeper-system
oc describe gatekeeper gatekeeper -n openshift-gatekeeper-system
```

View Gatekeeper pods:
```bash
oc get pods -n openshift-gatekeeper-system
```

List constraint templates:
```bash
oc get constrainttemplates
```

List constraints:
```bash
oc get constraints
```

Check for policy violations:
```bash
oc get constrainttemplates -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}' | xargs -I {} oc get {} -o yaml
```

View webhook configuration:
```bash
oc get validatingwebhookconfigurations | grep gatekeeper
```

## Requirements

- OpenShift 4.x cluster
- Cluster admin privileges for operator installation
- Understanding of OPA/Rego for policy development

## Policy Development

### Example Constraint Template

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        properties:
          labels:
            type: array
            items:
              type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels
        
        violation[{"msg": msg}] {
          required := input.parameters.labels
          provided := input.review.object.metadata.labels
          missing := required[_]
          not provided[missing]
          msg := sprintf("Missing required label: %v", [missing])
        }
```

### Example Constraint

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: must-have-env
spec:
  match:
    kinds:
      - apiGroups: ["apps"]
        kinds: ["Deployment"]
  parameters:
    labels: ["environment", "team"]
```

## Performance Considerations

- **Webhook Timeout**: Keep webhook timeout reasonable (1-3 seconds) to avoid client throttling
- **Audit Frequency**: Balance audit frequency with cluster load
- **Constraint Scope**: Use specific match criteria to minimize evaluation overhead
- **Resource Limits**: Set appropriate resource limits for Gatekeeper components

## Troubleshooting

Common issues and solutions:

1. **Webhook Timeouts**: Reduce webhook timeout or optimize policy logic
2. **Admission Failures**: Check failurePolicy setting and webhook logs
3. **Policy Violations**: Review constraint logic and audit logs
4. **Performance Issues**: Monitor resource usage and adjust configuration

## Links

- [Open Policy Agent Gatekeeper Documentation](https://open-policy-agent.github.io/gatekeeper/)
- [Gatekeeper Policy Library](https://github.com/open-policy-agent/gatekeeper-library)
- [OPA Rego Documentation](https://www.openpolicyagent.org/docs/latest/policy-language/)
- [Red Hat OpenShift Gatekeeper Operator](https://docs.openshift.com/container-platform/latest/security/gatekeeper-operator.html)
- [Constraint Template Library Reference](https://cloud.google.com/anthos-config-management/docs/latest/reference/constraint-template-library)
- [Gatekeeper Best Practices](https://open-policy-agent.github.io/gatekeeper/website/docs/operations)