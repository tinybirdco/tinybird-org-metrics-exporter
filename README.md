# Tinybird Organization Metrics Exporter

A **Tinybird project** designed to monitor Tinybird usage across an organization. This project uses Tinybird's **[Service Datasources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources)** to aggregate and expose organizational metrics, exporting them in **Prometheus format** for easy integration with monitoring tools.

## Features

- Includes a set of **Pipes** that consolidate and process organizational metrics.  
- Exposes all key metrics through a **single Pipe Endpoint** in **Prometheus format** for streamlined monitoring.  
- Provides essential insights to help you **monitor**, **detect anomalies**, and **set up alerts** in critical areas such as:  
  - **Data ingestion**: Track the volume and frequency of ingested data.  
  - **Pipe endpoint usage**: Analyze requests and identify unusual traffic patterns.  
  - **Storage**: Monitor storage usage to optimize resources and avoid limits.  
  - **Jobs**: Keep track of the status and performance of scheduled jobs.  

## Setup



### Prerequisites

1. You must have a **[Tinybird organization](https://www.tinybird.co/docs/monitoring/organizations)**.
2. You need to be an **admin of the organization** to access [organization service datasources](https://www.tinybird.co/docs/monitoring/organizations#organization-service-data-sources).

### Steps




#### 1. Deploy to a Tinybird Workspace

<p align="center">
  <a href="https://app.tinybird.co?starter_kit=https://github.com/tinybirdco/tinybird-org-metrics-exporter">
    <img width="300" src="https://img.shields.io/badge/Deploy%20to-Tinybird-25283d?style=flat&labelColor=25283d&color=27f795&logo=data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgNTAwIDUwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNNTAwIDQyLjhsLTE1Ni4xLTQyLjgtNTQuOSAxMjIuN3pNMzUwLjcgMzQ1LjRsLTE0Mi45LTUxLjEtODMuOSAyMDUuN3oiIGZpbGw9IiNmZmYiIG9wYWNpdHk9Ii42Ii8+PHBhdGggZD0iTTAgMjE5LjlsMzUwLjcgMTI1LjUgNTcuNS0yNjguMnoiIGZpbGw9IiNmZmYiLz48L3N2Zz4=" />
  </a>
</p>


#### 2. Use the Prometheus Endpoint  
- Access the Prometheus metrics endpoint at:  

`https://api.tinybird.co/v0/pipes/organization_metrics.prometheus`


- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region.  
- Use **admin `user@domain` Token** of an Organization admin or a token with read scope for `organization_metrics` pipe  to authenticate requests.

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
    bearer_token: '<pipe-read-token>'  # With permissions for organization_metrics Pipe
```

In case you use a different setup such as Datadog + OpenMetrics, you can use the following configuration:

```yaml
instances:
  - prometheus_url: https://api.tinybird.co/v0/pipes/organization_metrics.prometheus?token=<pipe-read-token>
    namespace: tinybird
    metrics:
      - "*"
    tags:
      - tinybird
    max_returned_metrics: 10000 # Adjust this value as needed
```

Remember to replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region and use a `pipe-read-token` with permissions for the `organization_metrics` pipe.

Start monitoring your Tinybird organization effortlessly with Prometheus metrics! 🎉

![Grafana dashboard example](./assets/img/grafana.png)

