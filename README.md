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
$ helm install --name my-release ncsa/pecan
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
| enableIngress | if this is set to true all pieces of pecan will be visible on clusterfqdn. Probably want to set: betydb.ingress.enabled to the same value. | false |
| initializeData | should be set to true to load demo data.                     | true |

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

## Persistence

PEcAn uses disk storage to store the results of the workflow execution as well as any data downloads as part of the executions.

### Existing PersistentVolumeClaims

1. Create the PersistentVolume
1. Create the PersistentVolumeClaim
1. Install the chart

```bash
$ helm install my-release ncsa/pecan --set persistence.existingClaim=PVC_NAME
```

## ChangeLog

### 0.5.2
- Removed ED git, image does not exist anymore

### 0.5.1
- Fixes bad version numbers of PEcAn images
- Add BSD-3 License to chart

### 0.5.0
- Initial release of the PEcAn helm chart, this is still work in progress

