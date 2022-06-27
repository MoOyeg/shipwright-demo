# Repo is a sample demo for [Shipwright](https://shipwright.io/) on Openshift

## Pre-Requisites

- Tested on Openshift 4.10
- Tested with Openshift Pipelines 1.7.1
- Tested with Shipwright 0.10.0
- Requires Cluster-Admin Permissions to install Operators and add privileges

## Installation

### Install Shipwright Operator/Controller

Check [Shipwright Install Docs](https://shipwright.io/docs/getting-started/installation/) for installation instructions appropriate for your platform.

#### Example for Direct Installation

```bash
kubectl apply -f https://github.com/shipwright-io/build/releases/download/v0.10.0/release.yaml
```

### Install Sample ClusterBuild Strategies

```bash
kubectl apply -f https://github.com/shipwright-io/build/releases/download/v0.10.0/sample-strategies.yaml
```

## Demos

### Simple NodeJs Build using default ClusterBuildStrategy:

- Create required objects.Please note example add's privileged scc to pipline sa for

  ```bash
    oc apply -k ./shipwright-demo/Buildpacks-Nodejs-Simple/
  ```

- Start Build by creating BuildRun

  ```bash
    oc apply -f ./shipwright-demo/Buildpacks-Nodejs-Simple/buildrun.yaml
  ```

- We can confirm creation and status of Objects and Tekton TaskRun

  ```bash
    oc get buildrun.shipwright.io -l Build-Name=Buildpacks-Nodejs-Simple
    oc get taskrun -l buildrun.shipwright.io/name=buildpack-nodejs-buildrun-demo -n buildpacks-nodejs-simple
  ```

- We can also read logs from TaskRun Pod

  ```bash
    oc logs $(oc get pods -l buildrun.shipwright.io/name=buildpack-nodejs-buildrun-demo -o name) --all-containers
  ```

- Clean Up

  ```bash
    oc delete -f ./shipwright-demo/Buildpacks-Nodejs-Simple/buildrun.yaml
    oc delete -k ./shipwright-demo/Buildpacks-Nodejs-Simple/
  ```
