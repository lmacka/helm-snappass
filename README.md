# helm-snappass

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/snappass)](https://artifacthub.io/packages/search?repo=snappass)

A Helm chart for deploying [Pinterest's Snappass](https://github.com/pinterest/snappass) - a secure way to share passwords and secrets with a time-based expiration.  At the time of writing, pintrest don't have a regular build pipeline for their helm chart, so this is a community maintained chart based off the image build pipeline here: https://github.com/lmacka/helm-snappass

## Overview

Snappass is a secure password sharing tool that automatically expires shared secrets after a specified time. This Helm chart provides a production-ready deployment of Snappass on Kubernetes, including:

- Built-in Valkey backend (Redis-compatible, optional)
- Support for external Valkey/Redis servers
- Ingress support
- Security hardening
- Horizontal Pod Autoscaling
- Health checks and monitoring

## Prerequisites

- Kubernetes 1.21+
- Helm 3.x
- Ingress controller (optional, but recommended)

## Quick Start

  ```sh
  # Add the helm repository
  helm repo add snappass https://lmacka.github.io/helm-snappass/
  helm repo update

  # Install with default configuration
  helm install snappass snappass/snappass
  ```

## Configuration

### Minimal Configuration

For a basic installation with ingress enabled:

  ```yaml
  ingress:
    enabled: true
    className: "nginx"
    hosts:
      - host: snappass.yourdomain.com
        paths:
          - path: /
            pathType: Prefix
  ```

### Production Configuration

For a production setup with TLS and resource limits:

  ```yaml
  ingress:
    enabled: true
    className: "nginx"
    annotations:
      cert-manager.io/cluster-issuer: "letsencrypt-prod"
    hosts:
      - host: snappass.yourdomain.com
        paths:
          - path: /
            pathType: Prefix
    tls:
      - secretName: snappass-tls
        hosts:
          - snappass.yourdomain.com

  resources:
    requests:
      cpu: 100m
      memory: 128Mi
    limits:
      cpu: 200m
      memory: 256Mi

  valkey:
    enabled: true
    storage:
      requestedSize: 100Mi
    haMode:
      enabled: false  # Set to true for high availability
  ```

## Parameters

### Global Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of Snappass replicas | `1` |
| `image.repository` | Snappass image repository | `lmacka/snappass` |
| `image.tag` | Snappass image tag | `latest` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |

### Valkey Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `valkey.enabled` | Deploy Valkey as part of the release | `true` |
| `valkey.haMode.enabled` | Enable high availability mode | `false` |
| `valkey.haMode.replicas` | Number of replicas in HA mode | `3` |
| `valkey.storage.requestedSize` | Storage size for persistence | `100Mi` |
| `externalValkey.host` | External Valkey/Redis host (if valkey.enabled=false) | `""` |
| `externalValkey.port` | External Valkey/Redis port | `6379` |

### Ingress Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress | `true` |
| `ingress.className` | Ingress class name | `nginx` |
| `ingress.hosts` | Array of host configurations | `[]` |
| `ingress.tls` | TLS configuration | `[]` |

## Security Considerations

The chart implements several security best practices:

- Non-root container execution
- ReadOnly root filesystem
- Dropped capabilities
- Resource limits
- Network policies (optional)

## Monitoring

The deployment includes readiness and liveness probes configured for the Snappass service. Default probe settings can be adjusted through values.yaml.

## Uninstalling

To remove the deployment:

  ```sh
  helm uninstall snappass
  ```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.