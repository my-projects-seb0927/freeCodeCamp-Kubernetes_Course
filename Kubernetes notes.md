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

This means that all containers inside of the same pod share volumes and shares IP address. Usuarlly when the containers have to be tightened together and they heavily depend on each other and they could exist in the same namespacd, it is possible to create several containers in the same pod.

> 💡 One container per pod is the most common use case.

Remember that each pod must be located on the same server. **It is not possible to spread containers from one pod across different servers**.

> 💡 One pod - One server.

# Kubernetes Cluster and Nodes
> **Time stamp:** 00:08:21

A kubernetes cluster constist of nodes, where nodes are servers either a metal server or virtual server. Inside of the nodes there are pods and inside of each pod there are contianers. We are in charge of setting up clusters, nodes and podes, but after they are done Kubernetes will automatically handle them.

![image](https://github.com/my-projects-seb0927/freeCodeCamp-Kubernetes_Course/assets/83418390/4c713c9f-1661-4524-a1b1-23991a729841)

## How do nodes communicate?
In all Kubernetes clusters there are tho type of nodes:
- **Master nodes:** It only exists one master node on every Kuberentes cluster. It manages worker nodes like for example distribute the work between each worker node.
  - It runs only system ports, which are responsible for actual work of the Kubernetes cluster in general.
  - It works like a kind of control plane, because it does not run client applications.
- **Worker nodes:** All boards that are related to your application are deployed on worker nodes.

# Kubernetes services
> **Time stamp:** 00:10:41

- **Kubelet:** It communicates with API servers on the master node.
- **Kube-proxy:** It is responsible for network communication inside of each node and between nodes. 
- **Container-runtime:** It runs the actual containers inside of each node.

- **API server:** It is the main point of communication between different nodes in the Kubernetes world.
- **Scheduler:** It is responsible for planning and distribution of the load between different nodes.
- **Kube Controller Manager:** It is a single point which controls everything actually in the Kubernetes cluster, controlling what happens on each of the nodes in the cluster.
- **Cloud Controller Manager:** It interacts with cloud service providers where you actually run your Kubernetes cluster.
- **etcd:** It stores all logs related to operation of the entire Kubernetes cluster.
- **DNS:** It is responsible flor names resolution in the entire Kubernetes cluster.

![image](https://github.com/my-projects-seb0927/freeCodeCamp-Kubernetes_Course/assets/83418390/38d28312-270f-43a1-941e-26afe03f4adb)

# What is kubectl? - Cluster management
> **Time stamp:** 00:14:24

kubectl, also known as "Kube controller", it is a command line tool which allows you to connect to specific Kubernetes clusters and manage them remotely. kubectl could be running even on your local computer, and using such kubectl tool you can manage a remote Kubernetes cluster, even connecting to it using REST API to the API server service on the master node using communication through HTTPS.

> 💡 Worker nodes communicates with theaster node in the same way.















