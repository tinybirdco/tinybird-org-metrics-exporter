This Org Metrics Exporter template uses Tinybird's [Service Data Sources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources) to aggregate and publish organizational metrics as endpoints in [Prometheus format](https://www.tinybird.co/docs/forward/work-with-data/publish-data/guides/consume-api-endpoints-in-prometheus-format) for quick integration with common monitoring tools.

It exports ingestion, request, storage, job, cluster, data transfer, Sink, LLM
usage, backup, and Private Link metrics. The `organization_metrics` endpoint
combines every category, while `cluster`, `data_transfer`, `sinks`,
`llm_usage`, `backups`, and `private_link` can be scraped independently.

## Set up the project

```bash
# select or create a new workspace
tb login

# deploy the template
tb --cloud deploy --template https://github.com/tinybirdco/tinybird-org-metrics-exporter/tree/main/tinybird
```

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
    bearer_token: '<your-token>'
```

- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region. See [Regions and endpoints](https://www.tinybird.co/docs/api-reference#regions-and-endpoints).
- **Token**: for a quick check in development, you can use the pre-existing *admin `user@domain` Token* of an Organization admin to authenticate requests (find it in the [Tinybird dashboard](https://app.tinybird.co/tokens)). But for production, create a dedicated token with minimal permissions using the [Tinybird CLI](https://www.tinybird.co/docs/forward/get-started/administration/tokens):

```bash
tb --cloud token create static prometheus_org_access --scope "WORKSPACE:READ_ALL" --scope "ORG_DATASOURCES:READ"
```

We've included a sample dashboard config for Grafana to help you get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/grafana/tinybird_org_metrics.json).

## Datadog and OpenMetrics

Add the following configuration to your OpenMetrics Datadog agent `conf.yaml` file:

```yaml
instances:
  - openmetrics_endpoint: 'https://api.tinybird.co/v0/pipes/organization_metrics.prometheus'
    namespace: tinybird_org_metrics
    headers:
      Authorization: 'Bearer <your-token>'
    metrics:
      - "*"
    max_returned_metrics: 700000
```

- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region. See [Regions and endpoints](https://www.tinybird.co/docs/api-reference#regions-and-endpoints).

We've included a sample dashboard config for Datadog that you can use to get started, see the [JSON file](https://github.com/tinybirdco/tinybird-org-metrics-exporter/blob/main/datadog/tinybird_org_metrics.json).
