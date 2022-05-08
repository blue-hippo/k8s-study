# poc_cluster_04

## Goal

Build bazel remote execution environment.

Create a cluster by `config.yaml`.

```
$ kind create cluster --name=config.yaml
```

Load docker images to the nodes.

```
$ kind load docker-image --name=my-cluster --nodes=my-cluster-worker \
  bazel-buildfarm:1.13.0 bazel-client:latest
```


Run buildfarm server.

```bash
# Terminal 1
$ kubectl exec -it bazel-server -- bash

$ bazel run src/main/java/build/buildfarm:buildfarm-server $(pwd)/examples/server.config.example
```

Run buildfarm worker.

```bash
# Terminal 2
$ kubectl exec -it bazel-server -- bash

$ bazel run src/main/java/build/buildfarm:buildfarm-memory-worker $(pwd)/examples/worker.config.example
```

Run a bazel project.
```bash
# Terminal 3
$ kubectl exec -it bazel-client -- bash

$ bazel test --remote_executor=grpc://10.244.1.2:8980 //...
```

You will find following output, which means the actions are executed by the remote worker.

```bash
INFO: 51 processes: 20 internal, 31 remote.
INFO: Build completed successfully, 51 total actions
//sample1:sample1_unittest                                               PASSED in 0.1s
//sample2:sample2_unittest                                               PASSED in 0.1s
//sample3:sample3_unittest                                               PASSED in 0.1s

INFO: Build completed successfully, 51 total actions
```


```
/work
├── .bazelversion
├── sample1
├── sample2
├── sample3
└── WORKSPACE
```
