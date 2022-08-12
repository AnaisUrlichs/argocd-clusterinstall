## First Install after creating a Kubernetes cluster

This repository is to showcase how to first get your cluster up and running with the following tools:
1. Prometheus to access metrics in your cluster
2. Grafana to visualise metrics in your cluster
3. Trivy to generate vulnerability and other security scans in your cluster
4. Teleport to access your Kubernetes cluster from anywhere.

## Install ArgoCD

The documentation < Getting Started Guide shows how to install ArgoCD.

You should have the following in your namespace after following the guide:
```
❯ kubectl get all -n argocd
NAME                                                    READY   STATUS    RESTARTS   AGE
pod/argocd-application-controller-0                     1/1     Running   0          76s
pod/argocd-applicationset-controller-86c8556b6d-2cffw   1/1     Running   0          76s
pod/argocd-dex-server-5c65569f55-s9k9s                  1/1     Running   0          76s
pod/argocd-notifications-controller-f5d57bc55-bgnd8     1/1     Running   0          76s
pod/argocd-redis-65596bf87-b5xqr                        1/1     Running   0          76s
pod/argocd-repo-server-5bfd7c4cfd-nh2n2                 1/1     Running   0          76s
pod/argocd-server-8544dd9f89-r9c58                      1/1     Running   0          76s
```

## Apply resources to your cluster

Before you go ahead and install all the resource, you need to replace the following two fields inside the teleport.yaml manifest:
```
clusterName: <yourpreferred domain name>
acmeEmail: <youremail>
```

Next, you can apply all the resources and ArgoCD will learn about them.

```
❯ kubectl apply -f ./argocd-resources 
application.argoproj.io/crds created
application.argoproj.io/prometheus created
application.argoproj.io/teleport-cluster created
application.argoproj.io/trivy-operator created
```

This will install prometheus and Teleport. However, since Trivy is sending metrics to Prometheus in this case, it will be dependent on Prometheus first running.

You can go over into the UI and do a manual sync on Trivy or you can run the following command:
```
argocd app list 
```

and then sync Trivy
```
argocd app sync trivy-operator --prune
```

