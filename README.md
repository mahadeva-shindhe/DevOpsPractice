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

## Kubernetes components
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
        2. Sheduler 
            - schedule new pod
            - Scheduler just decides on which Node new Pod should be scheduled
            - Kubelet on each Node will recieve requests from Scheduler and executes it(creates new Pod)
        3. Controller Manager
            - detects cluster state change
            - If any state change, then controller interacts with Scheduler and then Kubelet. Then Kubelet will restart the pod.
        4. etcd
        5. 

