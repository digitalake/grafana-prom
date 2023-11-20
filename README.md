# Grafana Prometheus

### Installing Grafana + Prometheus stack

If using Kubernetes, Grafana and Prometheus can be installed via Helm Charts:

- Using seperate installations
- Using [Prometheus stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack)

`Prometheus stack` Chart installs the kube-prometheus stack, a collection of Kubernetes manifests, Grafana dashboards, and Prometheus rules combined with documentation and scripts to provide easy to operate end-to-end Kubernetes cluster monitoring with Prometheus using the Prometheus Operator.

### Dependencies:
By default this chart installs additional, dependent charts:

- prometheus-community/kube-state-metrics
- prometheus-community/prometheus-node-exporter
- grafana/grafana

### Installing

Adding the Helm repo:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Installing the Helm Chart
```
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack -f [VALUES_FILE]
```

### Grafana dashboard

In this part, an namespaced application pods Grafana dashboard was created:

![Screenshot from 2023-11-20 01-14-11](https://github.com/digitalake/grafana-prom/assets/109740456/9ecc3379-bdd1-42e8-9377-afcdfb1dca1a)

### Alerting rule set up

The `alert rule` that `triggers` when the `current replicas` are `equal` to the `max replicas` specified in the Horizontal Pod Autoscaler (HPA). 

The query was used:
```
kube_horizontalpodautoscaler_spec_max_replicas{horizontalpodautoscaler="syndicate-django-demo"} - kube_horizontalpodautoscaler_status_current_replicas{horizontalpodautoscaler="syndicate-django-demo"}
```
If the result is equal to 0 (`result==0`), the Alert is being triggered.

For `testing this case`, the HPA settings were changed (memory target lowered):

![Screenshot from 2023-11-20 02-44-34](https://github.com/digitalake/grafana-prom/assets/109740456/b5d3c2f2-568e-4f7a-a0d1-195d9bd6dcf4)

So the `current replica count` is `equal` to the `max replica` count for the HPA.

Alert `pending` (1m period):

![Screenshot from 2023-11-20 02-42-41](https://github.com/digitalake/grafana-prom/assets/109740456/5e5de56f-06f0-40c6-8aca-733844d054e2)

Alert is `firing`:

![Screenshot from 2023-11-20 02-43-53](https://github.com/digitalake/grafana-prom/assets/109740456/a762dd53-c652-4505-896a-7c240357f4ba)

![Screenshot from 2023-11-20 02-44-11](https://github.com/digitalake/grafana-prom/assets/109740456/97c3fff5-463f-44e4-9b3d-86c48dbadcbd)









