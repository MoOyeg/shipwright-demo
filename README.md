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

### Simple NodeJs Build using default BuildPackv3 ClusterBuildStrategy:

- Create required objects.Please note example add's privileged scc to pipline sa for Build and default sa for deploy

  ```bash
  oc apply -k ./shipwright-demo/Buildpacks-Nodejs-Simple/
  ```

- Start Build by creating BuildRun

  ```bash
  oc apply -f ./shipwright-demo/Buildpacks-Nodejs-Simple/buildrun.yaml
  ```

- We can confirm creation and status of Objects and Tekton TaskRun

  ```bash
  oc get buildrun.shipwright.io -l Build-Name=Buildpacks-Nodejs-Simple -n buildpacks-nodejs-simple

  oc get taskrun -l build.shipwright.io/name=s2i-python-simple-build -n buildpacks-nodejs-simple
  ```

- We can also read logs from TaskRun Pod

  ```bash
  oc logs -n buildpacks-nodejs-simple $(oc get pods -l buildrun.shipwright.io/name=buildpack-nodejs-buildrun-demo -o name -n buildpacks-nodejs-simple) --all-containers
  ```

- Optional: Deploy App Sample(Requires Privileged SCC)

  ```bash
  oc apply -k ./shipwright-demo/Buildpacks-Nodejs-Simple/deploy
  ```

- Optional: Test App Sample

  ```bash
  curl -k -v $(oc get route -l Build-Name=Buildpacks-Nodejs-Simple -o jsonpath='{.items[0].spec.host}')
  ```

- Clean Up

  ```bash
  oc delete -f ./shipwright-demo/Buildpacks-Nodejs-Simple/buildrun.yaml

  oc delete -k ./shipwright-demo/Buildpacks-Nodejs-Simple/deploy

  oc delete -k ./shipwright-demo/Buildpacks-Nodejs-Simple/

  ```

### Simple Python Build using default SourceToImage ClusterBuildStrategy

- Create required objects.Please note example add's privileged scc to pipline sa for Build and default sa for deploy

  ```bash
  oc apply -k ./shipwright-demo/S2i-Python-Simple/
  ```

- Start Build by creating BuildRun

  ```bash
  oc apply -f ./shipwright-demo/S2i-Python-Simple/buildrun.yaml
  ```

- We can confirm creation and status of Objects and Tekton TaskRun

  ```bash
  oc get buildrun.shipwright.io -l Build-Name=S2i-Python-Simple -n s2i-python-simple

  oc get taskrun -l build.shipwright.io/name=s2i-python-simple-build -n s2i-python-simple
  ```

- We can also read logs from TaskRun Pod

  ```bash
  oc logs -n s2i-python-simple $(oc get pods -l build.shipwright.io/name=s2i-python-simple-build -o name -n s2i-python-simple) --all-containers 
  ```

- Optional: Deploy App Sample

  ```bash
  oc apply -k ./shipwright-demo/S2i-Python-Simple/deploy -n s2i-python-simple
  ```

- Optional: Test App Sample

  ```bash
  curl -k -v $(oc get route -n s2i-python-simple -l Build-Name=S2i-Python-Simple -o jsonpath='{.items[0].spec.host}')
  ```

- Clean Up

  ```bash
  oc delete -f ./shipwright-demo/S2i-Python-Simple/buildrun.yaml

  oc delete -k ./shipwright-demo/S2i-Python-Simple/deploy

  oc delete -k ./shipwright-demo/S2i-Python-Simple

  ```
