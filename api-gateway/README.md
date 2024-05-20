## Install NGINX Kubernetes Gateway

See and follow up current installation process at [nginx-kubernetes-gateway official site](https://github.com/nginxinc/nginx-kubernetes-gateway/blob/main/docs/installation.md)

at the end check if pods are running by 
```
> kubectl get pods -n nginx-gateway
```

it should display something like 
```
NAME                             READY   STATUS    RESTARTS   AGE
nginx-gateway-84c44b89db-872bp   2/2     Running   0          36s

```


## Troubleshooting

### pod nginx-gateway-* CrashLoopBackOff

sometimes after minikube restart problem with nginx-gateway pod may occure

```
> kubectl get pods --all-namespaces 

NAMESPACE       NAME                               READY   STATUS                  RESTARTS        AGE
nginx-gateway   nginx-gateway-84c44b89db-bjsq6     0/2     Init:CrashLoopBackOff   2 (24s ago)     6h16m
```

just kill this pod, new one healthy will be created
```
> kubectl delete pod nginx-gateway-84c44b89db-bjsq6 -n nginx-gateway

> kubectl get pods --all-namespaces 

NAMESPACE       NAME                               READY   STATUS    RESTARTS        AGE
nginx-gateway   nginx-gateway-84c44b89db-hgvk6     2/2     Running   0               30s
```

## Create NodePort for local testing

See [nodeport.yaml](k8/nodeport.yaml)


## Sample tea/coffee (from https://github.com/nginxinc)

```
> cd k8
> kubectl apply -f .
> kubectl get all --all-namespaces 

NAMESPACE       NAME                                   READY   STATUS    RESTARTS      AGE
default         pod/coffee-6f4b79b975-8grs4            1/1     Running   0             11m
default         pod/tea-6fb46d899f-mvwlh               1/1     Running   0             11m
nginx-gateway   pod/nginx-gateway-84c44b89db-jngf5     2/2     Running   0             19h

NAMESPACE       NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
default         service/coffee          ClusterIP   10.107.116.205   <none>        80/TCP                   11m
default         service/tea             ClusterIP   10.102.39.25     <none>        80/TCP                   11m
nginx-gateway   service/nginx-gateway   NodePort    10.98.195.215    <none>        80:30303/TCP             2d3h

NAMESPACE       NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
default         deployment.apps/coffee          1/1     1            1           11m
default         deployment.apps/tea             1/1     1            1           11m
nginx-gateway   deployment.apps/nginx-gateway   1/1     1            1           2d3h

NAMESPACE       NAME                                       DESIRED   CURRENT   READY   AGE
default         replicaset.apps/coffee-6f4b79b975          1         1         1       11m
default         replicaset.apps/tea-6fb46d899f             1         1         1       11m
nginx-gateway   replicaset.apps/nginx-gateway-84c44b89db   1         1         1       2d3h
```

NodePort service is running (service/nginx-gateway 80:30303) to You can test it:
```
> curl http://minikube.local:30303/tea

Server address: 172.17.0.2:8080
Server name: tea-6fb46d899f-mvwlh
Date: 05/Jan/2023:14:11:56 +0000
URI: /tea
Request ID: c4b742fb25ac8ce810a431e6ea2f4bce


> curl http://minikube.local:30303/coffee

Server address: 172.17.0.5:8080
Server name: coffee-6f4b79b975-8grs4
Date: 05/Jan/2023:14:42:55 +0000
URI: /coffee
Request ID: 85c73775479aef38cd2b553873ccbaa2
```

