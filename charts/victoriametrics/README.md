## Prerequisites for the Monitoring-Stack

* Install the follow packages: ``git``, ``kubectl``, ``helm``, ``storage-class``.

* PV support on the underlying infrastructure.

# Victoria Metrics Helm Chart for Cluster Version

 ![Version: 0.11.4](https://img.shields.io/badge/ChartVersion-0.11.4-informational?style=flat-square)
  ![Version: 1.94.0](https://img.shields.io/badge/AppVersion-1.94.0-informational?style=flat-square)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/victoriametrics)](https://artifacthub.io/packages/helm/victoriametrics/victoria-logs-cluster)


Victoria Metrics Cluster version - high-performance, cost-effective and scalable TSDB, long-term remote storage for Prometheus.


## Chart Details

Note: This chart installs VictoriaMetrics cluster components such as `vmAgent`, `vminsert`, `vmselect` and `vmstorage`. It also deploy or configure metrics scraping. If you are looking for the chart to configure the monitoring stack in cluster check out [vmAgent](https://github.com/coredgeio/devops/tree/main/monitoring-automation/monitoring-stack/victoriametrics/components/vmAgent).

## Image details

|**Image**                   | **Tag** | **Registry Link** |
|:-------------------------:|:--------:| :--------:|
| **victoriametrics/vmInsert** | v1.94.0-cluster | [vmInsert](https://hub.docker.com/r/victoriametrics/vminsert) |
| **victoriametrics/vmSelect** | v1.94.0-cluster | [vmSelect](https://hub.docker.com/r/victoriametrics/vmselect) |
| **victoriametrics/vmStorage** | v1.94.0-cluster | [vmStorage](https://hub.docker.com/r/victoriametrics/vmstorage) |
| **victoriametrics/vmBackup** | enterprise-stable | [vmBackup](https://hub.docker.com/r/victoriametrics/vmbackup) |
| **victoriametrics/vmRestore** | enterprise-stable | [vmRestore](https://hub.docker.com/r/victoriametrics/vmrestore) |
| **amazon/aws-cli** | latest | [aws-cli](https://hub.docker.com/r/amazon/aws-cli) |

> **ℹ️**  In my case I used [customValues.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/victoriametrics/customValues.yaml) file instead values.yaml

Change the values according to the need of the environment in values.yaml or customValuse.yaml file like the below parameters.
`extraSecrets:name`, `extra secrets:data:credentials`

`vmSelect:horizontalPodAutoscaler:enabled`, `vmSelect:horizontalPodAutoscaler:maxReplicas`, `vmSelect:horizontalPodAutoscaler:minReplicas`, `vmSelect:resources:limits`, `vmSelect:resources:requests`

`vminsert:horizontalPodAutoscaler:enabled`, `vminsert:horizontalPodAutoscaler:maxReplicas`, `vminsert:horizontalPodAutoscaler:minReplicas`, `vminsert:resources:limits`, `vminsert:resources:requests`

`vmbackup:enabled`, `vmbackup:schedule`, `vmbackup:s3:url`

`vmstorage:retentionPeriod`, `vmstorage:persistentVolume:enabled`, `vmstorage:persistentVolume:storageClass`, `vmstorage:persistentVolume:size`, `vmstorage:persistentVolume:replicaCount`, `vmstorage:resources:limits`, `vmstorage:resources:requests`

`vmstorage:extraVolumes:name`, `vmstorage:extraVolumes:secret`, `vmstorage:extraVolumes:secretName`

> **📌** Use `extra secrets:data:credentials` and `vmstorage:extraVolumes` section in values file and put object-storage creds in base64 encoded format. Use [link](https://www.base64encode.org/) to encode or decode your details
>

> **🤷‍♂️** What we have on vmbackup cron
> 
> This cron basically helps to take backup of VictoriaMetrics storage data and put into object storage in the format of year-month->date (It will look like 2024-January/01.01.2024).
>


# vmAgent Chart details

![Version: 0.8.40](https://img.shields.io/badge/Version-0.8.40-informational?style=flat-square)

Victoria Metrics Agent - collects metrics from various sources and stores them to VictoriaMetrics

> **ℹ️**  vmAgent chart has been added with victoriametrics helm chart and all the files related to vmAgent can be find in `/devops/monitoring-automation/monitoring-stack/victoriametrics/components/vmAgent`  FOr any changes in vmAgent you can modify [Values.yaml](https://github.com/coredgeio/devops/blob/main/monitoring-automation/monitoring-stack/victoriametrics/components/vmAgent/values.yaml).
>

Change the values according to the need of the environment in values.yaml or customValuse.yaml file like the below parameters.
`remoteWriteUrls`, `resources:limits`, `resources:requests`, `replicaCount`, `config:scrape_configs`

> **📌** The value of  remoteWriteUrls will be the kubernetes service of vmInsert with API ending with /insert/0/prometheus/ (Change the respected value as per your environment).
>

> What we have on vmAgent job scrape
> In the vmAgent we have different job scrape section like below:
> - vmagent
> - kubernetes-apiservers
> - kubernetes-nodes
> - kubernetes-nodes-cadvisor
> - kubernetes-service-endpoints
> - kubernetes-service-endpoints-slow
> - kubernetes-services
> - kubernetes-pods
> - node-exporter
> - kube-state-metrics
> - cilium-metrics
> - coredns
> - kube-scheduler
> - kube-controller-manager
> - etcd



### Contributors
[![Yogendra Pratap Singh][yogendra_avatar]][yogendra_homepage]<br/>[Yogendra Pratap Singh][yogendra_homepage] 

  [yogendra_homepage]: https://github.com/PratapSingh13
  [yogendra_avatar]: https://img.cloudposse.com/75x75/https://github.com/PratapSingh13.png
