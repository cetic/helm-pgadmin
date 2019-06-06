# Helm Chart for pgAdmin

[![CircleCI](https://circleci.com/gh/cetic/helm-pgadmin.svg?style=shield)](https://circleci.com/gh/cetic/helm-pgadmin/tree/master)

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [pgAdmin](https://github.com/postgres/pgadmin4) in a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- PV provisioner support in the underlying infrastructure.

## Installation

### Add Helm repository

```bash
helm repo add cetic https://cetic.github.io/helm-charts
helm repo update
```

### Configure the chart

The following items can be set via `--set` flag during installation or configured by editing the `values.yaml` directly (need to download the chart first).

#### Configure the way how to expose pgAdmin service:

- **Ingress**: The ingress controller must be installed in the Kubernetes cluster.
- **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster.
- **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort). You’ll be able to contact the NodePort service, from outside the cluster, by requesting `NodeIP:NodePort`.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.

#### Configure the way how to persistent data:

- **Disable**: The data does not survive the termination of a pod.
- **Persistent Volume Claim(default)**: A default `StorageClass` is needed in the Kubernetes cluster to dynamic provision the volumes. Specify another StorageClass in the `storageClass` or set `existingClaim` if you have already existing persistent volumes to use.

### Install the chart

Install the pgAdmin helm chart with a release name `my-release`:

```bash
helm install --name my-release cetic/pgadmin
```

## Uninstallation

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

## Configuration

The following table lists the configurable parameters of the pgAdmin chart and the default values.

| Parameter                                                                   | Description                                                                                                        | Default                         |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------| ------------------------------- |
| **Image**                                                                   |
| `image.repository`                                                          | pgAdmin Image name                                                                                                 | `dpage/pgadmin4`                |
| `image.tag`                                                                 | pgAdmin Image tag                                                                                                  | `4.8`                           |
| `image.pullPolicy`                                                          | pgAdmin Image pull policy                                                                                          | `IfNotPresent`                  |
| **PgAdmin**                                                                 |
| `pgadmin.username`                                                          | pgAdmin admin user                                                                                                 | `pgadmin4@pgadmin.org`          |
| `pgadmin.password`                                                          | pgAdmin admin password                                                                                             | `admin`                         |
| `pgadmin.tls`                                                               | pgAdmin admin TLS. the container will listen on port 80 for connections in plain text. If set to any value, the container will listen on port 443 for TLS connections. When TLS is enabled, a certificate and key must be provided. See [secrets file](/templates/secrets.yaml)| `false`                         |
| **Persistence**                                                             |
| `persistence.enabled`                                                       | Enable the data persistence or not                                                                                 | `true`                          |
| `persistence.existingClaim`                                                 | Provide an existing PersistentVolumeClaim, the value is evaluated as a template                                    | `nil`                           |
| `persistence.storageClass`                                                  | PVC Storage Class for PostgreSQL volume                                                                            | `nil`                           |
| `persistence.accessMode`                                                    | The access mode of the volume                                                                                      | `ReadWriteOnce`                 |
| `persistence.size`                                                          | The size of the volume                                                                                             | `4Gi`                           |
| **Service**                                                                 |
| `service.type`                                                              | Type of service for pgAdmin frontend                                                                               | `LoadBalancer`                  |
| `service.port`                                                              | Port to expose service                                                                                             | `80`                            |
| `service.tlsport`                                                           | Port to expose service in tls, `pgadmin.tls`must be enabled                                                        | `443`                           |
| `service.loadBalancerIP`                                                    | LoadBalancerIP if service type is `LoadBalancer`                                                                   | `nil`                           |
| `service.loadBalancerSourceRanges`                                          | Address that are allowed when svc is `LoadBalancer`                                                                | `[]`                            |
| `service.annotations`                                                       | Service annotations                                                                                                | `{}`                            |
| **ReadinessProbe**                                                          |
| `readinessProbe`                                                            | Rediness Probe settings                                                                                            | `{ "httpGet": { "path": "/", "port": http } "initialDelaySeconds": 60, "periodSeconds": 15, "timeoutSeconds": 10 }`|
| **LivenessProbe**                                                           |
| `livenessProbe`                                                             | Liveness Probe settings                                                                                            | `{ "httpGet": { "path": "/", "port": http } "initialDelaySeconds": 60, "periodSeconds": 30, "timeoutSeconds": 10 }` |
| **Resources**                                                               |
| `resources`                                                                 | CPU/Memory resource requests/limits                                                                                | `{}`                            |
| **Ingress**                                                                 |
| `ingress.enabled`                                                           | Enables Ingress                                                                                                    | `false`                         |
| `ingress.tls`                                                               | Ingress TLS configuration                                                                                          | `[]`                            |

## Credits

Initially inspired from https://github.com/jjcollinge/pgadmin-chart, which is archived.

## License

[Apache License 2.0](/LICENSE.md)
