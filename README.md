# dokc-helm-chart
Custom K8ssandra operator for Data on Kuberenetes' how-to-dok project.

## What is the custom k8ssandra-lightweight operator ?

This the kubernetes-operator which contains the following packages:
    1. Apache Cassandra
    2. Stargate 
    3. Prometheus
    4. Grafana

This package a lightweight package

## Installation and requirement

### Installation
To install this package, you can add this to your helm repositories by 

- `helm repo add dokc https://dokc.github.io/Helm-Charts/`

Once added, update the repository by running

- `helm repo update`

To check the repositories in your local setup, check if the `dokc` repo is added by running

- `helm repo list`

It must show something like this
```
NAME                    URL
cassandra               https://helm.k8ssandra.io/stable
traefik                 https://helm.traefik.io/traefik
minio                   https://helm.min.io
prometheus-community    https://prometheus-community.github.io/helm-charts
```

Once the repository is listed, you can run your k8ssandra cluster, but this would require a YAML file which would like this
_Replace: < > with values_

```yaml
 cassandra:
  version: < Cassandra Version Number, preferred version = 4.0.1 >  
  cassandraLibDirVolume:
    storageClass: local-path
    size: 5Gi
  allowMultipleNodesPerWorker: true
  heap:
   size: 1G
   newGenSize: 1G
  resources:
    requests:
      cpu: 1000m
      memory: 2Gi
    limits:
      cpu: 1000m
      memory: 2Gi
  datacenters:
  - name: < Data Center Name >
    size: < Size >
    racks:
    - name: < Rack Name >
    
kube-prometheus-stack:
  grafana:
    adminUser: < Grafana Admin User >
    adminPassword: < Grafana Password >
    
    
stargate:
  enabled: < Boolean >
  replicas: 1
  heapMB: 256
  cpuReqMillicores: 200
  cpuLimMillicores: 1000
  
```

To run the cluster, execute the following command: helm install -f <filename>.yaml <Instance Name (or) Cluster Name> dokc/k8s-lightweight

** The process should roughly take 4 minutes , wait for the pods to start up **

To check the status of the pods run:

`kubectl get pods`

```
NAME                                                   READY   STATUS    RESTARTS   AGE
prometheus-test-dep-kube-prometheus-s-prometheus-0     2/2     Running   0          2m43s
test-dep-cass-operator-6c75d6d684-vh2ts                1/1     Running   0          2m45s
test-dep-datacenter1-default-sts-0                     2/2     Running   0          2m28s
test-dep-datacenter1-stargate-699f64f646-w85kw         1/1     Running   0          2m45s
test-dep-grafana-57fd996d9-c58tf                       2/2     Running   0          2m45s
test-dep-kube-prometheus-s-operator-767fc8984c-9rlmj   1/1     Running   0          2m45s
```

Disclaimer: "It may work within 4 minutes, but it is advisable to wait for 4 minutes"


### Requirements:
 - CPU -> 4 Cores
 - Memory -> 8128M


<div class="artifacthub-widget" data-url="https://artifacthub.io/packages/helm/dokc-k8ssandra-lightweight/data-on-kuberenetes-k8ssandra-chart" data-theme="light" data-header="true" data-stars="true" data-responsive="true"><blockquote><p lang="en" dir="ltr"><b>Data on Kuberenetes K8ssandra Chart</b>: Provisions a simple, minimalistic K8ssandra stack. This includes Apache Cassandra (Database), Stargate (REST API), Prometheus (Metrics Middleware), and Grafana. (Data Visualization) </p>&mdash; Open in <a href="https://artifacthub.io/packages/helm/dokc-k8ssandra-lightweight/data-on-kuberenetes-k8ssandra-chart">Artifact Hub</a></blockquote></div>

