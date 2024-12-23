Monitor Tinybird usage across your Tinybird organization. 

This template uses Tinybird's [Service Data Sources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources) to aggregate and publish organizational metrics as endpoints in [Prometheus format](https://www.tinybird.co/docs/guides/integrations/consume-api-endpoints-in-prometheus-format) for quick integration with common monitoring tools.

Once deployed it provides essential insights to help you monitor your usage, detect anomalies, and set up alerts in critical areas such as:  
  - Data ingestion: Track the volume and frequency of ingested data
  - Pipe endpoint usage: Analyze requests and identify unusual traffic patterns
  - Storage: Monitor storage usage to optimize resources and avoid limits
  - Jobs: Keep track of the status and performance of scheduled jobs


## 0. Prerequisites

1. You must have a [Tinybird organization](https://www.tinybird.co/docs/monitoring/organizations)
2. You need to be an admin of the organization to access [organization Service Data Sources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources)

## 1. Set up the project

Fork the GitHub repository and deploy the data project to Tinybird.

## 2. Grafana and Prometheus quick start

To scrape the Tinybird metrics endpoint, you can configure your `prometheus.yml` file as follows:

```yaml
scrape_configs:
  - job_name: tinybird_org_metrics
    scrape_interval: 15s  # Adjust the scrape interval as needed
    scheme: 'https'
    static_configs:
      - targets: 
        - 'api.tinybird.co'  # Adjust this for your region if necessary
    metrics_path: '/v0/pipes/organization_metrics.prometheus'
    bearer_token: '<admin-user-token>'  # From an Organization admin
```

- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region.  
- Use `admin user@domain Token` of an Organization admin to authenticate requests.

We've included a sample dashboard config for Grafana to help you get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/grafana/tinybird_org_metrics.json).


## 3. Datadog and OpenMetrics quick start

Add the following configuration to your OpenMetrics Datadog agent `conf.yaml` file:

```yaml
instances:
  - prometheus_url: 'https://api.tinybird.co/v0/pipes/organization_metrics.prometheus?token=<admin-user-token>'
    namespace: tinybird_org_metrics
    metrics:
      - "*"
    max_returned_metrics: 700000
```

- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region.  
- Use `admin user@domain Token` of an Organization admin to authenticate requests.

We've included a sample dashboard config for Datadog that you can use to get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/datadog/tinybird_org_metrics.json).

## 3. Learn more

To learn more about this template check out the [README](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/README.md).

## 4. Support

If you have any questions or need help, please reach out to us on [Slack](https://www.tinybird.co/join-our-slack-community) or [email](mailto:support@tinybird.co).
