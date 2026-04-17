# K8s for Dev

## binaries
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-amd64

chmod 750 kubectl
chmod 750 kind
```


## Create k8s cluster
```bash
KIND_EXPERIMENTAL_PROVIDER=podman \
kind create cluster --name k8s-dev --image kindest/node:v1.31.0 --config k8s-dev/config.yaml
```

## nodes
```bash
kubectl get nodes
NAME                    STATUS   ROLES           AGE   VERSION
k8s-dev-control-plane   Ready    control-plane   36s   v1.31.0
k8s-dev-worker          Ready    <none>          23s   v1.31.0
k8s-dev-worker2         Ready    <none>          23s   v1.31.0
k8s-dev-worker3         Ready    <none>          22s   v1.31.0
```

## Hello, World"
```bash
kubectl create deployment nginx-hola --image=nginxdemos/hello
kubectl expose deployment nginx-hola --type=NodePort --port=80
kubectl port-forward svc/nginx-hola 8080:80

curl -I localhost:8080
```

## top
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Start
```bash
podman start $(podman ps -a -q --filter name=k8s-dev)
```

## Stop
```bash
podman stop $(podman ps -q --filter name=k8s-dev)
```

## Destroy cluster
```bash
kind delete cluster --name k8s-dev
```
