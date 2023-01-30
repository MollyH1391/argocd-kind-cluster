# Install Argocd with KIND cluster

## steps

### set up a kind cluster
```bash
kind create cluster -n  argocd-demo
kubectl create namespace argocd  
```

### install argocd and configure argocd-server service
```bach
kubectl apply -f argocd-local-install.yaml

kubectl get services
NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.96.116.181   <none>        7000/TCP,8080/TCP            39s
argocd-dex-server                         ClusterIP   10.96.36.74     <none>        5556/TCP,5557/TCP,5558/TCP   39s
argocd-metrics                            ClusterIP   10.96.57.87     <none>        8082/TCP                     39s
argocd-notifications-controller-metrics   ClusterIP   10.96.137.134   <none>        9001/TCP                     39s
argocd-redis                              ClusterIP   10.96.168.73    <none>        6379/TCP                     39s
argocd-repo-server                        ClusterIP   10.96.52.37     <none>        8081/TCP,8084/TCP            39s
argocd-server                             ClusterIP   10.96.211.140   <none>        80/TCP,443/TCP               39s
argocd-server-metrics                     ClusterIP   10.96.247.128   <none>        8083/TCP                     39s

```

### get argocd argocd-initial-admin-secret (login password)
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### access with localhost:8080
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443

Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

![argocd-login-GUI](https://github.com/MollyH1391/argocd-kind-cluster/blob/953647e1b345bf80838d66566dacf17caaea4840/argocd-local/GUI/argo_login.png)


## argocd cli
```bash
brew install argocd 
```

```bash
argocd login localhost:8080

WARNING: server certificate had error: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
WARNING: server certificate had error: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
Username: admin
Password: 
'admin:login' logged in successfully
Context 'localhost:8080' updated
peiyihuang@PeideMacBook-Air ~ % argocd cluster list
SERVER                          NAME        VERSION  STATUS   MESSAGE                                                  PROJECT
https://kubernetes.default.svc  in-cluster           Unknown  Cluster has no applications and is not being monitored.  
```