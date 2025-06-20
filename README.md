# Thunderbuddy K8s Service Endpoint Checker

K8s deployment that continuously checks the availability of internal service endpoints and reports issues via Slack and PagerDuty

## Overview

- Iterates over a list of namespaces and service names
- Curls internal service URLs every 60 seconds
- Sends Slack alerts on first failure and recovery
- Triggers and resolves PagerDuty incidents based on consecutive failures

## Configurations

### ConfigMap: endpoint-inputs

- `namespaces.txt`: List of thinstance namespaces
- `services.txt`: List of services to check per namespace

### ConfigMap: endpoint-script

- `check-endpoints.sh`: The health check and alerting logic

### Secret: all-secrets

- `SLACK_WEBHOOK_URL`: Slack Incoming Webhook
- `PAGERDUTY_API_KEY`: PagerDuty Events API v2 routing key

## Deployment

Apply all manifests nested in this file:

```bash
kubectl apply -f thunderbuddy.yaml
```

## Output Behavior

- ✅ Healthy services are logged
- ❌ Unreachable services trigger:
  - A Slack message on first failure
  - A PagerDuty incident on second consecutive failure only
- Recovery triggers:
  - A Slack recovery message
  - PagerDuty incident resolution

## Disabling

- To disable this tool go deployments under the `thunderbuddy` namespace and scale `endpoint-checker` to `0`



#
