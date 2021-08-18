# Intro

This challenge should demonstrate that you understand the world of containers and microservices. We want to build a simple but resilient Hello World app that is written in Golang, deployed to Kubernetes as a minimal Container and accessible via a load balancer that automatically retrieves the certificates from Let's Encrypt.

There are 3 main challenges:

● Automating the build of the container

● Setting up the TLS Offloading load balancer

● Defining the whole infrastructure as code

You do not necessarily need to implement all challenges. Watch your time and try to get as far as possible in about 5-8 hours of development.

## Components

### Golang Web App Container

You can write a simple app yourself and demonstrate some skills or just get an example app from the web, e.g. [https://github.com/golang/example/blob/master/outyet/main.go](https://github.com/golang/example/blob/master/outyet/main.go). You can also use any other more complex Golang app if you want (e.g. influxdb or gogs).

It is important that the container that the app is deployed within is as small and as secure as possible.

Automate the build and upload of the container. There are various free CI services that you can (ab-)use for that. Those services can then push the app to any container registry that is publicly usable from Kubernetes.

### TLS Offloading and Load Balancer

This part will require some research. The goal is to have a Kubernetes [http://kubernetes.io/docs/user-guide/load-balancer/](http://kubernetes.io/docs/user-guide/load-balancer/) infrastructure that is automatically able to get and update Let's Encrypt certificates for any service you deploy. Find a suitable solution that you can easily spin up within your cluster. Document what made you chose that solution over other ones. You should never need to touch a Certificate and all Certificates should be updated automatically. Adding a new TLS service behind the load balancer should be just a few lines of Kubernetes yaml to be applied. In case you don't own a spare subdomain name to test you can use [freenom.com](http://freenom.com/) to create one.

Steps:

1. Research the technologies you plan to use
2. Create a simple hello world service in Golang. Automate the build and upload of the container.
3. Tear up a kubernetes cluster you can use kind/minikube locally
4. Create a Helm chart for the whole process and find a way to tear everything up/down in the correct order.
5. Document your approach, your decisions and your general notes
6. Send back links to all git repos (please use github or gitlab).

Watch your time and try to get as far as possible in about 5-8 hours of development.

### Evaluation criterias

What we look for:

- Clean project setup and documentation
- Ability to dive into a new topic, extract the important points and then implement it
- Everything is automated as much as possible
- The Web App container is small and secure
- The infrastructure is resilient
