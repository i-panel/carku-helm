# Carku Helm Chart Repository

This repository contains a Helm chart which can be used to bootstrap a production-ready Carku GPS Tracking Server on a Kubernetes cluster using the Helm package manager.

### Prerequisites
- Kubernetes 1.12+
- Helm 3.1.0
- PV provisioner support in the underlying infrastructure

### Installing the Chart
To install the chart with the release name `my-carku`, run the following commands:
```
helm repo add carku https://carku.github.io/carku-helm/
helm install my-carku carku/carku
```
These commands deploy Carku server configured with a MySQL database. You can customize the deployment using a custom values file, or command-line arguments.

### Uninstalling the Chart
To uninstall/delete the `my-carku` deployment, run the following command:
```
helm del my-carku
```
The command removes all the Kubernetes components associated with the deplyoment except the PersistentVolume objects, for safety reasons.

### Parameters
The chart incorporates every configuration file setting, which can be found [ here](https://www.carku.org/configuration-file/)

### Image flavors
The [carku container image](https://github.com/carku/carku-docker) comes in _different flavors_. Currently available are
* alpine
* debian
* ubuntu
If none is specified during installation of this chart, the default is alpine based.

To set your preferred flavor, you can use `--set image.flavor=debian` for example, or, for more reusability, append the following lines to your installation's `values.yaml`
```yaml
image:
  flavor: "debian"
```
