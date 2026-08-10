# Pencil-icon

This repository contains a minimal static site and a GitHub Actions workflow for building a Docker image, publishing it to Google Artifact Registry, and deploying it to Google Kubernetes Engine (GKE).

## Included files

- `.github/workflows/build-deploy-gke.yml` — GitHub Actions workflow for build/publish/deploy
- `Dockerfile` — builds a static site served by NGINX
- `index.html` — placeholder static site content
- `deployment.yml` — Kubernetes Deployment manifest
- `service.yml` — Kubernetes Service manifest
- `kustomization.yml` — Kustomize manifest for deployment
- `.dockerignore` — Docker build context cleanup

## Setup

### 1. Configure GitHub secrets

Add the following repository secrets in GitHub Settings → Secrets → Actions:

- `GCP_PROJECT_ID`
- `GKE_CLUSTER`
- `GKE_ZONE`
- `DEPLOYMENT_NAME`
- `WORKLOAD_IDENTITY_PROVIDER`

If you want to run the workflow manually, you can also provide values through the workflow dispatch inputs.

### 2. Enable required Google Cloud APIs

Enable these APIs in your Google Cloud project:

- Artifact Registry: `artifactregistry.googleapis.com`
- Google Kubernetes Engine: `container.googleapis.com`
- IAM Credentials API: `iamcredentials.googleapis.com`

### 3. Enable Workload Identity Federation

Configure a GitHub Workload Identity Provider for your repository and give it permission to impersonate a Google service account.

### 4. Trigger the workflow

Push changes to the `main` branch or run the workflow manually from the Actions tab.

## Notes

- The workflow currently uses `us-central1` as the Artifact Registry location.
- The Kubernetes deployment image is updated using `kustomize edit set image` before applying.
- If you need a custom image tag or repository path, update the workflow and `kustomization.yml` accordingly.
