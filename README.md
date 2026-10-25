<html><body><h1 style="font-size:50px;color:blue;">Rajesh - CD </h1>
</body></html>


# PRODUCTION GRADE DEVSECOPS CD Pipeline (Argo CD)

## Create 1 EC2 server
- [ ] Minikube server with 4 GB storage - t2.medium

## Setup Minikube
```
$ Install Docker
$ sudo apt update && sudo apt -y install docker.io

 Install kubectl
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.23.7/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl

 Install Minikube
$ curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

 Start Minikube
$  sudo apt install conntrack (required incase --driver=none)
$  minikube start --driver=docker (bridge networking)
$  sudo minikube start --driver=none (host networking)


$  minikube stop
$  minikube delete

Install Kind on Ubuntu
$  [ "$(uname -m)" = "x86_64" ] && \
$  curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64

$  chmod +x ./kind
$  sudo mv ./kind /usr/local/bin/kind

$  kind version

$   cat > kind-3node.yaml <<'EOF'
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    nodes:
    - role: control-plane
    - role: worker
    - role: worker
    EOF

$  kind create cluster --name kind-cluster --config kind-3node.yaml

$  kind delete cluster --name kind-cluster

Port forwarding:
$  kubectl port-forward --address=0.0.0.0 pod/nginxpod 8080:80
$  kubectl port-forward --address=0.0.0.0 svc/nginxsvc 8080:80
$  nohup kubectl port-forward --address=0.0.0.0 svc/nginxsvc 8080:80 & (running in the background)
```

## Setup ArgoCD
```
$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
$ kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' 

For version 1.9 or later:
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```