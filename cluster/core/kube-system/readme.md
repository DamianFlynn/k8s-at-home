## kube-vip

TODO:


## reloader

[reloader](https://github.com/stakater/Reloader): A Kubernetes controller to watch changes in ConfigMap and Secrets and do rolling upgrades on Pods with their associated Deployment, StatefulSet, DaemonSet and DeploymentConfig

* [reloader/](reloader/helm-release.yaml)

## node-feature-discovery

Using the USB feature of [node-feature-discovery](https://github.com/kubernetes-sigs/node-feature-discovery) to dynamically label nodes that contain specific USB devices we care about

* [node-feature-discovery](node-feature-discovery/node-feature-discovery.yaml)

## descheduler

Leveraging [descheduler](https://github.com/kubernetes-sigs/descheduler) to automatically evict pods that no longer satisfy their NodeAffinity constraints.  This is used to work in concert with `node-feature-discovery` such that when USB devices are moved from one node to a different node, the pods requiring the USB devices will be properly forced to reschedule to the new location

* [descheduler/descheduler.yaml](descheduler/descheduler.yaml)

## metrics-server

Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through Metrics API for use by Horizontal Pod Autoscaler and Vertical Pod Autoscaler. Metrics API can also be accessed by kubectl top, making it easier to debug autoscaling pipelines.

* [metrics-server/helm-release.yaml](metrics-server/helm-release.yaml)

## reflector

Reflector is a Kubernetes addon designed to monitor changes to resources (secrets and configmaps) and reflect changes to mirror resources in the same or other namespaces.

https://artifacthub.io/packages/helm/emberstack/reflector

* [reflector/helm-release.yaml](reflector/helm-release.yaml)
