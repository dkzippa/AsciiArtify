# AsciiArtify

## Demo of ArgoCD setup in Kubernetes locally using k3d


![Image](argo_demo.gif)

```
alias k=kubectl

docker ps -a
k3d --version
cat ~/.kube/config

k3d cluster create asciiartify-argo --servers 1 --agents 3
k3d cluster list
k get all -A
cat ~/.kube/config

k create namespace argocd
k get ns

k apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
k get all -n argocd
k get po -n argocd -o wide

k port-forward svc/argocd-server -n argocd 8080:443&

k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode

```

#### Go to AgroCD web interface at localhost:8080 with username `admin` and password received.


