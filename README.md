# Kargo Quickstart Tutorial 

These are the supporting files for [the Kargo Quickstart tutorial](https://docs.akuity.io/tutorials/kargo-quickstart/) in the Akuity docs.

Setup
The tutorial was executed inside a GitHub Codespace.
Updated the Kargo installation/version to a current compatible version.

Ultimately, you will have a Kubernetes cluster, with Applications deployed using an Argo CD control plane; and handle promotion with Kargo.


This project demonstrates a GitOps-based application delivery workflow using Kargo, Argo CD, Kubernetes, Kustomize, and GitHub Container Registry (GHCR).

Overall Design:
```text
Container Registry
      │
      │ New image
      ▼
   Kargo Warehouse
      │
      │ Creates/identifies Freight
      ▼
    Kargo Stage
      │
      │ Promotion
      ▼
Kustomize rendering
      │
      │ Rendered manifests
      ▼
   Git environment branch
      │
      ▼
    Argo CD
      │
      ▼
Kubernetes cluster
```
The application is promoted through three environments:
dev → staging → prod

The environment branches contain the rendered manifests produced during Kargo promotion.

Argo CD then watches the appropriate environment branch and reconciles the rendered manifests with the target Kubernetes cluster.

Design Decisions: 

1. Highlighting the Rendered Manifests Pattern:
Kustomize was selected because the existing tutorial already uses Kustomize

2. Integrating the Pull Request approval into the workflow for Prod Promotion:
The original tutorial workflow directly pushed the promoted manifests to the env/prod branch.
For a production environment, I changed this behavior so that Kargo:

Opens a GitHub pull request targeting env/prod.
Waits for the pull request to be merged.
Updates Argo CD only after the production change has passed through the Git review workflow.

