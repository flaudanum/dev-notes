<h1>Usage examples with kubectl</h1>

## Starting pods with images on DockerHub

Start the _minikube_ Kubernetes cluster with `minikube start`.

Now let's start a _pod_ with the latest NingX image from _Docker Hub_:

```
$ kubectl run nginx --image=nginx
pod/nginx created
```

The related syntax is `kubectl run <pod's name> --image=<image:version | URL>`. When using
`--image=<image:version>` it refers to _Docker Hub_ otherwise this is the URL of a private
_Docker registry_.

Check the status of running pods:

```
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          9m42s
```

with more details:

```
$ kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          15m   172.17.0.3   minikube   <none>           <none>
```

Print the description of the running pod:

```
 kubectl describe pod nginx
Name:         nginx
Namespace:    default
Priority:     0
Node:         minikube/192.168.39.196
Start Time:   Sun, 28 Nov 2021 18:05:23 +0100
Labels:       run=nginx
Annotations:  <none>
Status:       Running
IP:           172.17.0.3
...
```
