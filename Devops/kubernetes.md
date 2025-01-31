# CKA -

## Docker-Fundamental-Overview

### Containers:

Containers are completely isolated environments, as in they can have their own processes or services, their own networking interfaces, their own mounts just like virtual machines.
They all will share the same operating system kernel.

LXC, LXD, LXCFS - containers

The main purpose of the docker is to containerize the application and to ship them and run them.
docker is a platform whose main step is to build ship and run the container.

### Virtual machines vs Containers:

![Containers-vs-VMachine](ContainersvsVMachine.png)

Docker has less isolation as compared to vm they have complete isolation.

### Container vs Image:

An image is a package or a template just like a VM Template. It is used to create one or more containers. Containers are running instances of images that are isolated and have their own environments and set of processes.

### Container Orchestration

Kubernetes is just a container orchestration technology . Docker has its own tool called Docker Swarm.
Kubernetes from google and MesOs from apache.

### Kubernetes Architecture -

- refer for diagram: https://github.com/piyushsachdeva/CKA-2024/blob/main/Resources/Day05/readme.md
- **Kubectl:** It is a type of client that helps to interact with the cluster and its components.
- **Node:** Node is nothing but a virtual machine on which we run our components or workloads is considered as node in kubernetes.
- **Control plane/Master Node:** Is a virtual machine or master node that hosts many administrative components. These components having their own specific purpose and they helps us to run clusters smoothly.
  - In high availability or in production environments we usually have more than one node for a production environment as well to support high availability.
- **Control Plane Components:**
  - **APIServer:** Api Server is the center of the control plane i.e. any incoming request from client will first reach to the APIServer and then APIServer will interact with other components on its behalf.
  - **Scheduler:** It will help us to schedule the workload. It receives the request from the APIServer and it finds the suitable nodes for scheduler pods.
  - **Controller Manager:** It is a combination of many different controllers. So we have node controllers, namespace controllers, deployment controllers etc. It makes sure that all the containers are running fine and everything is monitored. If suppose the pod goes down it  
    will monitor the pod and keep restarting the pod.
  - **etcd:** It is a key value data store. It store the information of cluster nodes, pods, cluster details etc. Every api server will interact with etcd database.
- **Worker Node:** In worker node we have the actual running/work happening in the cluster.
  - **Kubelet:** It receives the instructions from the control plane node. lets suppose we want to delete the pod so we will instruct the api server and it will go to kubelet that will make the changes and delete the pod. and it will inform the APIServer that the pod has been deleted and then APIServer will make the changes in the etcd database. It is a node based agent that works as a communication between worker node and control plane node.
  - **Kube-Proxy:** It helps in enabling the networking within the node. It allows the pod to communicate with each other. It creates some ip tables route.
- **Pod:** It encapsulates a container/ one or more container in its own so that those containers share the resources of the pod. We can have one or more container these can be helping containers/ monitoring agents.

**what kubectl do to connect with APIServer:**
Authenticate -> authenticate -> create a pod (kubectl create pod) -> etcd (it will not create a pod as it is a database it will just create entry in the etcd database) -> scheduler (this will monitor the pod and once it will find it will schedule the node) -> api-server(scheduler will notify to the api server that scheduler found a node go ahead and schedule the pod on this node) -> schedule the pod -> api-server reach put to kubelet and it will add in etcd database.

### process ->

authenticate
validate
retrieve
update in etcd from scheduler
send -> kubelet
response -> api-server -> client

## Kind Cluster(Kubernetes In Docker)

It is a tool to run local kubernetes clusters using Docker Containers.

### Kubernetes Multi Node Cluster ->

Creating a kubernetes cluster is as simple as kind create cluster. (make sure kubectl is installed - kubectl version --client)

- kind create cluster --image (image_name) --name cka-cluster
- kubectl cluster-info --context kind-cka-cluster1
- kubectl get nodes
- multi-node cluster:
  - ex - config.yaml:
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    nodes:
    role: control-plane
    role: worker
    role: worker
- kind create cluster --image (image_name) --name cka-cluster2 --config config.yaml (same command with diff cluster)
- kubectl config --set-context kind-cka-cluster1 - for switching to diff cluster

### PODS:

- So, within the kubernetes cluster we will be having pod.
- There are two different ways:
  - Imperative: Kubectl run nginx
  - Declarative: We will be creating a json/yaml config file and then run the kubectl create or apply command.

## Creating a nginx pod through kubectl imperative way:

- Kubectl run nginx-pod --image=nginx:latest
- This command will create a pod and we can check it by using the kubectl get pod command.

## Creating a nginx pod through kubectl declarative way:

- we will creating a yaml file for the same.

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```

use command to run a file:

- kubectl create -f pod.yaml
- kubectl get pods / kubectl get pods -o wide -> extended information
- to see the pod description: kubectl describe pod nginx-pod
- command to edit the configs directly on running pod -> kubectl edit pod nginx-pod
- kubectl exec -it nginx-pod --sh -> inside the container.
- kubectl run nginx --image=nginx --dry-run=client (it will just dry run and will not apply the changes but will create a pod)
- kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-new.yaml (it will just dry run and will not apply the changes but will create a pod adn will create a file with all the configs)

## Deployment and Replication Controller , ReplicaSet

- Replication controller is here to make sure that all the replicas are running all the time and it make sure the application does not crashes.
- replication_controller.yaml file

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    env: demo
spec:
  template:
    metadata:
      labels:
        env: demo
      name: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
  replicas: 3
  selector:
    matchLabels:
      env: demo
```

for running this:

- kubectl apply -f rc.yaml
- kubectl get pod
- kubectl get rc

## Kubernetes Services

## nodePort: This is a service in which the application will be exposed to a particular port. It is exposed internally.

- targetPort: This is a service in which the application will be exposed on.

nodePort.yaml -

```
apiVersion: v1
kind: Service
metadata:
  name: nodePort-svc
  labels:
    app: demo
spec:
  type: NodePort
  ports:
  - nodePort: 30001
    port: 80
    targetPort: 80
  selector:
    env: demo
```

commands:

- kubectl create -f nodePort.yaml
- kubectl get svc
- kubectl get pod -o wide

## cluster ip:

clusterIp.yaml -

```
apiVersion: v1
kind: Service
metadata:
  name: cluster-svc
  labels:
    app: demo
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 80
  selector:
    env: demo
```

commands:

- kubectl create -f clusterIp.yaml
- kubectl get svc
- kubectl describe svc/cluster-svc
- Endpoint is nothing but an IP of the pod in which IP is listening too.

## load balancer:

lb.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: lb-svc
  labels:
    app: demo
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  selector:
    env: demo
```

commands:

- kubectl create -f lb.yaml
- kubectl get svc
- kubectl describe svc/lb-sv

## external names:

external-name.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: external-name-svc
  namespace: prod
spec:
  type: ExternalName
  externalName: www.example.com
```

there is a method by which we can create service using command line:
kubectl expose rc nginx --port=80 --target-port=8000

## Namespaces:

It provides you another layer of isolation within the cluster so that you can we can separate our objects and resources within the cluster.
