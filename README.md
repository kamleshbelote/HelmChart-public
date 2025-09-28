# ParentApp Helm Chart

## Introduction

This repository contains a Helm chart for deploying the ParentApp application on Kubernetes clusters. The chart provides a flexible and configurable way to deploy containerized applications with supported features like ingress, autoscaling, and customizable resources.

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/commonchart)](https://artifacthub.io/packages/search?repo=commonchart)

## Repository & Cloning

Clone this repository if you want to work with the chart sources locally (lint, template, package, publish, etc.).

```bash
git clone https://github.com/kamleshbelote/HelmChart-public.git

git clone git@github.com:kamleshbelote/HelmChart-public.git
```

If you previously cloned with an incorrect remote URL, fix it using:

```bash
git remote set-url origin https://github.com/kamleshbelote/HelmChart-public.git
```

## Chart Details

- **Chart Version**: 0.2.0
- **App Version**: 1.16.0
- **Type**: Application Chart

## Prerequisites

- Kubernetes 1.16+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure (if persistence is needed)

## Adding the Helm Repository

Add the published chart repository (GitHub Pages for this repository):

```bash
helm repo add parentappv1 https://kamleshbelote.github.io/HelmChart-public/parentapp/
helm repo update
```

If you see a 404 or index not found, ensure the GitHub Pages site is enabled for the `main` branch (or `gh-pages` if you publish there) and that `index.yaml` contains the `parentapp` chart. Adjust the URL if your publishing path differs.

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

The following table lists the configurable parameters of the ParentApp chart and their default values:

### Core Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Container image repository | `nginx` |
| `image.tag` | Container image tag (overrides appVersion) | `""` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `imagePullSecrets` | Image pull secrets for private registries | `[]` |

### Namespace Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `namespace.create` | Create namespace if it doesn't exist | `false` |
| `namespace.name` | Target namespace name | `demoapps` |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.ports[0].name` | HTTP service port name | `http` |
| `service.ports[0].port` | HTTP service port | `8080` |
| `service.ports[0].targetPort` | HTTP target port | `http` |
| `service.ports[0].type` | HTTP service type | `ClusterIP` |
| `service.ports[1].name` | gRPC service port name | `grpc` |
| `service.ports[1].port` | gRPC service port | `5001` |
| `service.ports[1].targetPort` | gRPC target port | `grpc` |

### Ingress Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress controller resource | `false` |
| `ingress.className` | Ingress class name | `""` |
| `ingress.annotations` | Ingress annotations | `{}` |
| `ingress.hosts` | Ingress hostnames and paths | `[{host: chart-example.local, paths: [{path: /, pathType: ImplementationSpecific}]}]` |
| `ingress.tls` | Ingress TLS configuration | `[]` |

### Autoscaling Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoscaling.enabled` | Enable Horizontal Pod Autoscaler | `false` |
| `autoscaling.minReplicas` | Minimum number of replicas | `1` |
| `autoscaling.maxReplicas` | Maximum number of replicas | `100` |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage | `80` |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory utilization percentage | `80` |

### Security Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.automount` | Auto-mount service account token | `true` |
| `serviceAccount.annotations` | Service account annotations | `{}` |
| `serviceAccount.name` | Service account name | `""` |
| `podSecurityContext` | Pod security context | `{}` |
| `securityContext` | Container security context | `{}` |

### Health Checks Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `livenessProbe.httpGet.path` | Liveness probe HTTP path | `/` |
| `livenessProbe.httpGet.port` | Liveness probe HTTP port | `http` |
| `readinessProbe.httpGet.path` | Readiness probe HTTP path | `/` |
| `readinessProbe.httpGet.port` | Readiness probe HTTP port | `http` |

### Resource Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources` | CPU/Memory resource requests/limits | `{}` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `tolerations` | Toleration labels for pod assignment | `[]` |
| `affinity` | Affinity settings for pod assignment | `{}` |

### Additional Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podAnnotations` | Pod annotations | `{}` |
| `podLabels` | Pod labels | `{}` |
| `volumes` | Additional volumes | `[]` |
| `volumeMounts` | Additional volume mounts | `[]` |
| `env` | Environment variables | `[]` |

## Uninstalling the Chart

```bash
helm uninstall my-app
```

## Upgrading the Chart

To upgrade an existing release:

```bash
# Upgrade to the latest version
helm upgrade my-app parentappv1/parentapp

# Upgrade with new values
helm upgrade my-app parentappv1/parentapp --set key=value

# Upgrade with values file
helm upgrade my-app parentappv1/parentapp -f my-values.yaml
```

## Examples

### Basic Installation

```bash
helm install my-app parentappv1/parentapp
```

### Installation with Custom Image

```bash
helm install my-app parentappv1/parentapp \
  --set image.repository=my-registry/my-app \
  --set image.tag=v1.0.0
```

### Installation with Ingress Enabled

```bash
helm install my-app parentappv1/parentapp \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=my-app.example.com \
  --set ingress.hosts[0].paths[0].path=/ \
  --set ingress.hosts[0].paths[0].pathType=Prefix
```

### Installation with Autoscaling

```bash
helm install my-app parentappv1/parentapp \
  --set autoscaling.enabled=true \
  --set autoscaling.minReplicas=2 \
  --set autoscaling.maxReplicas=10 \
  --set autoscaling.targetCPUUtilizationPercentage=70
```

### Custom Values File Example

Create a `my-values.yaml` file:

```yaml
replicaCount: 3

image:
  repository: my-registry/my-app
  tag: "v1.2.0"
  pullPolicy: Always

service:
  ports:
    - name: http
      port: 8080
      targetPort: 8080
      protocol: TCP
      type: ClusterIP

ingress:
  enabled: true
  className: "nginx"
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: my-app.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: my-app-tls
      hosts:
        - my-app.example.com

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70

env:
  - name: ENV
    value: "production"
  - name: LOG_LEVEL
    value: "info"
```

Then install with:

```bash
helm install my-app parentappv1/parentapp -f my-values.yaml
```

## Troubleshooting

### Common Issues

1. **Error: INSTALLATION FAILED: cannot re-use a name that is still in use**
   - **Solution**: Use a different release name or uninstall the existing release:
     ```bash
     helm uninstall my-app
     ```

2. **Repository not found**
   - **Solution**: Make sure you've added the repository correctly:
     ```bash
     helm repo add parentappv1 https://kamleshbelote.github.io/HelmChart-public/parentapp/
     helm repo update
     ```

3. **Error: failed to create resource: namespaces "demoapps" not found**
   - **Solution**: Either create the namespace manually or set `namespace.create=true`:
     ```bash
     kubectl create namespace demoapps
     # OR
     helm install my-app parentappv1/parentapp --set namespace.create=true
     ```

4. **Pods not starting due to image pull errors**
   - **Solution**: Check if the image exists and configure image pull secrets if needed:
     ```bash
     helm install my-app parentappv1/parentapp \
       --set image.repository=valid-registry/my-app \
       --set image.tag=valid-tag
     ```

5. **Service not accessible**
   - **Solution**: Check service configuration and ensure correct ports are exposed:
     ```bash
     kubectl get services
     kubectl describe service my-app-parentapp
     ```

### Debugging Commands

```bash
# Check release status
helm status my-app

# Get release values
helm get values my-app

# Check pod logs
kubectl logs -l app.kubernetes.io/name=parentapp

# Check pod status
kubectl get pods -l app.kubernetes.io/name=parentapp

# Describe pods for more details
kubectl describe pods -l app.kubernetes.io/name=parentapp
```

## Development

### Testing the Chart

```bash
# Lint the chart
helm lint parentapp/

# Template the chart to see generated manifests
helm template my-app parentapp/

# Dry run installation
helm install my-app parentapp/ --dry-run --debug
```

### Building and Packaging

```bash
# Package the chart
helm package parentapp/

# Update repository index
helm repo index .
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the changes locally
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.