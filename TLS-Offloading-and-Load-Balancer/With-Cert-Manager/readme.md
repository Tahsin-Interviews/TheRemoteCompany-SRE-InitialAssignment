### minikube start with vm-driver

```shell
minikube start --driver=virtualbox
```

### Enable Ingress

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

Note: More about nginx ingress controller - https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/

### Install cert-manager

```shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.5.0 \
  --set installCRDs=true \
  --set 'extraArgs={--dns01-recursive-nameservers-only,--dns01-recursive-nameservers=8.8.8.8:53\,1.1.1.1:53}'
```

- [Why `--set installCRDs=true`?](https://github.com/jetstack/cert-manager/issues/3246)
- [Why `--set 'extraArgs={--dns01-recursive-nameservers-only,--dns01-recursive-nameservers=8.8.8.8:53\,1.1.1.1:53}'`?](https://stackoverflow.com/questions/60989753/cert-manager-is-failing-with-waiting-for-dns-01-challenge-propagation-could-not)

### Install letsencrypt-staging-issuer

```shell
cat prod-route53-credentials-secret.yaml.gpg | gpg 2>/dev/null | kubectl apply -f -
kubectl apply -f letsencrypt-staging-issuer.yaml
```

You can check progress with `kubectl get clusterissuer` and `kubectl describe clusterissuer`.

### Now it's the go-web-app

```shell
kubectl apply -f go-web-app.yaml
```

### When the app is ready, deploy the ingress.

```shell
 kubectl apply ingress.yaml
```

This will create ingress and also trigger a letsencrypt staging certificate request.

# How the certificate is generated ?

When cert-manager sees a request from ingress for a tls secret it creates a `certificaterequest` resource to generate a `certificate` which will later be mapped with the tls-secret described in the `ingress` resource.

You can see the status of certificaterequest with

```shell
kubectl describe certificaterequest
```

And you can see the certificate with

```shell
kubectl describe certificate
```

`certificaterequest` then creates an `order` resource which creates `challenge` resource.

```shell
kubectl describe order
kubectl describe challenge
```

This `challenge` resource varifies the identity wtih `dns01` or `http01` protocol as mentioned in the issuer.
