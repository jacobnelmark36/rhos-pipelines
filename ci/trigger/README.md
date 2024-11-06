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