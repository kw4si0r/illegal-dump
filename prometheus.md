# Prometheus and grafana

## Install via helm

add repo:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

run:

```
helm install monitoring prometheus-community/kube-prometheus-stack
```

## Run grafana

Run port-forward for to get grafana via localhost

```
kubectl port-forward svc/monitoring-grafana 8080:80
```

get user and password:
```
kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-user}"  | base64 -d

kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-password}"  | base64 -d
```

sample predefined dashboard:
![...](doc/img/grafana.png)