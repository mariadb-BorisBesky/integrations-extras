# Agent Check: PureFA

## Overview

This check monitors the [Pure Storage FlashArray][1] through the [Datadog Agent][2] and the [Pure Storage Prometheus exporter][1].

Note: This integration requires OpenMetricsBaseCheckV2 which is available in Agent v7.26.x+ and requires Python 3

Note: This integration requires the use of the [Pure Storage Prometheus exporter][1]

## Setup

Follow the instructions below to install and configure this check for an Agent running on a host. For containerized environments, see the Autodiscovery Integration Templates for guidance on applying these instructions.

### Installation

1. [Download and launch the Datadog Agent][9].
2. Manually install the Pure FlashArray integration. See [Use Community Integrations][10] for more details based on the environment.


#### Host

To configure this check for an Agent running on a host, run `datadog-agent integration install -t datadog-purafa==<INTEGRATION_VERSION>`.

### Configuration

1. Create a local user on your FlashArray with Read-Only role and generate an API token for this user.
   ![Generate an API Key](https://raw.githubusercontent.com/DataDog/integrations-extras/master/purefa/images/API.png) 
2. Add this configuration block to the `purefa.d/conf.yaml` file, in the `conf.d/` folder at the root of your Agent's configuration directory to start collecting your purefa performance data. See the sample [purefa.d/conf.yaml][4] for all available configuration options.

Note: The `/array` endpoint is required as an abolute minimum when creating your configuration file.

```yaml
init_config:
   timeout: 60

instances:

  - openmetrics_endpoint: http://<exporter_ip_or_fqdn>:9491/metrics/flasharray/array?endpoint=<array_ip_or_fqdn>
    tags:
       - env:<env>
       - fa_array_name:<full_fqdn>
       - host:<full_fqdn>
    hostname_label: host
    hostname_format: <HOSTNAME>.<domain_name>
    headers:
       Authorization: Bearer <api_token>
    min_collection_interval: 120
    empty_default_hostname: true
    max_returned_metrics: 100000

  - openmetrics_endpoint: http://<exporter_ip_or_fqdn>:9491/metrics/flasharray/volumes?endpoint=<array_ip_or_fqdn>
    tags:
       - env:<env>
       - fa_array_name:<full_fqdn>
    hostname_label: host
    hostname_format: <HOSTNAME>.<domain_name>
    headers:
       Authorization: Bearer <api_token>
    min_collection_interval: 120
    empty_default_hostname: true
    max_returned_metrics: 100000

  - openmetrics_endpoint: http://<exporter_ip_or_fqdn>:9491/metrics/flasharray/hosts?endpoint=<array_ip_or_fqdn>
    tags:
       - env:<env>
       - fa_array_name:<full_fqdn>
    hostname_label: host
    hostname_format: <HOSTNAME>.<domain_name>
    headers:
       Authorization: Bearer <api_token>
    min_collection_interval: 120
    empty_default_hostname: true
    max_returned_metrics: 100000

  - openmetrics_endpoint: http://<exporter_ip_or_fqdn>:9491/metrics/flasharray/pods?endpoint=<array_ip_or_fqdn>
    tags:
       - env:<env>
       - fa_array_name:<full_fqdn>
       - host:<full_fqdn>
    hostname_label: host
    hostname_format: <HOSTNAME>.<domain_name>
    headers:
       Authorization: Bearer <api_token>
    min_collection_interval: 120
    empty_default_hostname: true
    max_returned_metrics: 100000
```
2. [Restart the Agent][5].

### Validation

[Run the Agent's status subcommand][6] and look for `purefa` under the Checks section.

## Data Collected

### Metrics

See [metadata.csv][7] for a list of metrics provided by this check.

### Events

The PureFA integration does not include any events.

### Service Checks

The PureFA integration does not include any service checks.

## Support

For support or feature requests, contact Pure Storage through the following methods:
* Email: pure-observability@purestorage.com
* Slack: [Pure Storage Code// Observability Channel][11].

[1]: https://github.com/PureStorage-OpenConnect/pure-exporter
[2]: https://app.datadoghq.com/account/settings#agent
[4]: https://github.com/PureStorage-OpenConnect/observability/blob/master/datadog/integrations-extras/purefa/datadog_checks/purefa/data/conf.yaml.example
[5]: https://docs.datadoghq.com/agent/guide/agent-commands/#start-stop-and-restart-the-agent
[6]: https://docs.datadoghq.com/agent/guide/agent-commands/#agent-status-and-information
[7]: https://github.com/PureStorage-OpenConnect/observability/blob/master/datadog/integrations-extras/purefa/metadata.csv
[9]: https://app.datadoghq.com/account/settings#agent
[10]: https://docs.datadoghq.com/agent/guide/community-integrations-installation-with-docker-agent
[11]: https://code-purestorage.slack.com/messages/C0357KLR1EU