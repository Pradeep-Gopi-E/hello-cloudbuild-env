# Deployment Configurations for GitOps Pipeline

This repository contains the deployment manifests and Kubernetes configurations used in the **GitOps-style Continuous Delivery pipeline**. The configurations here are version-controlled and define the deployment process for the application running on **Google Kubernetes Engine (GKE)**.

## Overview

The _env_ repository stores Kubernetes manifests that describe the deployment of the application to different environments. This repository plays a crucial role in the **GitOps pipeline**, where changes to deployment configurations are automatically updated and deployed via Cloud Build when a new image is pushed to **Google Artifact Registry**.

### Repository Structure

This repository includes two primary branches:

- **_candidate_ branch**: Stores the deployment manifests for all attempts, both successful and failed. This serves as the staging area for testing new deployments before they go into production.
- **_production_ branch**: Holds the deployment manifests for the successfully deployed versions. This branch reflects the actual state of production and is always in sync with the latest successful deployment.

### How It Works

1. **Manifest Updates**: After a new container image is pushed to Artifact Registry by the CI pipeline in the _app_ repository, Cloud Build automatically updates the deployment manifests stored in this repository to point to the new image.
   
2. **Deployment Process**: The updated manifests are pushed to the **_candidate_ branch**, triggering a deployment to **Google Kubernetes Engine (GKE)**. If the deployment is successful, the manifests are then moved to the **_production_ branch**, marking it as the latest stable version.

3. **Rollback**: In case of any issues, deployments can be rolled back by re-executing previous Cloud Build jobs. The rollback process automatically updates the _production_ branch with the corresponding manifest from the previous successful deployment.

### Branch Management

- **_candidate_ branch**:
    - Used for tracking all deployment attempts.
    - Contains both successful and failed deployment configurations.
    - Allows for easy troubleshooting and testing of new deployments before they go live.

- **_production_ branch**:
    - Only contains deployment configurations that have been successfully deployed to production.
    - Represents the state of the live application on **GKE**.

### Benefits of Using This Repository

- **Auditability**: All deployment changes are tracked and stored in Git, making it easy to trace back any issues or changes.
- **Automated Deployments**: Changes pushed to the _candidate_ branch automatically trigger deployments, and only successful deployments make it to the _production_ branch.
- **Rollback Capability**: The versioned deployment configurations in Git allow for easy rollback to any previous successful deployment by re-executing the corresponding Cloud Build job.

## Conclusion

This repository serves as the source of truth for deployment configurations and is tightly integrated with the **GitOps pipeline**. By using Git to manage Kubernetes manifests, I ensure that deployments are automated, auditable, and easily reversible.
## References

- [Google Kubernetes Engine Pipeline using Cloud Build](https://www.cloudskillsboost.google/focuses/52829?parent=catalog)

