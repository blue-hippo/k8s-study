# poc_cluster_01

## Goal

Lean how Kubernetes Nods/Pods communicate in a Cluster.

Create a Cluster using the configulation file `config.yaml`.

```bash
$ kind create cluster --config=config.yaml
```

```bash
$ kind get clusters
my-cluster
```

```bash
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED              STATUS              PORTS                       NAMES
c1d041bbde24   kindest/node:v1.21.1   "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:33207->6443/tcp   my-cluster-control-plane
0377510738e4   kindest/node:v1.21.1   "/usr/local/bin/entr…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp          my-cluster-worker
```

There are two containers running, one is a control-plane node and another one is a worker node.
The control-plane node container has a port forwarding, `127.0.0.1:33207->6443/tcp`, for the Kubernetes API server.
The worker node container also has a port forwarding, `0.0.0.0:80->80/tcp`, as configured by `config.yaml`.
KIND's `extraPortMappings` binds `0.0.0.0` as `listenAddress` by default.

Create a Pod by applying the configuration file `pod.yaml`.
This will create a Pod, named `http-echo`, which runs `hashicorp/http-echo` container on the Kubernetes Node.

```
$ kubectl apply -f pod.yaml
```

Check created Pod, which is named `http-echo` and its status would be `Running`.
```
$ kubectl get pods
NAME   READY   STATUS    RESTARTS   AGE
foo    1/1     Running   0          30s
```

Check the response from the pod.

```bash
$ curl localhost:80
foo
```

```bash
$ kind delete cluster --name=my-cluster
```

## References
- [kind's cluster configuration (v1alpha4 apiVersion)](https://pkg.go.dev/sigs.k8s.io/kind/pkg/apis/config/v1alpha4)
- [Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
