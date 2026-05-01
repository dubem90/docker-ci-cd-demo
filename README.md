How to Run (Multi-Cloud Deployment)
This project demonstrates a manual CI/CD workflow, deploying a containerized application to both Google Cloud Run and Azure Container Apps.

1. Prerequisites
Docker Desktop installed and running.

Azure CLI installed.

Google Cloud SDK installed.

2. Infrastructure Setup
Before deploying, ensure the necessary cloud providers are registered and active.

For Azure:

PowerShell
# Login and select subscription
az login

# Register necessary providers (Required for new accounts)
az provider register -n Microsoft.App
az provider register -n Microsoft.OperationalInsights --wait
For GCP:

Bash
# Authenticate and set project
gcloud auth login
gcloud config set project [YOUR_PROJECT_ID]
3. Deployment Steps
Step A: Build and Push to Docker Hub
Bash
docker build -t your-username/docker-ci-cd-demo:latest .
docker push your-username/docker-ci-cd-demo:latest
Step B: Deploy to Azure Container Apps
PowerShell
az containerapp up `
  --name cicd-demo-azure `
  --resource-group MultiCloudDemo `
  --location northeurope `
  --image docker.io/your-username/docker-ci-cd-demo:latest `
  --ingress external `
  --target-port 3000
Step C: Deploy to Google Cloud Run
Bash
gcloud run deploy cicd-demo-gcp `
  --image your-username/docker-ci-cd-demo:latest `
  --region us-central1 `
  --platform managed `
  --allow-unauthenticated
🛠️ Challenges Overcome
Tenant Inactivity: Resolved Azure "Blocked Tenant" errors by activating a fresh subscription and verifying identity.

Resource Provider Propagation: Manually triggered Microsoft. OperationalInsights registration to enable serverless logging.

Multi-Region Deployment: Optimized latency by deploying the Azure instance to Northeurope to align with the local environment.
