# Istio Ambient Mesh Demo

This project provides a complete local development environment for experimenting with Istio's Ambient Mesh mode using KinD (Kubernetes in Docker). The environment is managed through a Taskfile that automates the setup, deployment, and cleanup processes.

## 🚀 Features

- Automated local Kubernetes cluster setup using KinD
- Istio Ambient Mesh installation and configuration
- Sample service deployment for testing
- Metrics Server for resource monitoring
- Cert-Manager for TLS certificate management
- Node labeling for topology-aware deployments
- Comprehensive verification and cleanup tasks

## 📋 Prerequisites

Before using this Taskfile, ensure you have the following tools installed:

- [Task](https://taskfile.dev/) - Task runner / simpler Make alternative
- [KinD](https://kind.sigs.k8s.io/) - Kubernetes in Docker
- [kubectl](https://kubernetes.io/docs/tasks/tools/) - Kubernetes command-line tool
- [Helm](https://helm.sh/) - Kubernetes package manager
- [istioctl](https://istio.io/latest/docs/setup/getting-started/#download) - Istio command-line tool

## 🛠️ Available Tasks

The Taskfile provides the following main tasks:

### Cluster Management

- `create-local-cluster` - Creates a local Kubernetes cluster using KinD
- `setup-kube-context` - Configures kubectl context for the local cluster
- `delete-local-cluster` - Deletes the local cluster and frees resources

### Component Installation

- `install-metric-server` - Installs Kubernetes Metrics Server
- `install-cert-manager` - Installs Cert-Manager for TLS management
- `install-istio-ambient` - Installs Istio in Ambient Mesh mode
- `deploy-sample-services` - Deploys sample services (httpbin and sleep)

### Utility Tasks

- `tag-nodes` - Labels nodes for topology-aware deployments
- `verify-deployment` - Verifies the health of all deployed components
- `cleanup` - Comprehensive cleanup of all resources

### Combined Tasks

- `full-deploy-local` - Complete deployment of all components (recommended for first-time setup)

## 🚀 Getting Started

1. Clone this repository
2. Install all prerequisites
3. Run the full deployment:
   ```bash
   task full-deploy-local
   ```

This will:

- Create a local Kubernetes cluster
- Install all necessary components
- Deploy sample services
- Verify the deployment

## 🔍 Verification

After deployment, you can verify the installation using:

```bash
task verify-deployment
```

This will show the status of:

- Cluster nodes
- System pods
- Cert-Manager components
- Istio components

## 🧹 Cleanup

To clean up all resources and remove the local cluster:

```bash
task cleanup
```

## ⚙️ Configuration

The Taskfile uses the following variables that can be modified if needed:

- `CLUSTER_NAME`: Name of the KinD cluster (default: "istio-ambient-demo")
- `ISTIO_VERSION`: Version of Istio to install (default: "1.20")
- `GATEWAY_API_VERSION`: Version of Gateway API to install (default: "v1.2.1")

## 🔐 Security Notes

- The local cluster is for development purposes only
- TLS certificates are managed by Cert-Manager
- Metrics Server is configured with `--kubelet-insecure-tls` for local development

## 🔍 Testing Istio Ambient Mesh

After deployment, you can test the Istio Ambient Mesh connectivity between services. The following command demonstrates communication between the `sleep` and `httpbin` services:

```bash
kubectl exec -it deploy/sleep -c sleep -- curl httpbin.default.svc.cluster.local:8000/headers
```

This command will:

1. Execute a curl command from the `sleep` pod
2. Target the `httpbin` service in the default namespace
3. Request the `/headers` endpoint which returns the request headers

Expected output will show the HTTP headers of the request, including Istio-specific headers that demonstrate the Ambient Mesh is working correctly.
