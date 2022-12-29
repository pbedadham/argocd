# Argo CD

**Table of Contents**

- [Introduction](#introduction)
- [Kubernetes Cluster - Minikube](#kubernetes-cluster---minikube)
- [Git Repository Hierarchy](#git-repository-hierarchy)
- [Intall Argo CD Using Helm](#intall-argo-cd-using-helm)
- [Grafana App in Dev and Prod namespaces](#grafana-application-in-dev-and-prod-namespaces)
- [Reference](#reference)

# Introduction
Argo CD is a continuous delivery tool that uses Git as the source of truth for deploying applications. It follows the GitOps model, which means that all of the desired application configurations and manifests are stored in Git, and Argo CD synchronizes the desired state with the current state of the applications in the target environments. This allows you to easily track changes to your application configurations and roll back to previous versions if needed.

In addition to using Git as a source of truth, Argo CD also has a number of other features that make it a useful tool for deploying applications. For example, it has built-in support for rolling updates and canary releases, as well as the ability to automate the promotion of applications between environments. It also integrates with a number of popular cloud-native tools, such as Kubernetes and Helm, and can be used to deploy applications to a variety of environments, including on-premises and cloud-based infrastructure

# Kubernetes Cluster - Minikube
Install minikube - Follow this documentation - https://minikube.sigs.k8s.io/docs/start/
```
kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   49s   v1.25.3

kubectl get pods -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-565d847f94-4hwkl           1/1     Running   0          25s
kube-system   etcd-minikube                      1/1     Running   0          37s
kube-system   kube-apiserver-minikube            1/1     Running   0          39s
kube-system   kube-controller-manager-minikube   1/1     Running   0          39s
kube-system   kube-proxy-clwgq                   1/1     Running   0          25s
kube-system   kube-scheduler-minikube            1/1     Running   0          38s
kube-system   storage-provisioner                1/1     Running   0          36s
```

# Git Repository Hierarchy
Folder structure below is used in this project. 
```
argocd
├── argocd-install           # ArgoCD installation
│   └── argo-cd              # ArgoCD helm chart
│       ├── Chart.yaml
│       └── values.yaml
├── helm-apps                # ArgoCD Application's yaml files
│   └── grafana              # Grafana helm chart
│       ├── Chart.yaml
│       ├── values-dev.yaml  # Dev values
│       ├── values-prod.yaml # Production values
│       └── values.yaml      # common values
└── argocd-apps              # ArgoCD Application's yaml files
    └── grafana-dev.yaml
    └── grafana-prod.yaml
```

# Intall Argo CD Using Helm
```
git clone https://github.com/pbedadham/argocd.git
```

Go to argocd directory.
```
cd argocd/argocd-install/
```

Install ArgoCD in the namespace *argocd* with custom values file.
```
helm install argocd argo-cd \
    -n argocd --create-namespace \
    -f argo-cd/values.yaml
```

Get initial admin password.
```
kubectl -n argocd get secrets argocd-initial-admin-secret \
    -o jsonpath='{.data.password}' | base64 -d
```

Forward argocd-server service port 80 to localhost:8080 using kubectl.
```
kubectl -n argocd port-forward service/argocd-server 8080:80
```

Browse http://localhost:8080 and login with initial admin password.

# Grafana Application in Dev and Prod namespaces

Deploy grafana-dev and grafana-prod yaml files in the argocd-apps directory. This illustrates managing dev and prod environments separately.  
```
kubectl apply -f argocd-apps/grafana-dev.yaml
application.argoproj.io/grafana-dev-app created

kubectl get po -n dev
NAME                              READY   STATUS    RESTARTS   AGE
grafana-dev-app-d877df495-9gg4s   1/1     Running   0          12s
```

```
kubectl apply -f argocd-apps/grafana-prod.yaml
application.argoproj.io/grafana-prod-app created

kubectl get po -n prod
NAME                               READY   STATUS    RESTARTS   AGE
grafana-prod-app-d66bb8868-llwvp   1/1     Running   0          16s
```
# Reference
