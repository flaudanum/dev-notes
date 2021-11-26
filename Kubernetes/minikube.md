<h1>minikube</h1>

## Install minikube on Fedora

[Official page of instructions for installation](https://minikube.sigs.k8s.io/docs/start/).

Download RPM package:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
```

Installation:

```
sudo rpm -Uvh minikube-latest.x86_64.rpm
```

Enable the virtualization

```
sudo su -
sudo dnf install @virtualization
sudo dnf group install --with-optional virtualization
sudo systemctl start libvirtd
sudo systemctl enable libvirtd
```

Check the installation:

```
lsmod | grep kvm
virt-host-validate
```

Add the user to the group `libvirt`

```
sudo usermod -a -G libvirt flaudanum
```

Start **minikube** with **kvm**:

```
$ minikube start --driver=kvm2
😄  minikube v1.24.0 on Fedora 35
✨  Using the kvm2 driver based on user configuration
💾  Downloading driver docker-machine-driver-kvm2:
    > docker-machine-driver-kvm2....: 65 B / 65 B [----------] 100.00% ? p/s 0s
    > docker-machine-driver-kvm2: 11.40 MiB / 11.40 MiB  100.00% 28.27 MiB p/s 
💿  Downloading VM boot image ...
    > minikube-v1.24.0.iso.sha256: 65 B / 65 B [-------------] 100.00% ? p/s 0s
    > minikube-v1.24.0.iso: 225.58 MiB / 225.58 MiB  100.00% 69.89 MiB p/s 3.4s
👍  Starting control plane node minikube in cluster minikube
💾  Downloading Kubernetes v1.22.3 preload ...
    > preloaded-images-k8s-v13-v1...: 501.73 MiB / 501.73 MiB  100.00% 99.46 Mi
🔥  Creating kvm2 VM (CPUs=2, Memory=3900MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.22.3 on Docker 20.10.8 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Set **kvm** as default driver and restart **minikube**.

```
$ minikube config set driver kvm2
❗  These changes will take effect upon a minikube delete and then a minikube start
$ minikube delete
🔥  Deleting "minikube" in kvm2 ...
💀  Removed all traces of the "minikube" cluster.
$ minikube start
😄  minikube v1.24.0 on Fedora 35
✨  Using the kvm2 driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🔥  Creating kvm2 VM (CPUs=2, Memory=3900MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.22.3 on Docker 20.10.8 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: default-storageclass, storage-provisioner
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Check **minikube**'s status:

```
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

Eventually install *Kubernetes client* which provides the CLI `kuebctl`.

```
sudo dnf install kubernetes-client
```

Check `kubectl` version

```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.0", GitCommit:"cb303e613a121a29364f75cc67d3d580833a7479", GitTreeState:"archive", BuildDate:"2021-07-22T00:00:00Z", GoVersion:"go1.16.6", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.3", GitCommit:"c92036820499fedefec0f847e2054d824aea6cd1", GitTreeState:"clean", BuildDate:"2021-10-27T18:35:25Z", GoVersion:"go1.16.9", Compiler:"gc", Platform:"linux/amd64"}
```

We are running a single node cluster:

```
$ kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   45m   v1.22.3
```

For a *Hello World* example see the
[Quickstart](https://v1-18.docs.kubernetes.io/docs/setup/learning-environment/minikube/#quickstart)
section in Kubernetes official documentation.

```
$ kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   45m   v1.22.3
$ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10
deployment.apps/hello-minikube created
$ kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
hello-minikube   1/1     1            1           52s
$ kubectl expose deployment hello-minikube --type=NodePort --port=8080
service/hello-minikube exposed
$ kubectl get pod
NAME                              READY   STATUS    RESTARTS   AGE
hello-minikube-5d9b964bfb-bnzkb   1/1     Running   0          3m51s
$ minikube service hello-minikube --url
http://192.168.39.208:31308
$ curl http://192.168.39.208:31308


Hostname: hello-minikube-5d9b964bfb-bnzkb

Pod Information:
	-no pod information available-

Server values:
	server_version=nginx: 1.13.3 - lua: 10008

Request Information:
	client_address=172.17.0.1
	method=GET
	real path=/
	query=
	request_version=1.1
	request_scheme=http
	request_uri=http://192.168.39.208:8080/

Request Headers:
	accept=*/*
	host=192.168.39.208:31308
	user-agent=curl/7.79.1

Request Body:
	-no body in request-
```

Next clean, stop and delete:

```
$ kubectl delete services hello-minikube
service "hello-minikube" deleted
$ kubectl delete deployment hello-minikube
deployment.apps "hello-minikube" deleted
$ minikube stop
✋  Stopping node "minikube"  ...
🛑  1 node stopped.
$ minikube delete
🔥  Deleting "minikube" in kvm2 ...
💀  Removed all traces of the "minikube" cluster.
```
