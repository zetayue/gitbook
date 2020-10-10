# Intro to NVIDIA Docker

![](../../.gitbook/assets/nvidia-docker-banner.png)

To enable portability in Docker images that leverage GPUs, NVIDIA developed [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker), an open source project that provides a command line tool to mount the user mode components of the NVIDIA driver and the GPUs into the Docker container at launch. **NVIDIA Docker is essentially a wrapper around Docker** that transparently provisions a container with the necessary components to execute code on the GPU. 

{% hint style="info" %}
On the DGX and DLS machines, the Docker installed is actually the NVIDIA Docker.
{% endhint %}

{% hint style="info" %}
NVIDIA Docker shares the **same** commands as the original Docker.
{% endhint %}

