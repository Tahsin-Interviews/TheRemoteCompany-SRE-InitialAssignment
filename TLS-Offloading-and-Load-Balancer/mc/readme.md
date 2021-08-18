# minikube start with vm-driver

```shell
minikube start --driver=virtualbox
```

# Enable Ingress

For minikube Need to enable Ingress Addon 
```shell
minikube addons enable ingress
```

For Other platform
```shell
# Nginx Ingress
helm repo add nginx-stable https://helm.nginx.com/stable
helm install nginx-ing nginx-stable/nginx-ingress
```

# Install cert-manger dependency

```shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.5.0 \
  --set installCRDs=true
```



# Deploy helm 

```shell
helm install frech-fries ./mc
```

#
```shell
kubectl get ingress
```

# Map Id address with the DNS and wait till the ip address is generated. To check cetificate generation status 

```shell
kubectl get certificate
kubectl describe certificate
```




