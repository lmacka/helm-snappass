# helm-snappass
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/snappass)](https://artifacthub.io/packages/search?repo=snappass)

[snappass](https://github.com/pinterest/snappass) is a clever and mostly secure way of sending secrets to someone else.
See it in action [here](https://snappass.ac3.com.au/).

## TL;DR;
```console
$ helm repo add https://lmacka.github.io/helm-snappass/
$ helm repo update
$ helm install snappass snappass/snappass
```

## Introduction

This chart bootstraps an snappass deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install -n default my-release snappass/snappass
```

The command deploys snappass on the Kubernetes cluster in the **default** namespace.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm uninstall my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Application

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Image repository | `lmacka/snappass` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `image.tag` | Image tag (defaults to appVersion) | `""` |
| `imagePullSecrets` | Image pull secrets | `[]` |
| `nameOverride` | Override chart name | `""` |
| `fullnameOverride` | Override full name | `""` |
| `gunicorn.workers` | Number of Gunicorn worker processes | `3` |
| `extraEnv` | Additional environment variables | `[]` |

### Security

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podSecurityContext.runAsNonRoot` | Run pod as non-root | `true` |
| `podSecurityContext.runAsUser` | Pod user ID | `1000` |
| `podSecurityContext.fsGroup` | Pod filesystem group | `1000` |
| `securityContext.allowPrivilegeEscalation` | Allow privilege escalation | `false` |
| `securityContext.capabilities.drop` | Dropped capabilities | `["ALL"]` |
| `securityContext.readOnlyRootFilesystem` | Read-only root filesystem | `true` |

### Service Account

| Parameter | Description | Default |
|-----------|-------------|---------|
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.annotations` | Service account annotations | `{}` |
| `serviceAccount.name` | Service account name | `""` |
| `serviceAccount.automount` | Automount service account token | `true` |

### Service

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `80` |
| `service.targetPort` | Target container port | `5000` |

### Ingress

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress | `false` |
| `ingress.className` | Ingress class name | `""` |
| `ingress.annotations` | Ingress annotations | `{}` |
| `ingress.hosts` | Ingress hosts | `[]` |
| `ingress.tls` | Ingress TLS configuration | `[]` |

### Probes

| Parameter | Description | Default |
|-----------|-------------|---------|
| `livenessProbe.httpGet.path` | Liveness probe path | `/_/_/health` |
| `livenessProbe.httpGet.port` | Liveness probe port | `http` |
| `livenessProbe.initialDelaySeconds` | Liveness initial delay | `10` |
| `livenessProbe.periodSeconds` | Liveness period | `10` |
| `livenessProbe.timeoutSeconds` | Liveness timeout | `5` |
| `readinessProbe.httpGet.path` | Readiness probe path | `/_/_/health` |
| `readinessProbe.httpGet.port` | Readiness probe port | `http` |
| `readinessProbe.initialDelaySeconds` | Readiness initial delay | `5` |
| `readinessProbe.periodSeconds` | Readiness period | `10` |
| `readinessProbe.timeoutSeconds` | Readiness timeout | `5` |

### Valkey

| Parameter | Description | Default |
|-----------|-------------|---------|
| `valkey.enabled` | Enable built-in Valkey | `true` |
| `valkey.haMode.enabled` | Enable Valkey HA mode | `false` |
| `valkey.storage.requestedSize` | Valkey storage size (empty = emptyDir) | `""` |
| `valkey.valkeyConfig` | Valkey configuration overrides | `save ""` |
| `valkey.servicePort` | Valkey service port | `6379` |
| `externalValkey.enabled` | Use external Valkey/Redis | `false` |
| `externalValkey.host` | External Valkey host | `""` |
| `externalValkey.port` | External Valkey port | `6379` |

### Init Container

| Parameter | Description | Default |
|-----------|-------------|---------|
| `initContainer.waitForValkey.enabled` | Enable Valkey wait init container | `true` |
| `initContainer.waitForValkey.image.repository` | Init container image | `busybox` |
| `initContainer.waitForValkey.image.tag` | Init container image tag | `stable` |
| `initContainer.waitForValkey.securityContext` | Init container security context | See `values.yaml` |
| `initContainer.waitForValkey.resources` | Init container resources | See `values.yaml` |

### Scheduling & Scaling

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources` | CPU/memory resource requests and limits | See `values.yaml` |
| `autoscaling.enabled` | Enable HPA | `false` |
| `autoscaling.minReplicas` | Minimum replicas | `1` |
| `autoscaling.maxReplicas` | Maximum replicas | `3` |
| `nodeSelector` | Node selector | `{}` |
| `tolerations` | Tolerations | `[]` |
| `affinity` | Affinity rules | `{}` |
| `volumeMounts` | Additional volume mounts | `[]` |
| `volumes` | Additional volumes | `[]` |

### Pod Disruption Budget

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podDisruptionBudget.enabled` | Enable PDB | `false` |
| `podDisruptionBudget.minAvailable` | Minimum available pods | `1` |
| `podDisruptionBudget.maxUnavailable` | Maximum unavailable pods | not set |
