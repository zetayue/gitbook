# Docker Useful Commands

This part contains a collection of basic and useful Docker commands in daily using. A full list of Docker commands can be found at [here](https://docs.docker.com/engine/reference/commandline/docker/). For the Docker commands for [NVIDIA DGX Cloud Service](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/docker-tutorial/use-docker-on-nvidia-gpu-cloud) usage can be found at [1](http://docs.nvidia.com/dgx/dgx-registry-user-guide/index.html#using-dgx-registry-from-docker-command-line) and [2](http://docs.nvidia.com/ngc/ngc-user-guide/index.html).

Some common use commands are listed below:

### 1. List images on the system

```text
docker images
```

It will return the Docker images on the system \(below is an example\):

```text
REPOSITORY                  TAG                            IMAGE ID            CREATED             SIZE
nvcr.io/nvidia/theano       18.01                          dc2fda091dd4        5 weeks ago         4GB
nvcr.io/nvidia/mxnet        18.01-py3                      1a70c143e041        5 weeks ago         2.67GB
nvcr.io/nvidia/mxnet        18.01-py2                      ffe7de2f34c3        5 weeks ago         2.64GB
nvcr.io/nvidia/pytorch      17.12                          5ac6ff8f9a81        2 months ago        4.52GB
nvcr.io/nvidia/tensorflow   17.12                          19afd620fc8e        2 months ago        2.88GB
nvcr.io/nvidia/caffe2       17.12                          ac17621b6d70        2 months ago        2.8GB
nvcr.io/nvidia/caffe        17.12                          cd94fcaaec3f        2 months ago        3.25GB
```

### 2. List running containers

```text
docker ps
```

It will return the Docker containers running by all users now on the system \(below is an example\):

```text
CONTAINER ID        IMAGE                                 COMMAND                  CREATED             STATUS              PORTS                          NAMES
9172d62a0f3f        nvcr.io/nvidia/pytorch:19.07-py3      "/usr/local/bin/nvid…"   4 days ago          Up 4 days                                          test_attn
2f48d36a73c0        dls/tensorflow:v2.1-cuda10.1-new2     "/usr/local/bin/nvid…"   12 days ago         Up 12 days          6006/tcp, 6064/tcp, 8888/tcp   clrn_run
d09e539854a9        dls/tensorflow:v2.1-cuda10.1          "/usr/local/bin/nvid…"   13 days ago         Up 13 days          6006/tcp, 6064/tcp, 8888/tcp   multi-1d-GRU
c7d73d8dbc0e        dls/tensorflow:v2.1-cuda10.1          "/usr/local/bin/nvid…"   13 days ago         Up 13 days          6006/tcp, 6064/tcp, 8888/tcp   music-lstm
2506b4262924        nvcr.io/hunter/pyg1.4.2:latest        "/usr/local/bin/nvid…"   3 weeks ago         Up 3 weeks                                         pyg1
2b62b12570e2        hlim_tensorflow3                      "/usr/local/bin/nvid…"   2 months ago        Up 2 months         6006/tcp, 6064/tcp, 8888/tcp   interesting_visvesvaraya
6fd51c5709ba        quay.io/coreos/etcd:v3.2.26           "/usr/local/bin/etcd"    2 months ago        Up 2 months                                        etcd1
76f5d2b3329b        nvcr.io/nvidia/pytorch:19.07-py3      "/usr/local/bin/nvid…"   3 months ago        Up 2 days                                          drug_attn
288cf9489f6b        nvcr.io/nvidia/tensorflow:19.08-py3   "/usr/local/bin/nvid…"   7 months ago        Up 2 months         6006/tcp, 6064/tcp, 8888/tcp   hlim_tensorflow2
6615744e4934        nvcr.io/nvidia/tensorflow:19.08-py3   "/usr/local/bin/nvid…"   7 months ago        Up 2 months         6006/tcp, 6064/tcp, 8888/tcp   hlim_tensorflow1
```

The `CONTAINER ID` can be used to identify your container in other commands. 

### 3. Run a container

```text
nvidia-docker run --name containerName -it --network=host --rm -v local_dir:container_dir repository:tag
```

### 4. Exit a container

Exit a container and close it, then container cannot be found via `docker ps`:

```text
exit
```

Exit a container **without** closing it, container can be found via `docker ps`:

```text
ctrl+P+Q
```

### 5. Resume a container

To resume a container after exiting with “ctrl+P+Q”:

```text
docker exec -it containerID /bin/bash
```

or

```text
docker attach containerID
```

{% hint style="warning" %}
Using the second one will sometimes get stuck and have no response for a long time. The first one is preferred.
{% endhint %}

### 6. Kill a container

```text
docker kill containerID
```

### 7. Save a customized container

Sometimes if you have done some changes \(e.g. install new packages or drivers\) in a container, it is better to save the modified container as a new container image before exiting it. Then next time you can directly run your saved image to resume the container environment.

```text
docker commit containerID repository:tag
```

The `repository` and `tag` are your customized ones for the next time using. 

### 8. Upload a customized container to NGC

Sometimes a customized container image is built and saved on one HPC, and you want to use the image on another machine. Then it is better to upload the customized container image to our group's cloud space in NGC. Then the image can be pulled from our cloud space on the other machine. 

Please contact [szhang4@gradcenter.cuny.edu](mailto:szhang4@gradcenter.cuny.edu) if you want to do so.

