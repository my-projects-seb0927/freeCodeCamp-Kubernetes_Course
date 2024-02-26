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

# Creating Kubernetes cluster using Minikube
> **Time stamp:** 00:29:43

It will show you the current status of minikube:
```
# minikube status
```

In order to create a cluster and start minikube:
```
# minikube start --driver=docker
```
In my case, I'm using docker so I indicate to minikube that is the driver I'm currently using.

> 💡 When Minikube finishes, you will read `Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default`. What is saying is that you don't have to do something in order to connect from kubectl to the actual minikube cluster, happening that this connection was automatically done.

# Exploring the Kubernetes node
> **Time stamp:** 00:33:50

We already know that Kubernetes run on a server, so that means that we can connect to that server through SSH protocol.

- For knowing our minikube ip:
  ```
  # minikube ip
  ```
  And with that we can see which IP address was assigned to the virtual machine that is running our Kubernetes node.


- For connecting to the server:
  ```
  # ssh docker@[IP Address]

  ### This can also work:
  # shh docker@$minikube ip
  ```
  These are the credentials for entering to the server:
  - **Username:** docker
  - **Password:** tcuser

  > 💡 If you set **`--driver=docker`** when you started minikube, you have to use `minikube ssh`.


- For listing all the running docker containers:
  ```
  # docker ps
  ```
  Since we can't manage kubernetes nodes here because kubectl is not installed on the server, we need to exit it in order to use kubectl in our machine.

  > 💡 You can also use minikube for kubectl commands, just adding `minikube` at the beggining.


- For viewing the cluster's information:
  ```
  # kubectl cluster-info
  ```
  Here we can see many aspects:
  - The IP address of the server where Kubernetes is running.
  - CoreDNS is running which means we can create Deployent, Services, etc on our Kubernetes cluster.

- Listing all the nodes available in our Kubernetes cluster:
  ```
  # kubectl get nodes
  ```
  Because Minikube only creates a single node, that's what usually there is, showing the name, status, roles, age and version.

- Listing all the pods in our Kubernetes cluster:
  ```
  # kubectl get pods
  ```
  This commands lists all the pods available inside of the **default namespace**.

- Listing all namespaces available found in default namespaces which are availables:
  ```
  # kubectl get namespaces
  ```
  **Namespaces** are used in Kubernets in order to group different resources and configuration objects. Here 

- Listing all pods running inside of the default namespace:
  ```
  # kubectl get pods --namespace=kube-system
  ```
  In this case, we are listing all the pods inside of `kube-system`

 # Creating just single Pod
 > **Time stamp:** 00:40:38

- For creating a pod:
  ```
  # kubectl run [NAME] --image=[DOCKER IMAGE NAME]
  ```

  In our case, we are going to create a pod called "nginx" which is going to have inside a docker container pulling the image of *nginx* that will be running inside of the pod:
  ```
  # kubectl run nginx --image=nginx
  ```

- Viewing the pods again will show us that there is a pod called *nginx* with the command:
  ```
  # kubectl get pods
  ```

- Viewing the details of a pod:
  ```
  # kubectl describe pod [POD'S NAME]
  ```
  - Here we can view many different details of the pod we wanted like Name, namespace, priority, node, start time, and many more information!

  - The IP address displayed on this command will not be able to connect to this particular pod using such internal IP address of the pod. **For connecting to pods, you have to create services in Kubernetes**.
  
  - There is information about the containers running inside of this pod

  - And at the end we can see the logs of this pod.

# Exploring Kubernetes Pod
> **Time stamp:** 00:46:07

- If we re-enter to the node again with `ssh docker@[IP ADDRESS]` and list again all the docker containers running that are related with nginx:
  ```
  # docker ps | grep nginx
  ```

  We may see two different containers, but those are related with nginx. The second on which has *"/pause"* is called the **Pause container**, and it is required to keep the namespace of the pod.

- In order to connect to a container, we may use the container's ID or name:
  ```
  docker exec -it [CONTAINER ID] sh
  ```
  And now you are inside of the container, you can check it with `hostname`. In our case, should be *`nginx`*, and with `hostname -i`we should see the IP address that we seaw in the details of the pod.

- Now, for connecting to the web server that is running on the nginx container, we can do it like this:
  ```
  curl [IP ADDRESS]
  ```
  And we can see the Welcome nginx page, meaning that is running with out no problem

  By the moment we have seen what we needed, we can exit now from the container.
  
- Listing all pods with more information:
  ```
  # kubectl get pods -o wide
  ```
  And with this we will not only have the information of each pod but also IP address of those particular pods in the output.c

- For deleting a pod:
  ```
  # kubectl delete pod [POD'S NAME]
  ```
  All the volumes, namespaces related to the pod are gone as well
