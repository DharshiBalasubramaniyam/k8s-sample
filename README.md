# Kubernetes
- Kubernetes (K8s) is a tool that helps you manage and orchestrate (i.e., automate) containers at scale.
- It’s like a manager for your containers, ensuring that: They run properly, They can scale up or down based on traffic, If one fails,
t’s automatically restarted or replaced, Everything can talk to each other easily

## Pod
- A Pod is the smallest unit in Kubernetes. 
- A Pod is where your actual application runs.
- It runs one or more containers (usually one). 
- 1 Pod can run multiple Docker images. Each image becomes 1 container. 

## ReplicaSet
- A ReplicaSet ensures that a certain number of identical Pods are running.
- It maintains high availability by replacing failed Pods automatically.
- If a Pod fails or is deleted, the ReplicaSet will create a new one to maintain the count.

## Deployment
- A Deployment manages the ReplicaSet.
- So, Deployment → controls ReplicaSet → which manages Pods.
- Deployment is used to create stateless pods.

## StatefulSet
- It’s like a Deployment, but for apps that need: **Stable identity (name, network)**, **Stable storage (volume)**, **Ordered deployment & scaling**. e.g., databases, queues, etc

| Deployment | StatefulSet |
|------------|-------------|
|Stateless apps|Stateful apps|
|Web servers, APIs, frontend|Databases, Kafka, Redis (with data), Zookeeper|
|Pods get predictable names: `app-0, app-1, app-2`. If a pod dies, new pod gets same name. | Pods get random hashed names: `app-4581f5, app-a45f1b, app-458ff`. If a pod dies, new pod gets different hashed name. |
|Each pod has its own PV(Stable Persistent Volumes). Pod-0 always uses its own PV-0. | Pods don't have PV |
|Pods start and stop in order. Starts one pod at a time (ordered) | Start or stop all at once |
|Each pods get a DNS name `mysql-0.mysql.default.svc.cluster.local`| Pod names are random `frontend-5d7f9696b7-2abc9`| 

## Service
- A Service exposes the Pods to the outside world or to other Pods.
- Stable IP address - It provides a stable endpoint even the pods change.
- Load balancing - It automatically load-balances traffic across Pods.
- Service → matches Pods using labels → those Pods are created by a Deployment
- **Types:**
	- **ClusterIP**: Default. Access from inside the cluster only. Other pods inside the cluster can reach it. e.g., Ingress to Service1
   	- **Headless**: Used when a user needs to access a particular pod. Useful for stateful apps (databases, etc) that need to know individual pod IPs. Set via `ClusterIP: None`.
   	- **NodePort**: Exposes service outside the cluster via a port on each node. Set via `type: NodePort`. e.g., Browser to Service1
   	- **LoadBalancer**: Creates an external IP with a real cloud load balancer (AWS, GCP, Azure, etc). Set via `type: LoadBalancer`. e.g., AWS LB to Service1

## Node
- A Node is a single machine (virtual or physical) in the cluster where pods run.
- There are usually multiple nodes in a cluster to distribute load.

## Cluster
- A Kubernetes cluster is the entire system.
- It includes a control plane (for managing everything) and nodes (where apps run).
- It manages the overall orchestration of your applications.

## Minikube
- Minikube is a lightweight, local version of Kubernetes. 
- It’s mostly used for learning Kubernetes and developing and testing apps locally before deploying to a real cluster.
- When you run minikube start, it sets up a single-node cluster on your machine.
- In Minikube, there's only one Node, and it's a virtual machine or container that runs inside your host system.
- In Minikube, you create a Deployment to tell Kubernetes what app to run, how many instances, and how to manage updates.
- In Minikube, when you deploy an app, it creates a Pod that runs inside the Node.
- In MiniKube, service exposes your Pod(s) to other Pods or to your local machine.

> Note: For production we can run k8s applications on cloud providers like AWS (EKS), Azure (AKS), or GCP (GKE).

## Config map
- A ConfigMap is used to store non-sensitive configuration data that our application can use.
- We can inject the values from the ConfigMap into a container by using environment variables in our Deployment.

## Secret
- A Secret is used to store sensitive data like passwords, API keys, or tokens securely. Secrets are encoded in Base64 to ensure safe transmission.
- In the Deployment, we reference the Secret to inject the sensitive value into the container’s environment variables.

## Namespaces
- A Namespace is like a virtual cluster inside your real cluster.
- It lets you logically separate and organize your Kubernetes resources (Pods, Deployments, Services, etc.).
- When you install Kubernetes, it comes with a few built-in namespaces:
	- default: The default for pods, deployments etc, if you don’t specify a namespace
	- kube-system: Used by Kubernetes system components (e.g., kube-dns, scheduler)
	- kube-public: Mostly unused, readable by all users, even unauthenticated
	- kube-node-lease: Manages node heartbeat leases for node health monitoring
- A Pod, Deployment, or any other resource in namespace A can only access ConfigMaps and Secrets in namespace A.

## Ingress

- Ingress is a smart router that sends external traffic to the right service inside our cluster, usually over HTTP/HTTPS.
- Without Ingress, we'd expose each service using a NodePort or LoadBalancer, which can get messy.
- Ingress Controller: A special pod (like nginx-ingress-controller) that watches Ingress rules and does the actual routing.
- Ingress Resource: YAML file where you define your routing rules.
- Install an NGINX Ingress Controller in minikube: `minikube addons enable ingress`
