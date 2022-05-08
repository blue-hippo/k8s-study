# poc_cluster_03

## Goal

Understand how PODs within a NODE communicate each others.

Create a cluster from the configuration file `config.yaml`.
```
$ kind create cluster --config=config.yaml
```

Load docker images to the Nodes so that Pods can use it.
```
$ kind load docker-image --name=my-cluster --nodes=my-cluster-worker \
  bazel-nginx:latest bazel-client:latest
```

Create Pods

```
kubectl apply -f pod.yaml
```
