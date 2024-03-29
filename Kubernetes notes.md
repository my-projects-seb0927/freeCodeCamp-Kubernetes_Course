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


# Creating alias for the kubectl command
> **Time stamp:** 00:53:00

`Kubectl` is a relatively long command, so we can create alias for a command in order to save some time.

On linux you can do it easily:
```
# alias k="kubectl"
```
So now I don't have to type `kubectl` but `k`. But this is not permanent, because then you have to edit your shell configuration profile.

On Windows - Powershell is not possible to do it but you can do it through the git bash.

# Creating and exploring Deployment
> **Time stamp:** 00:55:20

Remember that it is not convenient to create separate pods inside of the Kubernetes cluster because you are not able to scale and increase quantity of the pods.

So the most common way to create multiple pods is by using deployments, which will be responsible for creation of the actual pods

- Creating a deployment:
  ```
  # kubectl create deployment [NAME] --image=[IMAGE NAME]
  ```
  So if we want to create a deployment for *nginx*:
  ```
  # kubectl create deployment nginx-deployment --image=nginx
  ```

- Listing all deployments:
  ```
  # kubectl get deployments
  ```
  If we list again all podes with `kubectl get pods` we will see that there is a pod created automatically **managed by the deployment**, not by us.

- Listing the description of a deployment
  ```
  # kubectl describe [DEPLOYMENT NAME]
  ```
  With this command, we can see the name, namespace, creation, labels, annotations, selector, replicas, strategy type,  and many more information of the deployment

  - Deployment are co:nnected with pods through the label, because you will see that they have the same labels.
  - At the end you may see the Events of the deployment (Something like the logs).
  - **ReplicaSet:** ReplicaSet manages all pods related to deployment, being a set of replicas of the application.
    - Usually, the pods managed by a replica set will have a name like this: `nginx-deployment-84cd76b964-tw79f`. Being the last hash an ID.
    - If there are multiple pods in the same deployment which belongs to the same replica set, you'see the same names but with different hashes for different pods.

- Scaling a deployment for increasing quantity of the pods inside of the deployment:
  ```
  # kubectl scale deployment [DEPLOYMENT NAME] --replicas=[AMOUNT]
  ```

  So if we wanted five replicas, we can do it like this:
  ```
  # kubectl scale deployment nginx-deployment --replica=3
  ```

  - Even though the pods are the same, if you do `kubectl get pods -o wide`, you will see that each one of them has a different IP address.
  - They are created just on a single node, but if there were more nodes, the load would've been distributed on different nodes

# Conncecting to one of the Pods using its IP address
> **Time stamp:** 01:07:02

This is just the same process as the **Exploring Kubernetes Pod** chapter.

**Recomendations:**
- Connecting to the pods is possible from the node, but these IP addresses are assigned dynamically when the pods are created.
  - It's recommended to not rely on these IP addresses because of that.
- There is a service for connecting to the pods taking in account what said before.

# What is a Service
> **Time stamp:** 01:09:24

If you want to connect to specific deployments using an specific IP address you have to create **services**. There are many options:

- Create a ClusterIP and that IP address will be created and assigned to specific deployment and you will be able to connect to that specific deployment all inside of the cluster using a kind of **virtual IP address**.
  - Take a note, it will be just a single IP address for the entire deployment.
- There was an option to create external IP addresses to open deployment to outside world. It is possible to expose specific deployment to the IP address of the node or use a load balancer.
  - **The most common solution:** To have a load balancer IP address, which will be just single for the entire Kubernetes cluster for a specific deployment.
    -  It means that you will be able to connect to your deployment no matter where pods are created using just a single external IP address

# Creating and exploring ClusterIP service
> **Time stamp:** 01:11:26

- Read the details about size deployment:
  ```
  # kubectl get deployments
  ```

  So what is next is to create a service for exposing a specific port from the deployment.

  Since we hace nginx running on each pod, and we know that nginx web server runs at Port 80, it means that we have to expose internal port 80 from the containers to any other prod outside of the deployment.

- Exposing the internal port of the containers from a deployment:
  ```
  # kubectl expose deployment [DEPLOYMENT NAME] --port:[EXTERNAL PORT] --target-port:[INTERNAL CONTAINER PORT]
  ```

  So in our case of our nginx example:
  ```
  # kubectl expose deployment nginx-deployment --port=8080 --target-port=80
  ```

  And that means that the port exposed from the ngninx-deployment will be exposed in our machine through the port 8080.

  If we list the services again (`kubectl get services`), there will be a new service called ***ngninx-deployment***, where it will have a virtual IP address that Kubernetes created for it for exposing the port. **This cluster IP address will be only available only inside of the cluster**

# Conncecting to the Deployment using ClusterIP service
> **Time stamp:** 01:16:40

If we grab the *nginx-deployment* Cluster-IP and try from our local machine:
```
# curl [NGINX CLUSTER-IP]:8080
```

**It will not work** because this IP is only available inside the cluster, so you have to `minikube ssh` and if you try to connect again you will be capable of connecting to the deployment.

# Deleting Deployment and Service
> **Time stamp::** 01:21:21

For deleting deployments and services:
```
# kubectl delete deployment [DEPLOYMENT NAME]
# kubectl delte service [SERVICE NAME]
```

# Creating Node web application
> **Time stamp:** 01:22:29

So, here he creates a NodeJs application. With this application he will show how to use Kubernetes.

# Dockerizing Node application
> **Time stamp:** 01:30:09

Now, with our Node web application, the next step is to dockerize it!

```
FROM node:alpine

WORKDIR /app

EXPOSE 3000

COPY package.json package-lock.json ./

RUN npm install

COPY . ./

CMD ["npm", "start"]
```

The Docker code says the next:
- The container will use node alpine as its base image.
- It sets the working directory.
- Expose the 3000 port.
- It copies the files *package.json* and *package-lock.sjon*
- It runs `npm install` when the image is build.
- Copy all the remaining files from the *k8s-web-hello* folder like *index.mjs*.
- Runs `npm start` as the final command.

For building the Docker container:
```
# docker build ./ -t [DOCKERHUB USERNAME]/k8s-web-hello
```

For these notes, I'll be using my dockerhub username: seb0927
```
# docker build ./ -t seb0927/k8s-web-hello
```

# Pushing custom image to the Docker Hub
> **Time stamp:** 01:38:32

Just do the process:
```
# docker login
# docker push seb0927/k8s-web-hello
```

For the Kubernetes deployment, I will be using mine, but you can use the one from Bodgan Stashchuk: *bstashchuk/k8s-web-hello*

# Creating deployment based on the custom Docker image
> **Time stamp:** 01:40:20

For creating a deployment based on the custom Docker image:
```
# kubectl create deployment k8s-web-hello --image=seb0927/k8s-web-hello
```

When our pods are finally running, we have to expose the ports in order to see our application. For this task we will use ClusterIP (Remember that the IP is only available inside the cluster! What we talked before).

```
# kubectl expose deployment k8s-web-hello --port=3000
```

And if we enter to the cluster:
```
# minikube ssh
# curl [Cluster IP Deployment]:3000; echo
```

We will see that everything is working

# Scaling custom image deployment
> **Time stamp:** 01:45:54

(This was seen before, but let's see it again) For scaling our deployment:
```
kubectl scale deployment k8s-web-hello --replicas=4
```

if we make `curl` again:
```
# curl [Cluster IP Deployment]:3000; echo
```
You will get different responses from the different pods!

# Creating NodePort service
> **Time stamp:** 01:49:21

Now, we wll modify type of the service which we created for our deployment because Cluster IP is onlu available from inside the cluster. Delethe the *k8s-web-hello* service with `kubectl delete service k8s-web-hello` and let's create another one with type **NodePort**:

```
# kubectl expose deployment k8s-web-hello --type=NodePort --port=3000
```

If you list the services again (`k get services`), you will see the service that you crreated with a randomly generated external port. **That external port is where you can connect from your local machine** with the IP where minikube is running (`minikube ip`). So for example, you would have to go to `192.168.59.101:32142` for viewing our application.

The most simple way to get the URL for connection of a specific deployment is:
```
# minikube service [SERVICE NAME]
# minikube service [SERVICE NAME] --url
```
In our case is:
```
# minikube service k8s-web-hello
```

# Creating LoadBalancer Service
> **Time stamp:** 01:53:51

Let's delete the NodePort service created before: `kubectl delete service k8s-web-hello`.

Creating a LoadBalancer service:
```
# kubectl expose deployment k8s-web-hello --type=LoadBalancer --port=3000
```
And now there is a LoadBalancer service. If you do `kubectl get services` you will see that the External-IP is pending, but if you use Kubernetes on cloud services, that IP will be asigned automatically.

Here in Minikube, the behaviour of LoadBalancer is similar to NodePort

# Rolling update of the deployment
> **Time stamp:** 01:57:00

If you make `kubectl describe deploy k8s-web-hello` you will see that the StrategyType is ***RollingUpdate***. That means that when a new version of your application is out, you want to roll out this new version without interrupting your service.

That strategy type is indicating that new pods will be creted with the new image while previous pods will be still running, meaning that pods will be replaced one by one.

Here in this part Bodgan Stashchuk will roll out a new version of the application in order to see how does this works

- Basically, he updates the code and updated the docker image like this:
  ```
  # docker build ./ -t seb0927/k8s-web-hello:2.0.0
  # docker push seb0927/k8s-web-hello:2.0.0
  ```

- And it sets the image to the deployment:
  ```
  # kubectl set image deployment k8s-web-hello k8s-web-hello=seb0927/k8s-web-hello:2.0.0
  ```

- And finally rolls out the new version:
  ```
  # kubectl rollout status deploy k8s-web-hello
  ```
  And our new version is now rolled out! (`minikube service k8s-web-hello`)

# What happens when one of the pods is deleted
> **Time stamp:** 02:05:33

It just gets created again automatically.

# Kubernetes Dashboard
> **Time stamp:** 02:06:32

If you are running Kubernetes on a Cloud service, accessing to the Kubernetes Dashboard can be hard because you have to secure web access to dashboard, but here in minikube is only:

```
# minikube dashboard
```

# Creating YAML deployment specification file
> **Time stamp:** 02:10:50

- **What we've seen:** Imperative approach
- **What are we going to see:** Declarative approach

> 💡 **Delete all done in Kubernetes:** `kubectl delete all --all`

For creating the YAML deployment you need to create two files:
- *deployment.yaml*
- *service.yaml*

Since we have the Kubernetes extension, let's make life easier (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧ ✧ﾟ･: *ヽ(◕ヮ◕ヽ)

You can start writing *"Deployment"* and the Kubernetes extension will suggest you Deployment, and now the Yaml configuration file will be created automatically for you:

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: <Image>
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: <Port>
```

This is a brief explanation of the Deployment declaration:
```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  # Name of the deployment
  name: k8s-web-hello
spec:
  # Here we specify which podes will be managed by 
  # this deployment
  selector:
    matchLabels:
      app: k8s-web-hello

  # It describes the pod. It follows the template of "Pod"
  # suggested from the Kubernetes extension
  template:
    metadata:
      # Label here is the same as spec -> selector -> matchLabels
      labels:
        app: k8s-web-hello

    # Here we specify which containers we want to create in
    # this pod
    spec:
      containers:
      - name: k8s-web-hello
        # Here we put the Docker image
        image: <Image>
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"

        # Specify which ports I would like to open in specific
        # container
        ports:
        # Specify the number of port to expose on the pod's IP
        # address
        - containerPort: <Port>
```

# How to use Kubernetes documentation
> **Time stamp:** 02:17:11

[https://kubernetes.io/docs/reference/kubernetes-api/](https://kubernetes.io/docs/reference/kubernetes-api/)

There you will see how to create any of the services of Kubernetes.

# Applying YAML deployment file
> **Time stamp:** 02:20:39

Now it's time to apply this configuration using a declarative approach. In your terminal, located in the directory where you have your *deployment.yaml*:
```
# kubectl apply -f deployment.yaml
```

If you make any changes in the *deployment.yaml* file, just type the same command:
```
# kubectl apply -f deployment.yaml
```

# Creating YAML service specification file
> **Time stamp:** 2:24:18

In order to create Kubernetes services, you need to go to the *service.yaml* file. Type *Service* and the Kubernetes extension will handle the rest as you did in the last chapter:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
  - port: <Port>
    targetPort: <Target Port>
```

So we need to change it like this:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: k8s-web-hello
spec:
  selector:
    app: k8s-web-hello
  ports:
    # We expose the Node application port (3000) to our local machine
    # to the port 3030
  - port: 3030
    targetPort: 3000
```

Finally, for specifying that we want a LoadBalancer:
```yaml
[...]
spec:
  type: LoadBalancer
  selector:
    app: k8s-web-hello
[...]
```

And for applying our changes:
```
# kubectl apply -f service.yaml
```

Finally, for deleting deployments and services using declarative approach:
```
# kubectl delete -f deployment.yaml -f service.yaml
```

# Plan for the creation of the two deployments
> **Time stamp:** 02:28:34

![image](https://github.com/my-projects-seb0927/freeCodeCamp-Kubernetes_Course/assets/83418390/c4e51957-71cc-4a02-b032-de73a23bbc8b)

> 💡 The two deployments are connected through ClusterIP by a static route (http://nginx). This last solution was thanks to **DNS Service** from Kubernetes

# Creating another web app with two endpoints
> **Time stamp:** 02:31:16

It's just new code added to *k8s-web-to-nginx/index.mjs*

# Building custom Docker image for the second web app
> **Time stamp:** 02:35:19

```
docker build ./ -t [NAME OF THE IMAGE]
```

The same that we did before...

# Creating YAML specification for the second web app
> **Time stamp:** 02:36:38

A new *yaml* file created where deployments and services are all in one.

# Creating YAML specification for the NGINX app
> **Time stamp:** 02:39:02

Just as the title says...

Some anotations:
```yaml
apiVersion: v1
kind: Service
metadata:
  # This name for the service will be utilized inside of
  # our deployment
  name: nginx
spec:
```

Go to the *k8s-web-to-nginx.yaml* to see the important notes!

# Applying specifications for both apps
> **Time stamp:** 02:42:10

The commands for the *yaml* files!

```
k apply -f k8s-web-to-nginx.yaml -f nginx.yaml
```

# Verifying connectivity between different deployments
> **Time stamp:** 02:44:09

```
# minikube service k8s-web-to-nginx
```

Test if http://192.168.49.2:32532/nginx works.

# Resolving Service name to IP address
> **Time stamp:** 02:44:09

```
# kubectl get pods
# kubectl exec [ANY POD FROM k8s-web-to-nginx] -- nslookup nginx
# kubectl get services
# kubectl exec [ANY POD FROM k8s-web-to-nginx] -- wget -q0- http://nginx
```

Kubernets is able to resolve name of the service to corersponding clusterIP

# Other titles
> **Time stamp:** 02:51:09

Well, The 7 minutes left he invests them in deploying apps uding another container runtimes like CRI-O. I just wanted to end so... ajá.

---

And this is the end \ (•◡•) /