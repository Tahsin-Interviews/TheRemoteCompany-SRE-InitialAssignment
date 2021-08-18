### minikube start with vm-driver

```shell
minikube start --driver=virtualbox
```

In case virtualbox driver is not mentioned, minikube will start with default driver. For my mac it starts with docker driver. There's other choises like hyperkit and virtualbox.

ingress is not supported for docker driver. So we will go with virtualbox driver.

### Lets deploy our go-web-app

```shell
kubectl apply -f go-web-app.yaml
```

you can check the progress with `kubectl get all`. Everything is created under `default` namespace.

When everything is up and running you can access the service with `minikube service go-web-app-service`. As `go-web-app-service` is a `LoadBalancer` type service, a port (default: 30000-32767) from kubernetes host is mapped with the application port (8080).

### Lets now enable Ingress

```shell
minikube addons enable ingress
```

### Lets add tls secret now. 

```shell
cat tls-secret.yaml.gpg | gpg 2>/dev/null | kubectl apply -f -
```

This tls secret will be used inside ingress to configure https. To know more about kubernetes tls type secret - https://kubernetes.io/docs/concepts/configuration/secret/#tls-secrets

Note: `2>/dev/null` removes error from stdout. For more -> https://superuser.com/questions/1409312/is-it-possible-to-use-gpg-to-decrypt-a-file-to-stdout-without-parasite-info-suc
### Now apply our ingress

```shell
kubectl apply -f ingress.yaml
```

check if the ingress is ready with ` kubectl get ingress`. When ingress is ready it will show and IP address in the ADDRESS field, like this.

```shell
$ kubectl get ingress
NAME      CLASS    HOSTS                ADDRESS          PORTS     AGE
ingress   <none>   localhost.s7em.com   192.168.99.108   80, 443   102s

```

Notes:

1. [Generate TLS Certificate Online](https://punchsalad.com/ssl-certificate-generator/)