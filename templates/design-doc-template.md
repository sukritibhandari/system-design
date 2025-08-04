# Project Name: Multi Cloud Cost Monitor

## Overview  
This system provides automated, unified monitoring and reporting of cloud resource costs across the major providers: AWS, Azure, and GCP. It is designed for FinOps and DevOps professionals who need centralized visibility and auditing of their multi-cloud expenses. The ultimate goal is to deliver a customizable dashboard offering clear, daily visualizations of aggregated cloud costs.

## Goals and Requirements  
Functional Goals: 
    - Fetch daily billing data from all three cloud providers (AWS, Azure, GCP).
    - Clean and normalize the data into a unified format.
    - Store the normalized data in simple CSV files.
    - Send notifications summarizing daily bills and usage.
    - Trigger alerts when predefined cost thresholds are breached.
    - Automate the entire workflow using scheduled jobs (e.g., GitHub Actions).
    - Manage credentials securely to protect sensitive information.

Non-Functional Goals: 
    - Ensure scalability to handle increasing data volumes or additional accounts.
    - Maintain reliability with error handling and retry mechanisms.
    - Deliver a robust, bug-free solution through testing and validation.

## Architecture Diagram  
(Embed or link to the diagram file here)

## Components  
- Trigger Service: GitHub Actions workflow to invoke cost-collection daily
- Billing Clients: Python modules that interface with each cloud’s API
- Data Normalizer: Component that transforms provider-specific data into a common schema
- Storage Layer: CSV files stored in a /data directory
- Alert Engine: Module that checks daily totals against thresholds and sends emails via SMTP
- Reporting Module: Aggregates normalized data into summary CSVs and composes email reports

## Data Flow  
1) Trigger: GitHub Actions scheduled event triggers main.py daily.
2) Fetch: Billing Clients call each cloud API to retrieve cost summaries.
3) Normalize: Data Normalizer merges responses into a consistent record format.
4) Store: Write normalized records to CSV files (one per cloud).
5) Alert: Alert Engine reads CSV, compares costs to thresholds, sends email if needed.
6) Report: Reporting Module generates daily summary report and emails stakeholders.

## Technology Choices  
- Python: Versatile, strong SDK support for cloud APIs.
- GitHub Actions: Free CI/CD for scheduling and deployment.
- Terraform (future): Infrastructure as code for secret managers and cloud function deployment.
- SMTP/email: Simple alert mechanism without third-party dependencies.

## Security Considerations  
- Store cloud credentials and SMTP credentials in GitHub Secrets or cloud Secret Managers.
- Use least-privilege IAM roles for billing API access.
- Encrypt any data at rest (CSV in cloud storage) if using production storage.

## Scalability and Availability  
- Serverless Execution: Can evolve to AWS Lambda/GCP Functions for on-demand scaling.
- Retries & Backoff: Implement retry logic for API calls in case of transient errors.
- Parallel Execution: Fetch each cloud’s data concurrently to reduce total runtime.

## Cost Considerations  
- Leverage free GitHub Actions minutes and free-tier cloud functions for low execution cost.
- Use cheap storage (CSV in public repo for development, S3 Glacier or GCS Nearline for archival).
- Optimize API calls (fetch only necessary fields and time ranges).

## Trade-offs  
- CSV vs DB: CSV is simple and no extra hosting cost but less performant for large data volumes.
- Email vs Slack: Email has universal reach; Slack integration can be added later for real-time team alerts.
- GitHub Actions vs Serverless: GitHub Actions simplifies CI/CD but serverless functions provide better separation of concerns and scaling.

## Future Enhancements  
- Integrate Slack or other messaging platforms for alerts.
- Add a lightweight dashboard (Streamlit or Grafana) for visualizing cost trends.
- Move storage from CSV to a time-series database or data warehouse.
- Implement multi-account cost awareness and tagging support.
- Add authentication and multi-user configuration for threshold settings.
