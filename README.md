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

## Service
- A Service exposes the Pods to the outside world or to other Pods.
- It provides a stable endpoint even the pods change.
- It automatically load-balances traffic across Pods.

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
