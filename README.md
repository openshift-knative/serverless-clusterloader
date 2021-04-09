# OpenShift Serverless Control Plane Scale Tests

## Repo Layout

`config/` is for Cluster Loader configs
`content/` is for the content used by the configs


## Running Cluster Loader Tests

Make sure `oc` is logged in to the target test cluster. And ensure you
have a valid OpenShift pull secret.

From the root of this cloned git repo:

### Pull Tests Container Image

```
podman pull $(oc adm release info --image-for=tests) \
  --authfile <path to your OpenShift pull secret json file>
```

### 1000 Knative Service only

100 namespaces with 10 knative services in each.

```
podman run -v ~/.kube/config:/root/.kube/config:z \
  -v $(pwd):/root/serverless-scale/:z \
  -i $(oc adm release info --image-for=tests) \
  /bin/bash -c 'cd /root/serverless-scale && \
  KUBECONFIG=/root/.kube/config VIPERCONFIG=/root/serverless-scale/config/1000-knative-services.yaml \
  openshift-tests run-test "[Feature:Performance][Serial][Slow] Load cluster should load the cluster [Suite:openshift]"'
```

### 1000 Namespaces Test

700 namespaces with 1 config map in each, plus 300 namespaces with 10 
knative services in each.

```
podman run -v ~/.kube/config:/root/.kube/config:z \
  -v $(pwd):/root/serverless-scale/:z \
  -i $(oc adm release info --image-for=tests) \
  /bin/bash -c 'cd /root/serverless-scale && \
  KUBECONFIG=/root/.kube/config VIPERCONFIG=/root/serverless-scale/config/1000-namespaces.yaml \
  openshift-tests run-test "[Feature:Performance][Serial][Slow] Load cluster should load the cluster [Suite:openshift]"'
```

### 2500 namespaces test

```
podman run -v ~/.kube/config:/root/.kube/config:z \
  -v $(pwd):/root/serverless-scale/:z \
  -i $(oc adm release info --image-for=tests) \
  /bin/bash -c 'cd /root/serverless-scale && \
  KUBECONFIG=/root/.kube/config VIPERCONFIG=/root/serverless-scale/config/2500-namespaces.yaml \
  openshift-tests run-test "[Feature:Performance][Serial][Slow] Load cluster should load the cluster [Suite:openshift]"'
```
