# rhos-pipelines
Repo contains OpenShift Pipelines (Tekton) code

# OpenShift Pipelines Demo

## Pre-Reqs

1. Install RedHat OpenShift Pipelines Operator v1.16.0
2. Make sure pipeline.enable-api-fields is set to **beta** in Tekton config
```shell
oc edit TektonConfig config
```

## Initial Setup

```shell
# Clone repo
git clone https://github.com/jacobnelmark36/rhos-pipelines.git
cd rhos-pipelines

# OpenShift Local Setup Commands
eval $(crc podman-env)
eval $(crc oc-env)
oc get co
oc config set-context crc-developer
oc new-project pipeline-demo

# OpenShift Web Console Commands
crc console --credentials
crc console
```

## Create / Update Pipeline

```shell
oc create -f pipelines/pipeline-build-demo.yaml

# Update Pipeline
oc replace -f pipelines/pipeline-build-demo.yaml
```

## Create CI Trigger

### Create Trigger Binding
```shell
oc create -f ci/trigger/bindings/build-demo-binding.yaml
```

### Create Trigger Template
```shell
oc create -f ci/trigger/templates/build-demo-template.yaml
```

### Create Event Listener
```shell
oc create -f ci/trigger/event-listeners/build-demo-el.yaml

# Expose the Event Listener
oc get svc

oc expose svc el-build-demo-event-listener

# Get webhook URL
echo "URL: $(oc  get route el-build-demo-event-listener --template='http://{{.spec.host}}')"
```