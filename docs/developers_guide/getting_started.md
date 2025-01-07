# How to Start

This guide walks you through the initial steps to start working with your workload in CGDevX. By following these instructions, you'll learn how to prepare your workload repository, build your application, and deploy it to Kubernetes.

## Overview

CGDevX simplifies the adoption of modern development practices by integrating open-source tools into a cohesive platform. Getting started involves these key steps:

- Requesting repository setup.
- Preparing your workload repository.
- Building and deploying your workload.
- Monitoring your deployment.

Each step is designed to minimize manual intervention, allowing you to focus on application development.

## Step 1: Request Repository Setup

Before starting, request your platform operator to provision the necessary repositories for you. By default, you'll receive:

- **Workload Repository**:  
  Stores your application's source code and the `Dockerfile` for containerization. Learn more about workload concepts in the [Workloads Guide](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/workloads/concept/).

- **GitOps Repository**:  
  Contains Kubernetes manifests, Terraform configurations, and environment-specific values for managing deployments. Learn more about GitOps structure and environments in the [How to Model Your Environments](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/workloads/gitops_environments/).

> **Why it matters**: These repositories separate application logic from infrastructure, ensuring clear boundaries and maintainability.

For more details on the structure of these repositories, refer to the [Bootstrap Templates Guide](https://cloudgeometry.github.io/cg-devx-docs/operators_guide/workload_management/bootstrap_templates/).

## Step 2: Prepare Your Workload Repository

Once you have access to the **Workload Repository**, add your application’s source code and customize the provided `Dockerfile` to match your workload’s requirements.

- Use the [CGDevX Workload Template](https://github.com/CloudGeometry/cg-devx-wl-template) as a basic starting point for your repository setup.
- As a real-world example, check out [CGDevX CNASK Workload](https://github.com/CloudGeometry/cg-devx-wl-cnask) for a working implementation.

## Step 3: Build and Deploy Your Workload

The next step is to build and deploy your application. This process is automated via CGDevX’s CI/CD pipelines.

### Trigger the Build

Push a tag to your Workload Repository to trigger the build process:

```bash
git tag v0.0.1
git push origin v0.0.1
```

### Behind the Scenes

When you push a tag to your **Workload Repository**:

1. **GitHub Actions** submits an **Argo Workflow** with the required parameters.
2. **Argo Workflow** builds the container image, pushes it to the registry, and updates the **GitOps Repository**.
3. **ArgoCD** syncs the updated configurations and deploys your application.

This automated pipeline streamlines building, deploying, and syncing your workload.

## Step 4: Monitor Your Deployment

Once deployed, you can verify your application and monitor its performance:

### Access Your Application

- Navigate to the **Ingress URL** listed in the ArgoCD dashboard to access your application.

### Monitor Logs and Metrics

- **Metrics**: Use [Grafana](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/observability/monitoring/) to track application performance and resource usage.
- **Dashboards**: Explore the [pre-configured Grafana dashboards](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/observability/dashboards/) to visualize metrics and monitor workload health.
- **Logs**: Use [Loki](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/observability/log_management/) for centralized log aggregation and troubleshooting.

## Step 5: Troubleshoot Issues

If the deployment encounters any issues, use the following tools to diagnose and resolve them:

- **Argo Workflows**: Review the [Argo Workflows dashboard](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/ci/integration/) for details on pipeline execution.
- **ArgoCD Dashboard**: Check deployment logs directly in the ArgoCD dashboard for insights into application issues.
- **Loki Logs**: Use [Loki](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/observability/log_management/) to analyze application and Kubernetes logs for potential errors.

## What's Next?

Once your workload is up and running, explore these guides to make the most out of CGDevX:

- [Working with ArgoCD](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/cd/delivery/): Learn how to manage and monitor deployments using ArgoCD.
- [Promoting Workloads Across Environments](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/workloads/promotion/): Move your workload between dev, staging, and production environments.
- [Code Quality Practices](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/ci/code_quality/): Improve code quality with actionable best practices.
- [Static Code Analysis with MegaLinter](https://cloudgeometry.github.io/cg-devx-docs/developers_guide/ci/github_action_workflow/#megalinter): Ensure code quality through automated linting.

Explore these resources to unlock the full potential of CGDevX.
