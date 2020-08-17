## Development

An example application and guide for getting a Rails application deployed with Kubernetes. The instructions below use minikube, but they should apply to any Kubernetes cluster.

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
$ kubectl apply -f kube/postgres
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

**DB Setup:**
```
$ kubectl apply -f kube/setup.yml
$ kubectl logs -f jobs/setup
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

### Stop minikube

```
$ minikube stop
```

Delete deployments, jobs, and services with:

```
kubectl delete deployments --all
kubectl delete jobs --all
kubectl delete services --all
```

## Other resources

**Guides**
* [tzumby/rails-on-kubernetes](https://github.com/tzumby/rails-on-kubernetes) (and [article](https://www.monkeyvault.net/rails-on-kubernetes-part-2/))

**Documentation**
* https://kubernetes.io/docs/home/

**Courses**
* https://www.edx.org/course/introduction-to-kubernetes

**Tools**
* https://www.docker.com/products/docker-desktop
* https://www.docker.com/products/kubernetes
* https://k3s.io

**Misc**
** http://kubernetes-rails.com
** https://m.signalvnoise.com/seamless-branch-deploys-with-kubernetes/
** https://medium.com/containers-101/local-kubernetes-for-mac-minikube-vs-docker-desktop-f2789b3cad3a

