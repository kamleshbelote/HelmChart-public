# ParentApp Helm Chart

## Introduction

This repository contains a Helm chart for deploying the ParentApp application on Kubernetes clusters.
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/commonchart)](https://artifacthub.io/packages/search?repo=commonchart)


## Prerequisites

- Kubernetes 1.16+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure (if persistence is needed)

## Adding the Helm Repository

```bash
helm repo add parentappv1 https://kamleshbelote.github.io/HelmChart/parentapp/
helm repo update
```

## Installing the Chart

```bash
# Install with release name "my-app"
helm install my-app parentappv1/parentapp

# Install with custom values
helm install my-app parentappv1/parentapp --set key=value

# Install with custom values file
helm install my-app parentappv1/parentapp -f my-values.yaml
```

<!-- If you encounter "cannot re-use a name that is still in use" error, choose a different release name or uninstall the existing release first -->

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Image repository | `nginx` |
| `image.tag` | Image tag | `latest` |
| `replicaCount` | Number of replicas | `1` |
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.port` | Service port | `80` |

## Uninstalling the Chart

```bash
helm uninstall my-app
```

## Upgrading the Chart

```bash
helm upgrade my-app parentappv1/parentapp
```

## Troubleshooting

### Common Issues

1. **Error: INSTALLATION FAILED: cannot re-use a name that is still in use**
   - Solution: Use a different release name or uninstall the existing release:
     ```bash
     helm uninstall release-name
     ```

2. **Repository not found**
   - Solution: Make sure you've added the repository correctly:
     ```bash
     helm repo add parentappv1 https://kamleshbelote.github.io/HelmChart/parentapp/
     helm repo update
     ```

## License

<!-- Add your license information here -->