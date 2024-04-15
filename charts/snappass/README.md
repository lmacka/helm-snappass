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
