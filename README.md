# About

Project for kubernetes <img src="https://kubernetes.io//images/favicon.png" style="height:32px"/> training purposes. 

Scope:
- [webapp](./webapp), simple GUI written in Vue <img src="https://vuejs.org/logo.svg" style="height:32px"/>, Node <img src="https://nodejs.org//static/images/favicons/favicon-32x32.png" style="height:32px"/> and mock server <img src="https://www.mock-server.com/favicon.ico" style="height:32px"/>
- [api-gateway](./api-gateway), set of ymls for NGINX Kubernetes Gateway <img src="https://avatars.githubusercontent.com/u/8629072?s=200&v=4" style="height:32px"/>deployment
- [coordinates](./coordinates), app written in Kotlin <img src="https://kotlinlang.org/assets/images/favicon.svg" style="height:32px" />, Quarkus <img src="https://raw.githubusercontent.com/github/explore/4a0bdb9141afd8d9be5d6b8d6b22eb40be88f665/topics/quarkus/quarkus.png" style="height:32px"/> and PostgreSQL <img src="https://www.postgresql.org/media/img/about/press/elephant.png" style="height:32px"/>  
- ...
- [k8](./k8) - complete set of ymls for illegal-dump project (all above projects)


As a part of training deployment will take place locally (minikube) and on AWS.

---

# Architecture

![uml](http://www.plantuml.com/plantuml/png/VL9DYzH04BtthoWM90uI44_YGLOyBA8kYeB2wA4cgMIRTBgprTMpBZB_tPhqV2Hcs3bCgkgzUlLHVHq5MLBlJEoi7v2nXveQJ4w-qnzvvsphU_82bVuH6rD0MPR6C5M2ldK4NTeGUnAAyDSO5RCDLW8_Q8F3e27Gt_fXz7xvvzj4gz-04NB4xFQAwpvih2FrH-Za6RF6mPOj2ZtWesRVtdou_GaVpnLlCfGbbNkWJlfu7mBNrgEoZyrKvv1gTa6SzI_k4tANObQQCCQKOfO4J4DW8VwVUumlokX8LQUrzfyo4c-95uNsM4uSMgnxwqV2mHw16WKt60ckZfPAVWbHMgPlNp-jhZJXJCh3xinkiepFmLi9R7shSAx5eIUvenHpkPERuPQnGOyZTh3poUuJ9O8rRSdBk4LiEimpvLZMBq2vch0xFPnyTxNVpkvLJBmHlBJM1MMsF7W3hru-Wz20C3QDhRGj2L3TaM8LUcHzGZKDBZps-mvNJ-YfrkkPbZ5JLDcdHPl3YUR10cPyHuqed5TZ5CrlVBa9KF3Ix0k9Q_9rwjq_)

---

# Kubernetes

## 1. Create cluster

```
minikube start --memory 8192 --cpus 2
```

## 2. Add entry to /etc/hosts

```
echo "$(minikube ip) minikube.local" | sudo tee -a /etc/hosts
```

test by `ping minikube.local`:
```
PING minikube.local (192.168.49.2) 56(84) bytes of data.
64 bytes from minikube.local (192.168.49.2): icmp_seq=1 ttl=64 time=0.133 ms
64 bytes from minikube.local (192.168.49.2): icmp_seq=2 ttl=64 time=0.117 ms
^C
--- minikube.local ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1005ms
rtt min/avg/max/mdev = 0.117/0.125/0.133/0.008 ms
```

## 3. Install NGINX Kubernetes Gateway

See [api-gateway](./api-gateway) instruction.


## 4. Install PostgreSQL via helm

See [PostgreSQL](./coordinates) instruction via helm


## 5. Prometheus + grafana

See [Prometheus](./prometheus.md) instruction via helm


## 6. Apply yml-s from k8 folder

```
kubectl apply -f k8/
```

and go to [http://minikube.local:30080](http://minikube.local:30080)

![..](./.github/blob/main/doc/img/screeenshots.gif)

![..](./.github/blob/main/doc/img/grafana-and-pgadmin.png)


final cluster:
```
kubectl get all --all-namespaces 
NAMESPACE       NAME                                                         READY   STATUS    RESTARTS      AGE
default         pod/alertmanager-monitoring-kube-prometheus-alertmanager-0   2/2     Running   1 (27m ago)   27m
default         pod/illegal-dump-coordinates-649d979885-nd5cj                1/1     Running   0             27m
default         pod/illegal-dump-coordinates-649d979885-tdvqs                1/1     Running   0             27m
default         pod/illegal-dump-coordinates-mock-56544fcb4-htfjp            1/1     Running   0             27m
default         pod/illegal-dump-webapp-58cb59b87c-q72bk                     1/1     Running   0             27m
default         pod/monitoring-grafana-cf9b6c6dd-nfqnc                       3/3     Running   0             28m
default         pod/monitoring-kube-prometheus-operator-7b84d5bd99-g6cvw     1/1     Running   0             28m
default         pod/monitoring-kube-state-metrics-5f4b58495-28wgm            1/1     Running   0             28m
default         pod/monitoring-prometheus-node-exporter-k5nvf                1/1     Running   0             28m
default         pod/pgadmin-7b867bd6d8-r8lfn                                 1/1     Running   0             31m
default         pod/postgresql-0                                             1/1     Running   0             32m
default         pod/prometheus-monitoring-kube-prometheus-prometheus-0       2/2     Running   0             27m
kube-system     pod/coredns-78fcd69978-h7w2g                                 1/1     Running   0             37m
kube-system     pod/etcd-minikube                                            1/1     Running   0             37m
kube-system     pod/kube-apiserver-minikube                                  1/1     Running   0             37m
kube-system     pod/kube-controller-manager-minikube                         1/1     Running   0             37m
kube-system     pod/kube-proxy-r2qhq                                         1/1     Running   0             37m
kube-system     pod/kube-scheduler-minikube                                  1/1     Running   0             37m
kube-system     pod/storage-provisioner                                      1/1     Running   1 (36m ago)   37m
nginx-gateway   pod/nginx-gateway-84c44b89db-tszvx                           2/2     Running   0             33m

NAMESPACE       NAME                                                         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
default         service/alertmanager-operated                                ClusterIP      None             <none>        9093/TCP,9094/TCP,9094/UDP     27m
default         service/illegal-dump-coordinates                             ClusterIP      10.99.123.248    <none>        8061/TCP                       27m
default         service/illegal-dump-coordinates-mock                        ClusterIP      10.102.10.207    <none>        1080/TCP                       27m
default         service/illegal-dump-webapp                                  LoadBalancer   10.104.121.147   <pending>     80:30080/TCP                   27m
default         service/kubernetes                                           ClusterIP      10.96.0.1        <none>        443/TCP                        37m
default         service/monitoring-grafana                                   ClusterIP      10.110.51.222    <none>        80/TCP                         28m
default         service/monitoring-grafana-admin                             LoadBalancer   10.101.38.237    <pending>     3000:30081/TCP                 2m49s
default         service/monitoring-kube-prometheus-alertmanager              ClusterIP      10.107.245.120   <none>        9093/TCP                       28m
default         service/monitoring-kube-prometheus-operator                  ClusterIP      10.100.173.250   <none>        443/TCP                        28m
default         service/monitoring-kube-prometheus-prometheus                ClusterIP      10.110.21.22     <none>        9090/TCP                       28m
default         service/monitoring-kube-state-metrics                        ClusterIP      10.97.123.171    <none>        8080/TCP                       28m
default         service/monitoring-prometheus-node-exporter                  ClusterIP      10.100.124.78    <none>        9100/TCP                       28m
default         service/pgadmin                                              LoadBalancer   10.97.121.1      <pending>     80:31453/TCP                   31m
default         service/postgresql                                           ClusterIP      10.111.60.180    <none>        5432/TCP                       32m
default         service/postgresql-hl                                        ClusterIP      None             <none>        5432/TCP                       32m
default         service/prometheus-operated                                  ClusterIP      None             <none>        9090/TCP                       27m
kube-system     service/kube-dns                                             ClusterIP      10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP         37m
kube-system     service/monitoring-kube-prometheus-coredns                   ClusterIP      None             <none>        9153/TCP                       28m
kube-system     service/monitoring-kube-prometheus-kube-controller-manager   ClusterIP      None             <none>        10257/TCP                      28m
kube-system     service/monitoring-kube-prometheus-kube-etcd                 ClusterIP      None             <none>        2381/TCP                       28m
kube-system     service/monitoring-kube-prometheus-kube-proxy                ClusterIP      None             <none>        10249/TCP                      28m
kube-system     service/monitoring-kube-prometheus-kube-scheduler            ClusterIP      None             <none>        10251/TCP                      28m
kube-system     service/monitoring-kube-prometheus-kubelet                   ClusterIP      None             <none>        10250/TCP,10255/TCP,4194/TCP   27m
nginx-gateway   service/nginx-gateway                                        NodePort       10.105.188.151   <none>        80:30303/TCP                   27m

NAMESPACE     NAME                                                 DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
default       daemonset.apps/monitoring-prometheus-node-exporter   1         1         1       1            1           <none>                   28m
kube-system   daemonset.apps/kube-proxy                            1         1         1       1            1           kubernetes.io/os=linux   37m

NAMESPACE       NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
default         deployment.apps/illegal-dump-coordinates              2/2     2            2           27m
default         deployment.apps/illegal-dump-coordinates-mock         1/1     1            1           27m
default         deployment.apps/illegal-dump-webapp                   1/1     1            1           27m
default         deployment.apps/monitoring-grafana                    1/1     1            1           28m
default         deployment.apps/monitoring-kube-prometheus-operator   1/1     1            1           28m
default         deployment.apps/monitoring-kube-state-metrics         1/1     1            1           28m
default         deployment.apps/pgadmin                               1/1     1            1           31m
kube-system     deployment.apps/coredns                               1/1     1            1           37m
nginx-gateway   deployment.apps/nginx-gateway                         1/1     1            1           33m

NAMESPACE       NAME                                                             DESIRED   CURRENT   READY   AGE
default         replicaset.apps/illegal-dump-coordinates-649d979885              2         2         2       27m
default         replicaset.apps/illegal-dump-coordinates-mock-56544fcb4          1         1         1       27m
default         replicaset.apps/illegal-dump-webapp-58cb59b87c                   1         1         1       27m
default         replicaset.apps/monitoring-grafana-cf9b6c6dd                     1         1         1       28m
default         replicaset.apps/monitoring-kube-prometheus-operator-7b84d5bd99   1         1         1       28m
default         replicaset.apps/monitoring-kube-state-metrics-5f4b58495          1         1         1       28m
default         replicaset.apps/pgadmin-7b867bd6d8                               1         1         1       31m
kube-system     replicaset.apps/coredns-78fcd69978                               1         1         1       37m
nginx-gateway   replicaset.apps/nginx-gateway-84c44b89db                         1         1         1       33m

NAMESPACE   NAME                                                                    READY   AGE
default     statefulset.apps/alertmanager-monitoring-kube-prometheus-alertmanager   1/1     27m
default     statefulset.apps/postgresql                                             1/1     32m
default     statefulset.apps/prometheus-monitoring-kube-prometheus-prometheus       1/1     27m


```




## 7. Additional - ingress

for minikube:
```
minikube addons enable ingress
```

more at [https://kubernetes.github.io/ingress-nginx/deploy/#minikube](https://kubernetes.github.io/ingress-nginx/deploy/#minikube)











---

---

### kubectl Cheat Sheet

- routing: `kubectl describe httproute illegal-dump-coordinates-routes`

- [https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

### Attribution
- icons: [https://www.vecteezy.com/free-vector/dump](https://www.vecteezy.com/free-vector/dump)

- map: [https://www.openstreetmap.org/](https://www.openstreetmap.org/) and [https://leafletjs.com](https://leafletjs.com)

