This Org Metrics Exporter template uses Tinybird's [Service Data Sources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources) to aggregate and publish organizational metrics as endpoints in [Prometheus format](https://www.tinybird.co/docs/guides/integrations/consume-api-endpoints-in-prometheus-format) for quick integration with common monitoring tools.

## Set up the project

Fork the GitHub repository and deploy the data project to Tinybird.

## Grafana and Prometheus

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


- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region. See [Regions and endpoints](https://www.tinybird.co/docs/api-reference#regions-and-endpoints).  
- Use `admin user@domain Token` of an Organization admin to authenticate requests. Find it in the [Tinybird dashboard](https://app.tinybird.co/tokens).

We've included a sample dashboard config for Grafana to help you get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/grafana/tinybird_org_metrics.json).

## Datadog and OpenMetrics

Add the following configuration to your OpenMetrics Datadog agent `conf.yaml` file:

```yaml
instances:
  - prometheus_url: 'https://api.tinybird.co/v0/pipes/organization_metrics.prometheus?token=<admin-user-token>'
    namespace: tinybird_org_metrics
    metrics:
      - "*"
    max_returned_metrics: 700000
```


- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region. See [Regions and endpoints](https://www.tinybird.co/docs/api-reference#regions-and-endpoints).  
- Use `admin user@domain Token` of an Organization admin to authenticate requests. Find it in the [Tinybird dashboard](https://app.tinybird.co/tokens).

We've included a sample dashboard config for Datadog that you can use to get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/datadog/tinybird_org_metrics.json).
