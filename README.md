# Helm Chart for pgAdmin

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [pgAdmin](https://github.com/postgres/pgadmin4) in a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- PV provisioner support in the underlying infrastructure.
- [postgresql](https://github.com/kubernetes/charts/tree/master/stable/postgresql) instance deployed.
- TLS support requires the [cert-manager](https://github.com/jetstack/cert-manager) Kubernetes add-on to be deployed into your cluster.

## Installation

### Add Helm repository

```bash
helm repo add pgadmin https://helm.pgadmin.io
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

| Parameter                                                                   | Description                                                                                                                                                                                                                                                                                                                                     | Default                         |
| --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| **Persistence**                                                             |
| `persistence.enabled`                                                       | Enable the data persistence or not                                                                                                                                                                                                                                                                                                              | `true`                          |
| `persistence.accessMode`                     | The access mode of the volume.                                                                                                                                                                                                                                                        | `ReadWriteOnce`                 |
| `persistence.size`                           | The size of the volume.                                                                                                                                                                                                                                                               | `4Gi`                           |

## Credits

Initially inspired from https://github.com/jjcollinge/pgadmin-chart

## License

[Apache License 2.0](/LICENSE.md)



