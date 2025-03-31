# Deploy ArgoCD on GKE using GitHub Actions

This repository contains a GitHub Actions workflow to automate the deployment of ArgoCD on a Google Kubernetes Engine (GKE) cluster.

## Workflow Overview
The workflow is triggered on:
- Push events to the `develop`, `staging`, and `main` branches.
- Pull requests to the `develop`, `staging`, and `main` branches.
- Manual triggers via `workflow_dispatch`.

## Prerequisites
Before running this workflow, ensure the following requirements are met:
- A GKE cluster is available.
- A Google Cloud Service Account with the required permissions (`roles/container.admin`).
- GitHub secrets are set up:
  - `GCP_PROJECT_ID`: Google Cloud project ID.
  - `GCP_SA_KEY`: Base64-encoded JSON key of the Google Cloud Service Account.
  - `GKE_CLUSTER_NAME`: Name of the GKE cluster.
  - `GKE_REGION`: Region where the GKE cluster is deployed.

## Workflow Steps
### 1. Checkout Code
The workflow starts by checking out the repository code.

### 2. Setup Google Cloud SDK
The Google Cloud SDK is installed and authenticated using the provided service account credentials.

### 3. Install Dependencies
The workflow installs:
- `kubectl` for interacting with Kubernetes.
- `kustomize` for managing Kubernetes configurations.

### 4. Configure `kubectl` for GKE
The workflow retrieves GKE cluster credentials and configures `kubectl` to interact with the cluster.

### 5. Generate Kubernetes Manifests
`kustomize` is used to generate the ArgoCD deployment manifest from the `argocd/overlays/dev` directory.

### 6. Apply ArgoCD Deployment
The generated Kubernetes manifests are applied to the GKE cluster using `kubectl apply`.

### 7. Apply cert-manager Configuration
A cluster issuer configuration is applied to enable certificate management via cert-manager.

## Running the Workflow
To manually trigger the workflow, navigate to the repository’s **Actions** tab and select the `Deploy ArgoCD on GKE` workflow, then click **Run workflow**.

## Troubleshooting
If the workflow fails, check the following:
- Ensure all GitHub secrets are correctly configured.
- Verify that the service account has the required permissions.
- Inspect workflow logs for specific error messages.

## License
This project is licensed under the MIT License.