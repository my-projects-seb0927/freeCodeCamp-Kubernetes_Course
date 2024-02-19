# Kubernetes Introduction
> **Time stamp: 00:00:00**

*Done by:* Bodgan Stashchuk

## Course plan
- Terminology and key features of the Kubernets.
- Create Kubernetes cluster locally.
- Create and scale deployments.
- Build custom Docker image and create deployment using published image.
- Create services and deployments using YAML files.
- Connect different deployments together.
- Change container runtime from the Docker to CRI-0.

# What is Kubernetes
> **Time stamp: 00:02:45**

**Kubernetes is a container orchesration system**. Basically, you can create containers with Docker, but if you want to do it on different computers or different servers, you could get into troubles. Kubernetes allows you to create the containers on different servers, either physical or virtual, done automatically without intervention. You just tell Kubernetes how many containers you would like to create based on a specific image.


It is usually mentioned also as **k8s**.

Kubernetes takes care of the:
- **Automatic deployment** of the containerized applications across different servers.
- **Distribution of the load** across multiple servers.
- **Auto-scaling** of the deployed applications.
- **Monitoring** and **health check** of the containers.
- **Replacement** of the failed containers.

Nowadays, Kubernets has the next supported container runtimes:
- Docker
- Cri-0
- containerd

# What is a Pod
> **Time stamp:** 00:06:46

A pod is the smallest unit in the Kubernets World, being that containers are created inside pods. They may have one or more containes, there are shared voulmes and shared resources like shared IP address:

![image](https://github.com/my-projects-seb0927/freeCodeCamp-Kubernetes_Course/assets/83418390/4a8e027c-7f37-4279-a53e-b7c7ebe1c3a5)

This menas that all containers inside of the same pod share volumes and shares IP address. Usuarlly when the containers have to be tightened together and they heavily depend on each other and they could exist in the same namespacd, it is possible to create several containers in the same pod.

> 💡 One container per pod is the most common use case.

Remember that each pod must be located on the same server. **It is not possible to spread containers from one pod across different servers**.

> 💡 One pod - One server.








