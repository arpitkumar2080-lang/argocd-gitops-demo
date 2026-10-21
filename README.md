# Project 10: GitOps Workflow using ArgoCD on Kubernetes

## Project Overview
This project is based on GitOps principles, where the GitHub repository acts as the "single source of truth" for infrastructure and applications. It sets up a fully automated deployment pipeline using ArgoCD and Minikube (Kubernetes), eliminating the need for manual cluster modifications.

## Tools Used
* **Minikube:** To run the Local Kubernetes Cluster.
* **ArgoCD:** GitOps continuous delivery tool.
* **GitHub:** For version control and storing manifest files.
* **Docker:** Container runtime environment.

## Step-by-Step Implementation Guide

### 1. Local Kubernetes Cluster Setup
* Started Docker Desktop in the background.
* Initialized the Minikube cluster with the Docker driver:
  ```bash
  minikube start --driver=docker

  Verified the cluster status:

Bash
kubectl get nodes
2. Git Repository & Manifest Setup
Created a new repository (argocd-gitops-demo) on GitHub.

Created a deployment.yaml file for the NGINX application with the initial state set to replicas: 2.

Committed and pushed the file to the Git repository.

3. ArgoCD Installation on Kubernetes
Created a dedicated namespace for ArgoCD:

Bash
kubectl create namespace argocd
Applied ArgoCD's official manifests to the cluster:

Bash
kubectl apply -n argocd -f (https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml)
Configured port-forwarding to access the ArgoCD UI on the local browser:

Bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
Extracted and base64 decoded the default admin password to log into the dashboard.

4. GitOps Application Configuration
Configured a new application after logging into the ArgoCD dashboard:

Repository URL: https://github.com/arpitkumar2080-lang/argocd-gitops-demo.git

Path: ./ (Since the manifest is in the root folder)

Cluster URL: https://kubernetes.default.svc

Namespace: default

Sync Policy: Enabled 'Automatic', along with 'Prune Resources' and 'Self Heal' options so the cluster always matches the Git state.

5. GitOps Auto-Sync Testing
To verify the GitOps flow, edited the deployment.yaml file in the GitHub repository, changing replicas: 2 to replicas: 3.

ArgoCD automatically detected this new commit (desired state) and immediately deployed a new pod in the Kubernetes cluster (live state).

The application state was successfully verified as "Synced" and "Healthy" on the ArgoCD dashboard.


Conclusion
This implementation proved that integrating ArgoCD with Kubernetes makes the deployment process fast, secure, and error-free. Developers do not need to make direct changes to the cluster; everything is managed and tracked through GitHub commits.
