# Alertmanager Configuration
Alertmanager is used to process alerts sent from Prometheus. It will turn these alerts into notifications that can be sent to external systems. These are called [receivers](https://prometheus.io/docs/alerting/latest/configuration/#receiver). In order to ensure only the wanted alerts gets through, one can filter them when setting up [routes](https://prometheus.io/docs/alerting/latest/configuration/#route).

> NOTE: In order to sent alerts to Microsoft Teams, the [prometheus-msteams](https://code.bbraun.io/IT-WI-OpenShift/multi-cluster-configs/tree/main/base/prometheus-msteams) chart needs to be installed.

## Configuration
The customizable values are:
- If msTeams is enabled or not.
- Which channel in Teams that should be getting the alerts.
- The smarthost for sending emails.
- The 'from' address for emails.
- A default list of email recipients.
- An optional list of email recipients for `critical` alerts.
> NOTE that when msTeams is enabled, and prometheus-msteams is installed in the cluster, all notifications from AlertManager will be sent to the **prometheus-msteams** pod to be processed before forwarding it to the Teams channel.

Example of all configurables:
```yaml
alertConfig:
  enabled: true

cluster:
  name: sandbox

msTeams:
  enabled: false
  connector: openshift-to-teams # This needs to match the name that is used for prometheus-msteams

email:
  smarthost: 'smtp.example.com:25'
  from: 'openshift-noreply@example.com'
  default:
    - suresh@example.com
    - oscar@example.com
    - guru@example.com
  critical: 
    - suresh@example.com
  send_resolved: true
```

### How the alerts are being processed
The base receiver is a default list of emails. You can override this for `critical` alert severities by creating a list of emails for that severity.

1.  If `msTeams.enabled` is `true`:
    - All alerts are sent to the Teams channel.
    - `critical` alerts are also sent to the email addresses in `email.critical`. If that list is empty, they are sent to the `email.default` list.
2.  If `msTeams.enabled` is `false`:
    - `critical` alerts are sent to the email lists corresponding to their severity.
    - If a severity-specific list is empty, the alert is sent to the `email.default` list.
3.  Alerts with severity `none` (e.g., Watchdog) are sent to a "blackhole" and are not received.


## Useful commands
In order to view the configuration on a running cluster via the CLI:
```bash
oc -n openshift-monitoring get secret alertmanager-main --template='{{ index .data "alertmanager.yaml" }}' |base64 -d 
```

[OpenShift Documentation on Alerting](https://docs.openshift.com/container-platform/latest/monitoring/managing-alerts.html)
