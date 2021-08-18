
The dns localhost.s7em.com should be mapped to Kubernetes cluster IP address.

# Need to run the following kubernetes yaml files to deploy the application.

```shell
kubectl apply -f go-web-app.yaml
kubectl apply -f Ingress.yaml
kubectl apply -f cert-manager.yaml
kubectl apply -f staging-issuer.yaml
```

After few minutes, the application should be up and running.

Cert Manager deployment Process was inspired by : https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-with-cert-manager-on-digitalocean-kubernetes

# Why I've used Ingress instead of load balancer

I've implemented the solution in minikube. If the application was deployed with LB, minikube would expose the application on a random port. I need the application to be accessed with port 80 and the IP address to be mapped with DNS to verify acme challenge.

# Why helm is not created?

I don't have much knowledge about helm. I knew it was the package manager for kubernetes but I didn't have much in depth knowledge. Also I didn't get much time to gather enough knowledge to create the helm.

# Why IaC is not done ?

If I had much time I surely implement it.
