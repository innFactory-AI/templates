# CI/CD Pipelines for innFactory AI Consulting

This repository provides reusable GitHub Actions workflows to quickly setup CI/CD in a project.

## Workflows

### [Build Only](./.github/workflows/build-only.yaml)

Builds a Docker image and pushes it to GitHub Container Registry (GHCR) with `latest` and specified version tags.

**Inputs:**
- `service` (required): Name of the service/image.
- `version` (required): Version tag for the image.

**Secrets:**
- `token` (required): GitHub token for authentication.

---

### [Build Project](./.github/workflows/build.yaml)

Builds a Node.js project. Installs dependencies using `npm install` and runs `npm run build`. Uploads the build output from `dist` folder as an artifact named `dist`.

**Secrets:**
- `token` (required): GitHub token for authentication.

---

### [Create Kubernetes Secrets from Azure Key Vault](./.github/workflows/create-secrets.yaml)

Creates a Kubernetes secret in an AKS cluster. It parses environment variables and application secrets provided via secrets inputs.

**Inputs:**
- `secret-name` (required): Name of the Kubernetes secret to create.
- `namespace` (required): Kubernetes namespace to deploy the secret to.

**Secrets:**
- `APPLICATION_SECRETS` (required): Application secrets in format `export KEY=value` (one per line).
- `AZURE_SECRETS` (required): Azure credentials in format `export ARM_CLIENT_ID=...`, etc.
- `CLUSTER_NAME` (required): AKS cluster name.
- `RESOURCE_GROUP` (required): Azure resource group name.

---

### [Build and Deploy Docker Container to GHCR](./.github/workflows/deploy.yaml)

Downloads the `dist` artifact (e.g., from the Build Project workflow), builds a Docker image, and pushes it to GitHub Container Registry (GHCR).

**Inputs:**
- `service` (required): Name of the service/image.
- `version` (required): Version tag for the image.

**Secrets:**
- `token` (required): GitHub token for authentication.

---

### [Install Helm Chart To Customer AKS](./.github/workflows/install-helm-to-customer-aks.yaml)

Deploys a Helm chart to a customer's Azure Kubernetes Service (AKS) cluster. It authenticates with Azure, updates the image repository in `values.yaml`, and executes `helm upgrade --install`.

**Inputs:**
- `image-name` (required): Name of the image.
- `version` (required): Version of the deployment.
- `namespace` (optional): Kubernetes namespace.

**Secrets:**
- `token` (required): GitHub token.
- `AZURE_SECRETS` (required): Azure credentials.
- `CLUSTER_NAME` (required): AKS cluster name.
- `RESOURCE_GROUP` (required): Azure resource group name.
- `REGISTRY` (required): Azure Container Registry name.

---

### [Publish Helm Chart to GHCR](./.github/workflows/publish-chart.yaml)

Packages a Helm chart located in `k8s/charts` and publishes it to the OCI registry at `ghcr.io/innfactory-ai/charts`.

**Inputs:**
- `service` (required): Name of the service.
- `version` (required): Version of the chart.

**Secrets:**
- `token` (required): GitHub token.

---

### [Build and Deploy Docker Container to Customer ACR (No Prior Build)](./.github/workflows/publish-to-customer-acr-without-prior-build.yaml)

Builds a Docker image directly from the source code (without downloading artifacts) and pushes it to a customer's Azure Container Registry (ACR).

**Inputs:**
- `image-name` (required): Name of the image.
- `version` (required): Version tag.

**Secrets:**
- `token` (required): GitHub token.
- `AZURE_SECRETS` (required): Azure credentials.
- `CLUSTER_NAME` (required): AKS cluster name.
- `RESOURCE_GROUP` (required): Azure resource group name.
- `REGISTRY` (required): Azure Container Registry name.

---

### [Build and Deploy Docker Container to Customer ACR](./.github/workflows/publish-to-customer-acr.yaml)

Downloads the `dist` artifact, builds a Docker image, and pushes it to a customer's Azure Container Registry (ACR).

**Inputs:**
- `image-name` (required): Name of the image.
- `version` (required): Version tag.

**Secrets:**
- `token` (required): GitHub token.
- `AZURE_SECRETS` (required): Azure credentials.
- `CLUSTER_NAME` (required): AKS cluster name.
- `RESOURCE_GROUP` (required): Azure resource group name.
- `REGISTRY` (required): Azure Container Registry name.

---

### [Update Helm Chart](./.github/workflows/update-chart.yaml)

Updates the version in `k8s/charts/Chart.yaml` and `k8s/charts/values.yaml`, then commits and pushes the changes to the `main` branch.

**Inputs:**
- `service` (required): Name of the service.
- `version` (required): New version string.

**Secrets:**
- `token` (required): GitHub token.