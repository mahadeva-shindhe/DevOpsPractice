# Kubernetes

- Open source container orchestration tool
- Developer by Google
- Helps you manage containerized applications in differenct environments(pysical or cloud)

### What problems does Kubernetes solve?
- Monolith to Microservice 
- Containers
- Managing the containers

### What are the tasks of orchestration tool?
- High availability or no downtime
- Scalability or high performance
- Disaster recovery - backup and restore

### Kubernetes components
#### Pod
- Smallest unit of Kuberentes
- Abstraction over container
- Usually 1 application per pod
- Each pod gets its own IP address
- New IP address upon re-creation

#### Service
- Permanent IP address
- Life cycle of pod and service not connected
- Pod communicate each other using service
- load balancer for request to different pods

#### Ingress 
- Roue traffic into kubernetes cluster
- Used for communicating externally

#### Service
- Usd for internal communication between different pods

#### ConfigMap
- External configurations of application
- Pod are connected to ConfigMap

#### Secret
- used to store secret data
- base 64 encoded
- Pods are connected to Secret

#### Volumes
- data storage or data persistence
- Storage on local machine
- Remote, outside of the kubernetes cluster(Cloud)

#### Deployment
- Pod blueprint
- StateLess applications

Database's can not be replicated as they are having states.
#### StaefulSet
- Avoid data inconsistency
- It is used for stateful apps or databases

## Kubernetes Architecture
- Each node has multiple pods on it
- Worker nodes do the actual work
- Three processes needs to be installed on every node 
    1. Container Runtime (Docker or any other technologies)
    2. Kubelet
        1. interacts with both node and container
        2. sarts the pod with a container inside
    3. KubeProxy - responsible for communication between the pods using the services.
- Master Node 
    - 4 processes run on every master node
        1. API server
           - External clients interacts with API server
           - acts as a Cluster Gateway
           - acts as a gatekeeper for authentication
           - Only one entry point into the cluster from any client
           - Its load balanced
        2. Sheduler 
            - schedule new pod
            - Scheduler just decides on which Node new Pod should be scheduled
            - Kubelet on each Node will recieve requests from Scheduler and executes it(creates new Pod)
        3. Controller Manager
            - detects cluster state change
            - If any state change, then controller interacts with Scheduler and then Kubelet. Then Kubelet will restart the pod.
        4. etcd
            - key value store
            - cluster brain
            - Cluster changes get stored in the key value store
            - stores information - What resources are availabile? Is the cluster healthy? Did the cluster state change?
            - Application data is not stored in etcd
            - Distributed storage across all master nodes
## Cluster Setup
- 2 Master Nodes, 3 Worker nodes.
- Hardware related RAM, CPU and Storage are different for master and worker nodes. Master have less resources and worker nodes have more resources
- Add new Master/Node Server
    - get new bare server
    - install all the master/worker node processes
    - add it to the cluster
        
### Minikube
Any Production Cluster Setup will have Multiple Master and Worker Nodes and they have separate virtual or physical machines.
Test on local machine use minikube. It is one node with master processes and worker processes in it.
- Creates virtual box on your laptop
- Node runs in that virtual box
- 1 Node Kubernetes cluster
- for testing purpose in local
###  Kubectl
different clients which interacts with cluster 
- UI 
- API 
- CLI (Kubectl)
###### Features:
- Kubectl is a command line tool for K8s cluster
- Enables interaction with cluster for master and worker processes
- Kubectl interacts with minikube cluster and cloud cluster
#### Minikube installation
Virtualization on local machine required before installing Minikube. [Installation Guide](https://minikube.sigs.k8s.io/docs/start/)
To know more [cli commands](https://github.com/mahadeva-shindhe/DevOpsPractice/blob/main/basic-kubectl-commands/nginx-deployment.yaml)

#### Basic Kubectl commands
###### CRUD commands
- Create deployment  `kubectl create deployment [name]`
- Edit deployment  `kubectl edit deployment [name]` 
- Delete Deployment `kubectl delete deployment[name]`

###### status of different Kuberentes components
`kubectl get nodes|pod|services|replicaset|deployment`

###### debugging pods
- log to console `kubectl logs [pod name]`
- get interactive terminal `kubectl exec -it [pod name] --bin/bash`
- get info about pod `kubectl describe pod [pod name]`

###### Use configuration file for CRUD
- Apply a configuration file `kubectl apply -f [file name]`
- Delete with configuration file `kubectrl delete -f [file name]`

Create [nginx-deployment.yaml](basic-kubectl-commands\nginx-deployment.yaml)
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.16
          ports:
            - containerPort: 80
```
Create nginx version 1.16 deployment with 2 replica set 
``` 
kubectrl apply -f nginx-deployment.yaml
kubectl get pod
kubectl get deployment
```

Change replica set or version of the image in `nginx-deployment.yaml` and then apply new deployment configuration
```
kubectrl apply -f nginx-deployment.yaml
```
####  YAML Configuration File
The 3 parts of configuration file and it has information for connecting deployment to service to pods

1. Metadata
2. Specification 
3. Status - automatically generated and added by Kuberentes(desired state, actual state, used for self healing of cluster). Status data is in etcd. etcd holds the current status of any K8s component


