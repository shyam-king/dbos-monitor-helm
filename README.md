# About
Helm chart for [DBOS Monitor](https://github.com/shyam-king/dbos-monitor)

# WARNING: New Project
This is a new project and not yet stable!

# Usage

## Prerequisites

- Kubernetes cluster (e.g., Minikube, kind, EKS, GKE, etc.)
- Helm 3+

## Installing the Chart

You can add this repository as a standard Helm repository and install it directly via `helm`:

```bash
# 1. Add the Helm repository
helm repo add dbos-monitor https://shyam-king.github.io/dbos-monitor-helm
helm repo update

# 2. Install the chart
helm install my-release dbos-monitor/dbos-monitor
```

To install a specific version of the `dbos-monitor` image (e.g., `v0.0.1`), you can override the image tag:

```bash
helm install my-release dbos-monitor/dbos-monitor --set image.tag=v0.0.1
```

> Available tags can be found in [Docker Hub](https://hub.docker.com/r/shyamking/dbos-monitor/tags)

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm uninstall my-release
```

## Configuration Options

### Mandatory ConfigMap

The `dbos-monitor` service requires a ConfigMap containing configuration environment variables to function correctly. You must create this ConfigMap in your cluster prior to installing the chart, and pass its name using the `configMapName` value. 

If `configMapName` is not provided, the `helm install` command will intentionally fail. 

For information on what variables the ConfigMap should contain, please refer to the [DBOS Monitor Configuration Reference](https://github.com/shyam-king/dbos-monitor#configuration).

**Example Installation:**

```bash
# 1. Create a ConfigMap with the required environment variables
kubectl create configmap my-monitor-config --from-literal=DBOS_MONITOR_DBOS_POSTGRES_CONNECTION_URI="postgres://..."

# 2. Install the Helm chart and reference the ConfigMap
helm install my-release dbos-monitor/dbos-monitor \
  --set configMapName=my-monitor-config \
  --set image.tag=v0.0.1
```

### General Configuration

You can override defaults by modifying `charts/dbos-monitor/values.yaml` directly, or by passing `--set key=value` arguments to the `helm install` command.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `configMapName` | **(Required)** Name of an existing ConfigMap with env vars | `""` |
| `image.repository` | Docker image repository | `shyamking/dbos-monitor` |
| `image.tag` | Image tag | `latest` |
| `replicaCount` | Number of pod replicas | `1` |
| `service.port` | Service port exposed to the cluster | `8080` |
| `autoscaling.enabled` | Enable Horizontal Pod Autoscaling | `false` |

# Maintainers
shyam-king
