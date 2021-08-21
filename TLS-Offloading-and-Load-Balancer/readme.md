### minikube start with vm-driver

```shell
minikube start --driver=virtualbox
```

### Enable Ingress

For minikube Need to enable Ingress Addon

```shell
minikube addons enable ingress
```

[comment]: <> (For Other platform)

[comment]: <> (```shell)

[comment]: <> (# Nginx Ingress)

[comment]: <> (helm repo add nginx-stable https://helm.nginx.com/stable)

[comment]: <> (helm install nginx-ing nginx-stable/nginx-ingress)

[comment]: <> (```)

[comment]: <> (Note: More about nginx ingress controller - https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/)

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

### Decrypt encrypted secret resource file with gpg and remove the encrypted file

```shell
gpg go-web-app-chart/templates/prod-route53-credentials-secret.yaml.gpg 
rm go-web-app-chart/templates/prod-route53-credentials-secret.yaml.gpg
```

### Install app with helm

```shell
helm install alt-f4 ./go-web-app-chart
```
After few minutes, the application should be up and running. After everything is up and running `kubectl get ingress` will show it's IP address

### DNS Mapping

The address [localhost.s7em.com](localhost.s7em.com) should be mapped to Kubernetes Ingress IP address.

# Need to run the following kubernetes yaml files to deploy the application.





Cert Manager deployment Process was inspired by : https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-with-cert-manager-on-digitalocean-kubernetes

# Why I've used Ingress instead of load balancer

I've implemented the solution in minikube. If the application was deployed with LB, minikube would expose the application on a random port. I need the application to be accessed with port 80 and the IP address to be mapped with DNS to verify acme challenge.

# Why helm is not created?

I don't have much knowledge about helm. I knew it was the package manager for kubernetes but I didn't have much in depth knowledge. Also I didn't get much time to gather enough knowledge to create the helm.

# Why IaC is not done ?

If I had much time I surely implement it.
