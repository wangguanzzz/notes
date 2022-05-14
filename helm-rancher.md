## install specific version
helm install rancher rancher-stable/rancher --version 2.5.12   --namespace cattle-system   --set hostname=acde73a7f06f8439ab7e06e2421fc68c-1324828553.ap-east-1.elb.amazonaws.com   --set replicas=3

## search availabe version in the repo
helm search repo rancher-stable/rancher --versions


## EKS install rancher steps:
```
eksctl create cluster --name test   --version 1.20   --region ap-southeast-1 --nodegroup-name worknodes --nodes 1   --nodes-min 1   --nodes-max 1   --managed
```

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm upgrade --install \
  ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer \
  --version 3.12.0 \
  --create-namespace
```

record the load balancer
```
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

install cert manager
```
# If you have installed the CRDs manually instead of with the `--set installCRDs=true` option added to your Helm install command, you should upgrade your CRD resources before upgrading the Helm chart:
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.1/cert-manager.crds.yaml

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.5.1

## check
kubectl get pods --namespace cert-manager

```
kubectl create namespace cattle-system
helm install rancher rancher-stable/rancher --version 2.5.12   --namespace cattle-system   --set hostname=a112e53058f2c4e368d7cbc06eb036d1-2125288962.ap-east-1.elb.amazonaws.com   --set replicas=1
kubectl -n cattle-system rollout status deploy/rancher
```

