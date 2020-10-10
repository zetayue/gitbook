# Intro to Docker



![](https://dgx-wiki.readthedocs.io/en/latest/_images/docker01.png)

Docker is an open-source project for automating the deployment of applications as portable, self-sufficient containers that can run on the cloud or on-premises. Docker is also a company that promotes and evolves this technology. Docker works in collaboration with cloud, Linux, and Windows vendors.

Official website of Docker: [https://www.docker.com/](https://www.docker.com/)

{% hint style="info" %}
On the DGX and DLS machines, we actually use the **NVIDIA Docker** to get the GPU support for deep learning researches.
{% endhint %}

### About Containers

![](https://dgx-wiki.readthedocs.io/en/latest/_images/Package.png)

A container image is a lightweight, stand-alone, executable package of a piece of software that includes everything needed to run it: code, runtime, system tools, system libraries, settings. Available for both Linux and Windows based apps, containerized software will always run the same, regardless of the environment.

Containers isolate software from its surroundings, for example differences between development and staging environments and help reduce conflicts between teams running different software on the same infrastructure.

### Comparing Containers and Virtual Machines

Containers and virtual machines have similar resource isolation and allocation benefits, but function differently because containers virtualize the operating system instead of hardware. Containers are more portable and efficient.

![](https://dgx-wiki.readthedocs.io/en/latest/_images/vs.png)

CONTAINERS

> Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs \(container images are typically tens of MBs in size\), and start almost instantly.

VIRTUAL MACHINES

> Virtual machines \(VMs\) are an abstraction of physical hardware turning one server into many servers. The hypervisor allows multiple VMs to run on a single machine. Each VM includes a full copy of an operating system, one or more apps, necessary binaries and libraries - taking up tens of GBs. VMs can also be slow to boot.

