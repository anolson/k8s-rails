## Development

An example application and guide for getting a Rails application deployed with Kubernetes. The instruction below use minikube, but they should apply to any Kubernetes cluster.

## Getting Started

### Install minikube

```
$ brew install minikube
$ minikube start
```

And (optionally) start the dashboard with:
```
$ minikube dashboard
```

## Deploy Postgres

Create volume, service, and deployment with:
```
$kubectl apply -f kube/postgres
```

View logs with:
```
$ kubectl logs -f deployments/postgres
```

## Deploy Rails

### Build the docker image

First, we need to build our app image. One of nice things about minikube is that it allows you to build docker images without needing to push to an external registry.

https://minikube.sigs.k8s.io/docs/handbook/pushing/#1-pushing-directly-to-the-in-cluster-docker-daemon-docker-env

```
$ eval $(minikube docker-env)
$ docker build -t quotes:latest .
```

**Create the console pod with:**
```
$ kubectl apply -f kube/console-pod.yml
```

**DB Setup:**
```
$ kubectl exec -it console -- bin/rake db:setup
```

Create the service and deployment with:
```
$ kubectl apply -f kube/app
```

Open the service with:
```
$ minikube service quotes
```

## Stop services

### Stop `minikube`

```
$ minikube stop
```

Delete all services and deployments with:

```
kubectl delete deployments --all
kubectl delete services --all
```
