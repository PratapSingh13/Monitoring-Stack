<!--- app-name: Monitoring Stack -->

# Monitoring Stack

A monitoring stack for Kubernetes and Cloud, includes a set of tools and components designed to observe, collect, and analyze metrics, logs, and traces from any Kubernetest cluster and Cloud environment. The primary goal is to gain insights into the health, performance, and behavior of the applications and infrastructure 

This chart bootstraps a Monitoring Stack deployment on a K8s cluster.

## Components

The stack includes following compoenents: <em>(to be updated )</em>
- Grafana : v10.1.4
- Victoriametrics : v1.94.0
- VM Agent : v1.91.2
- Loki: v2.8.11
- Promtail: v2.9.3
- Node Exporter
- Kube State Metrics

## Prerequisites

- Kubernetes 1.23+
- Helm 3.8.0+
- Storage Class configured

Also create required configmaps to enabled monitoring for `etcd`, `kube-controller` and `kube-scheduler` :

<em>Run below as root/admin user</em>

```console
sudo su
export KUBECONFIG=/etc/kubernetes/admin.conf
kubectl create ns monitoring
kubectl create configmap etcd-certs --from-file=/etc/kubernetes/pki/etcd/server.crt --from-file=/etc/kubernetes/pki/etcd/server.key --from-file=/etc/kubernetes/pki/etcd/ca.crt -n monitoring

kubectl create configmap kubelet-certs --from-file=/etc/kubernetes/pki/apiserver-kubelet-client.crt --from-file=/etc/kubernetes/pki/apiserver-kubelet-client.key --from-file=/etc/kubernetes/pki/ca.crt --namespace=monitoring
```

Follow this link for configuring endpoints of k8s-component pod yaml for kube-scheduler & kube-controller
LINK : https://coredgeio.atlassian.net/wiki/spaces/~62a9798e6085950068ae4ba3/pages/256442628/Steps+to+enable+k8s-components+kube-scheduler+kube-controller+metrics

## Installing the Chart

To install the chart with the release name `monitoring-stack`:

```console
git clone https://github.com/coredgeio/devops.git
cd devops/monitoring-automation/monitoring-stack/
helm install monitoring-stack monitoring-stack-helm -n monitoring --create-namespace
```

These commands deploy the monitoring stack on the Kubernetes cluster in the default configuration.

## Uninstalling the Chart

To uninstall/delete the `monitoring-stack` deployment:

```console
helm delete monitoring-stack -n monitoring
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Details of Chart
Here in this coredge monitoring stack we are offering below components:
 - [Grafana](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/grafana)
 - [victoriametrics](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/victoriametrics)
 - [vmAgent](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/vmAgent)
 - [prometheus-node-exporter](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/prometheus-node-exporter)
 - [kube-state-metrics](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/kube-state-metrics)
 - [prometheus](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/prometheus)
 - [loki](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/loki)
 - [promtail](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/promtail)

All the above components can be manged by their `values.yaml` itself which is hosted on there individual chart.

The structure of the each component will be looks like below.

### Grafana

![](../../..//images/grafana_tree.png)

> **📌** If user want to make any changes on **Grafana** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/grafana/values.yaml).
>


### VictoriaMetrics Cluster

![](../../..//images/victoriametrics.png)

> **📌** If user want to make any changes on **VictoriaMetrics** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/victoriametrics/values.yaml).
>

### vmAgent

![](../../..//images/vmagent.png)

> **📌** If user want to make any changes on **vmAgent** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/vmAgent/values.yaml).
>

### Loki

![](../../..//images/loki.png)

> **📌** If user want to make any changes on **loki** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/loki/values.yaml).
>

### Promtail

![](../../..//images/promtail.png)

> **📌** If user want to make any changes on **promtail** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/promtail/values.yaml).
>

### Node-Exporter

![](../../..//images/node-exporter.png)

> **📌** If user want to make any changes on **Node-Exporter** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/prometheus-node-exporter/values.yaml).
>


### Kube-State-Metrics

![](../../..//images/kube-state.png)

> **📌** If user want to make any changes on **kube-state-metrics** chart configuration then they can edit [values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/monitoring-stack-helm/charts/kube-state-metrics/values.yaml).
>


## How to manage any component deployment in our stack
For this we have added a functionality in chart main `values.yaml` there we can enable/disable the component deployment.

```yaml

grafana:
  enabled: true
  fullnameOverride: "grafana"
  adminUser: admin
  adminPassword: admin
  service:
    type: NodePort
  ingress:
    enabled: false
    ingressClassName: ""
    host: ""
    tlsSecret: ""
  persistence:
    size: 10Gi
  sidecar:
    alerts:
      enabled: false
    dashboards:
      enabled: true
    datasources:  
      enabled: true

victoria-metrics-cluster:
  enabled: true
  vmstorage:
    persistentVolume:
      size: 10Gi

victoria-metrics-agent:
  enabled: true

kube-state-metrics:
  enabled: true
  fullnameOverride: "kube-state-metrics"

prometheus-node-exporter:
  enabled: true
  fullnameOverride: "node-exporter"

loki:
  enabled: true
  fullnameOverride: "loki"

promtail:
  enabled: true
  config:
    clients:
      - url: "http://loki.monitoring.svc:3100/loki/api/v1/push"
  fullnameOverride: "promtail"

prometheus:
  enabled: false
  server:
    name: prometheus-server
    service:
      type: ClusterIP
    global:
      scrape_interval: 15s
    persistentVolume:
      size: 10Gi
```


### Contributors
[![Yogendra Pratap Singh][yogendra_avatar]][yogendra_homepage]<br/>[Yogendra Pratap Singh][yogendra_homepage] 

  [yogendra_homepage]: https://github.com/PratapSingh13
  [yogendra_avatar]: https://img.cloudposse.com/75x75/https://github.com/PratapSingh13.png