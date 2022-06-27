# PEcAn

Climate change science has witnessed an explosion in the amount and types of data that can be brought to bear on the potential responses of the terrestrial carbon cycle and biodiversity to global change. Many of the most pressing questions about global change are not necessarily limited by the need to collect new data as much as by our ability to synthesize existing data. This project specifically seeks to improve this ability. Because no one measurement provides a complete picture, multiple data sources must be integrated in a sensible manner. Process-based models represent an ideal framework for integrating these data streams because they represent multiple processes at different spatial and temporal scales in ways that capture our current understanding of the causal connections across scales and among data types. Three components are required to bridge this gap between the available data and the required level of understanding: 1) a state-of-the-art ecosystem model, 2) a workflow management system to handle the numerous streams of data, and 3) a data assimilation statistical framework in order to synthesize the data with the model.

[PEcAn](https://pecanproject.github.io/) is an open source datamanagement for long tail data.

## TL;DR;

```bash
$ helm repo add ncsa https://opensource.ncsa.illinois.edu/charts/
$ helm install pecan ncsa/pecan
```

## Introduction

This chart bootstraps a [PEcAn](https://pecanproject.github.io/) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.16+
- helm 3
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release ncsa/pecan
```

The command deploys PEcAn on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation. This will also install BetyDB as well as some model runners.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

Needs to be written

The following table lists the configurable parameters of the PEcAn chart and their default values.

| Parameter                            | Description                                      | Default                                                 |
| ------------------------------------ | ------------------------------------------------ | -------------------------------------------------------|
| clustername | clustername is set to the short name that is shown in the pull down menu | demo |
| clusterfqdn | clusterfqdn is set to the name that is stored in the machines table. This should be a Fully Qualified Domain Name. Probably want to set: betydb.ingress.hostName to the same value. | pecan.localhost |
| initializeData | should be set to true to load demo data.                     | true |
| rstudioUsers | List of accounts for rstudio users, this is a list of usernames, passwords. | [ ] |
| ingress.enabled | Add ingress routes for all the components, you probably want to set `bety.ingress.enabled` to be same value. | false |
| ingress.hosts | List of host names used as part of ingress, you probably want to set clusterfqdn as one of the host names. Any Rstudio instances will use the hosts specified here, and will prefix them with the username, for example user carya, will have hsotname carya.pecan.localhost. | [ "pecan.localhost" ] |
| ingress.path | prefix added to all of the pods. |  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install my-release ncsa/pecan --set clustername=ncsa
```

The above command sets the cluster name to `ncsa`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release ncsa/pecan --values values.yaml
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Rstudio

To enable Rstudio you will need to add users to the rstudioUsers, this is a list with usernames and passwords, for example:

```yaml
rstudioUsers:
	- username: carya
	  password: illinois
	  size: 1Gi
```

This will add a Rstudio container with a dedicated 1GB of storage. This container is reachable at http://carya.pecan.localhost/

## Persistence

PEcAn uses disk storage to store the results of the workflow execution as well as any data downloads as part of the executions.

### Existing PersistentVolumeClaims

1. Create the PersistentVolume
1. Create the PersistentVolumeClaim
1. Install the chart

```bash
$ helm install my-release ncsa/pecan --set persistence.existingClaim=PVC_NAME
```

## Testing Locally

If you want to test this helm chart on your local machine, you can either leverage of [docker](https://www.docker.com/) kubernetes, or [rancher desktop](https://rancherdesktop.io/). When using rancher desktop you will need to first setup the shared storage, using `kubectl apply -f`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pecan-data
spec:
  storageClassName: manual
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/tmp/data"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pecan-data
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Gi
```

Next you will install the helm chart with the following local values: `helm upgrade --install --namespace default pecan . --values values-local.yaml`

```yaml
clusterfqdn: pecan.localhost

rstudioUsers:
  - username: carya
    password: illinois

persistence:
  existingClaim: pecan-data

ingress:
  enabled: true
  hosts:
      - pecan.localhost

betydb:
  ingress:
    enabled: true
    hosts:
      - pecan.localhost
  postgresql:
    persistence:
      storageClass: local-path

rabbitmq:
  rabbitmq:
    username: guest
    password: guest
    setUlimitNofiles: false
    ulimitNofiles: "1024"
```



## ChangeLog

### 0.6.1
- Update BETY chart
- update url forb bitnami

### 0.6.0

- Upgraded ingress routes to networking.k8s.io/v1
- Set hostnames using ingress.hosts, instead of using clusterfqdn
- Use ncsa/checks for init containers
- Fixed web pages

### 0.5.2

- Removed ED git, image does not exist anymore

### 0.5.1
- Fixes bad version numbers of PEcAn images
- Add BSD-3 License to chart

### 0.5.0
- Initial release of the PEcAn helm chart, this is still work in progress

