# Tinybird Organization Metrics Exporter

A **Tinybird project** designed to monitor Tinybird usage across an organization. This project uses Tinybird's **Service Datasources** to aggregate and expose organizational metrics, exporting them in **Prometheus format** for easy integration with monitoring tools.

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

1. You must have a **Tinybird organization**.
2. You need to be an **admin of the organization** to obtain the **Organization Token**.

### Steps

#### 1. Create or Select a Workspace  
- Create a new **Workspace** in your Tinybird organization specifically for monitoring purposes (recommended for better organization).  
- Alternatively, you can use an existing workspace.

#### 2. Deploy the Pipes  
- Clone this repository and deploy the pipes located in the `pipes` folder into your selected workspace.  
- Deployment options:
  - **Tinybird CLI**: Use `tb push` to deploy the pipes.
  - **Drag-and-Drop**: Upload the `datafiles` via the Tinybird UI.
  - **CI/CD Integration**: Automate deployment for future updates. This repository includes a GitHub Actions workflow template.  
    > **Important**: Ensure you configure the following secrets in your GitHub repository:
    > - `tb_admin_token`: Your Tinybird workspace token.  
    > - `tb_host`: The Tinybird host (e.g., `api.tinybird.co`, or a region-specific host like `api.us-east.tinybird.co`).  

#### 3. Use the Prometheus Endpoint  
- Access the Prometheus metrics endpoint at:  

`https://api.tinybird.co/v0/pipes/organization_metrics.prometheus`


- Replace `api.tinybird.co` with your Tinybird host if the workspace is in a different region.  
- Use your **Organization Token** to authenticate requests.

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
    bearer_token: '<organization-token>'
```


Start monitoring your Tinybird organization effortlessly with Prometheus metrics! 🎉
