# Day 86 – GitOps Project: End-to-End CI/CD Pipeline with AI-BankApp

## What I Learned

Today I worked on the complete GitOps CI/CD pipeline using AI-BankApp. The goal was to connect GitHub Actions, DockerHub, Kubernetes, ArgoCD, and AWS EKS into one automated deployment workflow.

### 1. GitHub Actions CI Pipeline

I studied the `gitops-ci.yml` workflow and understood how it runs when application code changes.

The main steps are:

* Checkout the source code.
* Set up Java 21 and Maven.
* Build the application.
* Run tests.
* Build and push the Docker image.
* Update the Kubernetes deployment manifest.
* Commit the updated image tag back to Git.

### 2. GitOps Workflow

The complete workflow is:

```text
Developer
    |
    v
Git push to GitHub
    |
    v
GitHub Actions
    |
    +--> Build Maven application
    |
    +--> Run tests
    |
    +--> Build Docker image
    |
    +--> Push image to DockerHub
    |
    +--> Update Kubernetes manifest
    |
    v
Git repository
    |
    v
ArgoCD
    |
    v
AWS EKS
    |
    v
Application deployed
```

### 3. Image Tagging

I learned how the pipeline uses the short Git commit SHA as the Docker image tag.

Example:

```text
trainwithshubham/ai-bankapp-eks:1c7cb0e
```

This helps connect the running application to the exact Git commit that created the image.

### 4. ArgoCD and Self-Healing

I practiced how ArgoCD compares the desired state in Git with the actual state in Kubernetes.

I tested:

* Scaling down the deployment.
* Changing the container image directly.
* Deleting the Kubernetes service.

These tests helped me understand drift detection and self-healing.

### 5. Complete DevOps Pipeline

This project connected the different topics I learned throughout the 90-day challenge:

* Git and GitHub.
* GitHub Actions.
* Docker.
* Kubernetes.
* Helm.
* AWS EKS.
* ArgoCD.
* Monitoring and observability.

### Key Takeaways

1. GitOps uses Git as the source of truth for deployment configuration.
2. GitHub Actions automates the CI part of the pipeline.
3. ArgoCD automates the deployment of the desired state to Kubernetes.
4. Git commit SHA tags provide traceability for Docker images.
5. Self-healing helps restore resources when someone changes them manually.
6. Proper teardown is important when working with AWS resources to avoid unnecessary charges.

### Result

Completed the Day 86 GitOps project and understood how a code change can move from GitHub to a running application on AWS EKS through an automated CI/CD pipeline.

**Next:** Continue learning and improving my DevOps skills.
