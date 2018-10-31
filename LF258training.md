# LF258 Cource
https://training.linuxfoundation.org/linux-courses/system-administration-training/kubernetes-fundamentals

# mypage
https://training.linuxfoundation.org/cas?destination=portal

# Lab
https://lms.quickstart.com/custom/858487/LFS258%20-%20Labs.pdf

# Chapter1
Intoroduction Linux foundation

# Chapter2
- Basics of Kubernetes
- what is Kubenetes
    - Connecting containers across multiple-hosts
    - scaling
    - deploying app without downtime
    - Service discovery
    - a set of Primitives and a Powerful API
    - 15 years of experience of Google
- the meaning of Kubernetes
    - Greek: Helmsman or Pilot of the ship
    - pilot of a ship of containers
- Containers and Docker have emppered developers with:
    - Ease of building container images
    - Simplicity of shaing images via Docker registries
    - Powerful user experience to manage contaiers
- Challenge
    - managing containers at scale
    - architecting a distributed application based on Microsercvices' principal
- Continuous Integration pipeline
    1. Build your contaier images
    1. test them
    1. verify them
    1. need a cluster of machines acting as your base infrastructure on which to run your containers
    1. need a system to lunch your containers
    1. watch over them when things fail and self-heal
    1. be able to perform rolling updates, rollbacks
    1. need a network setup which permits self-discovery of services in a very ephemeral environment
- Other Solutions
    - Docker Swarm
        - Docker Solution. 
        - It's embedded with the Docker Engine.
    - Apache Mesos
        - DataCenter scheduler. 
        - it builds Multi-level shedurler
        - it can run containers throuch the use of Marathon frameworks
        - it aims to better use a datacenter cluster.
    - Nomad
        - HashiCorp product. the makers of Vagrant and Consul.
        - Managing Containerized application
        - Nomad schedule task defined in Jobs
        - It has Docker driver
    - Rancher
        - container orchestrator agnostic system
        - it provides a sigle pane of glass interface, to managing application.
        - it supports Mesos, Swarm, Kubernetes, Cattle(native system)
    - Kubenetes's distinguishing point is Heritage(History)
- The Borg Heritage
    - Kubernetes is inspired by Borg
        - internal system used by Google
    - Borg is operating over 15 years, world wide.
    - Borg was a Google Secret for a long time.
    - Borg was finally described publicly in 2015. 
    - Borg Documentes: https://research.google.com/pubs/pub43438.html
- Borg Lineage (they inspired by Borg)
    - Borg
        - Mesos
        - Kubernetes
        - Omega
        - Cloud Foundry
        - cgroups
            - LXC
            - Docker
            - OCI
                - rkt/appc
    - cgroups
        - Linux kernel,2007,Contributed by Google. It limits the resource used by collection of processes
        - cgroups and Linux namespaces are at the heart of containres today, including Docker.
    - Mesos
        nspired by discussion with Google when Borg was still a secret.
    - Cloud Foundry Foundation
        - 12 factor application principles. great guidance to build web application
            - it can scale easily
            - it can be deployed in the cloud
            - its build is automated
            - https://12factor.net/
        - Borg and Kubernetes address these principles as well
- Kubernetes Architechture
    - API Server : you can communicate with API
    - kubectl : local client
        - ajax
        - etc
    - Scheduler : requests for running container coming to API an dfind a suitable node to run
    - Controller Manager
    - Nodes
        - Kubelet : it receive requests to run the container and watch over them
        - Service Proxy : it creates and manages neteworking rules
- Kubernetes componentes
    - master : Central Manager
        - it run API server, a Sheduler, a controller, a storage system
        - to keep the sate of the cluster
    - worker nodes
    - Each nodes in the cluster runs tow process, "kubelet" and "Services Proxy"
- Kubernetes characteristics
    - it is made of a manager and a set of nodes
    - it has a scheduler to place container in a cluster
    - it has an API server and a persistence layer with etcd
    - it has a controller to reconcile states
    - it is deployed on VMs or bare-metal machines, in public Clouds, or on-premise
    - it is written in GO
    - it licensed under the Apache Software License v2.0
- Kubernetes Community
    - it was open sourced in June 2014
    - 1500+ contributors
    - K8s Core has over 55,000 commits
    - Meet up:  over 150 cities, 100,000 particioantes in 50 countories
    - slack: 20,000 people
    - Major release: every 3 months
    - Weekly Hangouts take places
- Cloud Native Computing Foundation
    - Google donate Kubernetes to a newly formed foundation within the Linux Foundation in July 2015, when K8s v1.0
    - CNCF members; cisco, Cloud Foundry Foundation, AT&T, Goldman Sachs, etc
- K8s Documents
    - Official Page: https://kubernetes.io/
    - Github: https://github.com/kubernetes/kubernetes
    - Paper: Google Borg(2015): https://research.google.com/pubs/pub43438.html
    - Paper: Borg, Omega, and Kubernetes(2016): https://research.google.com/pubs/pub44843.html
    - Podcast: Borg and Kubernetes with John Wilkes(2016): https://www.gcppodcast.com/post/episode-46-borg-and-k8s-with-john-wilkes/
    - YouTube: Cluster Management at Google(2014):  https://www.youtube.com/watch?v=VQAAkO5B5Hg 
    - CNCF page: https://www.cncf.io/

# Chapter3
- Kubernetes Cluster
    - Master node
    - a set of Worker nodes
- For testing process: Minikube
    - these nodes can be run on the same node(Virtual or Physical).
- Kubernets 6 compornents, it can run as starnderd Linux process or docker container.
    - API server : on master node
    - Scheduler : on master node
    - Controller manager : on master node
    - kubelet: on worker node
    - proxy (aka kube proxy) : on worker node
    - etcd cluster : on master node
- K8s Master nodes runs;
    - API server
        - REST interface to all the K8s resources
        - it's highly configurable
    - scheduler
        - place to the containers on the node in the cluster.  according to various policies, metrics, resource requiremetns
        - it's configurable via command line flags
    - controller manager
        - reconciling the actual state of cluster with the desired state, specified via the API.
            - control loop that performs action based on the observed state of the cluster and the desired state.
        - it's highly configurable
    - the master node can be configured in a multi-master highly available
        - setup: https://github.com/kelseyhightower/kubernetes-the-hard-way
        - indeed, the shcedulers and contoller manages can elect a leader, while the API servers can be fronted by a load-balancer.
- K8s worker nodes runs;
    - kubelet
        - interact with the underlyining docker engine
        - installed on all worker nodes
        - make sure that the containers that need to run are actually running.
    - kube-proxy
        - managing the network connectivity to the containers
    - as well as docker engine
- Keeping State with etcd
    - K8s needs a persistency later,  to know the state fo the cluster over time.
    - Traditionally, relational database may become a single point of failure
    - K8s uses etcd.
        - Distributed key-value stores, made to run on multiple nodes
        - It can be run on a single node (it loses its distributed characteristic, and hence, it's tolerance to failure)
        - etcd use a leader election algorithm
        - to provide strong consistency of the stored state among the nodes.
        - etcdctl
- Networking
    - a pod is a group of co-localted containers the share the same IP address.
    - the network needs to assing IP address to pods, and needs to provide traffic routes between all pods on any nodes
    - Networking challenges
        - Coupled container-to-container communications
            - solved by the pod concept
        - pod-to-pod communication
        - external-to-pod communications
            - solved by the services concept
- Pod
    - K8s call the "single IP-per-Pod Model" 
    - Pod represents a group fo co-locationed contaienrs with associated data volumes.
    - The lowest compute unit in K8s is the Pod.
    - all containers in a pod communicate over "localhost"
    - Pod has a single IP address
    - Containers in a pod, Container A and B and "pause conteiner" share the network namespace. 
        - "pause container" is used to get an IPaddress, then all the containers will use its network namespace
_ CNI
    - K8s is standardizing on "the Container Network Interface(CNI)" spectification.
    - kubeadam(K8s cluster bootstrapping tool) use CNI as the default network interface mechanism.
    - CNI doesn't not help you with pod-to-pod communication across nodes
    - https://github.com/containernetworking/cni

```    
$ cat >/etc/cni/net.d/10-mynet.conf <<EOF
{
	"cniVersion": "0.2.0",
	"name": "mynet",
	"type": "bridge",
	"bridge": "cni0",
	"isGateway": true,
	"ipMasq": true,
	"ipam": {
		"type": "host-local",
		"subnet": "10.22.0.0/16",
		"routes": [
			{ "dst": "0.0.0.0/0" }
		]
	}
}
EOF
```

- Pod-to-Pod communication (Networking)
    - the requirement from K8s
         - All pods can communication eith eath other across nodes.
         - All nodes can communicate with all popds.
         - No "Network Address Translation(NAT)"
    - All IPs involved(nodes and pods) are routable without NAT.
    - Software defined overlay Solution
        - Weave
        - Flannel
        - Calico
        - Romana
- kubectl : main command line cliant
- Network Add-Ons
    - example: networking using Weave net.
        - "$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')""
        - https://www.weave.works/docs/net/latest/kubernetes/kube-addon/
- Mesos Architecture
    - No diffrerent from K8s, at high level.
    - Persistence layer: Zookeeper (instead of etcd)
    - head node / worker nodes
- K8s command
    - check status of each module on master node
        - "systemctl list-units | grep kube"
        - "systemctl list-units | grep etcd"
        - "systemctl status kube-apiserver
            - return "active(running)"
        - "systemctl status kube-proxy
    - configuration
        - "vi /etc/systemd/system/kube-apiserver.service"
        - "vi /etc/systemd/system/kube-proxy.service"
    - check the inside-KVS
        - "etcdctl ls"
        - "etcdctl ls /registry"
        - "etcdctl ls /registry/namespaces"
        - "etcdctl ls /registry/pods/kube-system"
    - check status of each module on worker node
        - "docker ps"
        - "systemctl list-units | grep kube"
    - check Images name, status, version, networking
        - "kubectl get nodes"
        - "kubectl get pods --all-namespaces"
    - configure networking plugin (CNI) (Demo)
        - master node
            - "systecmctl status kubelet"
            - "more /etc/systemd/system/kubelet.service.d/10-kubeadm.conf"
            - "cd /etc/kubernetes/manifests/"
                - etcd.yaml
                - kube-apiserver.yaml
                - kube-contoller0manager.yaml
                - kube-scheduler.yaml
            - "kubectl apply -f ..."
            - kubectl get nodes
            - kubectl get pods --all-namespaces
            - ifconfig
        - worker node
            - "systecmctl status kubelet"
            - "more /etc/systemd/system/kubelet.service.d/10-kubeadm.conf"
            - "more /etc/cni/net.d/weave.conf"
            - "docker ps"
                - k8s_weave-npc..


# Chapter4 K8s install and configuratin
## Install kubectl

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl
```

config file: /.kube/config
- see cluster definitions (IP endopons)
- see credentials
- contexts

switch between context

```
kubectl config use-context foobar
```
## Use GKE

Install

```
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init
```

create k8s cluster

```
gcloud container clusters create linuxfoundation

Creating cluster linuxfoundation...done.
Created [https://container.googleapis.com/v1/projects/k8s-training-190416/zones/us-west1-a/clusters/linuxfoundation].
kubeconfig entry generated for linuxfoundation.
NAME             LOCATION    MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
linuxfoundation  us-west1-a  1.7.8-gke.0     35.203.149.93  n1-standard-1  1.7.8-gke.0   3          RUNNING
```

check cluster

```
gcloud container clusters list

NAME             LOCATION    MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
linuxfoundation  us-west1-a  1.7.8-gke.0     35.203.149.93  n1-standard-1  1.7.8-gke.0   3          RUNNING
```

check k8s node list

```
kubectl get nodes

NAME                                             STATUS    ROLES     AGE       VERSION
gke-linuxfoundation-default-pool-34a8474a-8szm   Ready     <none>    11m       v1.7.8-gke.0
gke-linuxfoundation-default-pool-34a8474a-lk20   Ready     <none>    11m       v1.7.8-gke.0
gke-linuxfoundation-default-pool-34a8474a-sk55   Ready     <none>    11m       v1.7.8-gke.0
```

delete cluster

```
gcloud container clusters delete linuxfoundation
```

## Using Minikube (local environmnet)

install (MacOS)

```
brew cask install minikube
```

Start k8s.

```
minikube start
```

check k8s nodes

```
kubectl get nodes
```

## Minikube internals
- Minikube is an open source project on github https://github.com/kubernetes/minikube#configuring-kubernetes
- it runs a single "Go" binary called "localkube".
- it start a VirtualBox VM that will contain a single nodes k8s deployment and docekr engine
- Minikube VM also runs Docker, to be able to run containers
    - k8s container on docker engine on Virtualbox

connect minikube(virtual box)

```
minikube ssh
```

```
docker ps

CONTAINER ID        IMAGE                                                 COMMAND                  CREATED              STATUS              PORTS               NAMES
7b1ef9c1f72b        gcr.io/google_containers/kubernetes-dashboard-amd64   "/dashboard --inse..."   21 seconds ago       Up 21 seconds                           k8s_kubernetes-dashboard_kubernetes-dashboard-hvr8b_kube-system_e112aabe-ebf3-11e7-a7f8-0800278459df_0
5934570a8d43        gcr.io/k8s-minikube/storage-provisioner               "/storage-provisioner"   About a minute ago   Up About a minute                       k8s_storage-provisioner_storage-provisioner_kube-system_e0b5c968-ebf3-11e7-a7f8-0800278459df_0
6e895b61950c        fed89e8b4248                                          "/sidecar --v=2 --..."   About a minute ago   Up About a minute                       k8s_sidecar_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
a7c09c35a8a6        459944ce8cc4                                          "/dnsmasq-nanny -v..."   About a minute ago   Up About a minute                       k8s_dnsmasq_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
2aa747d09357        512cd7425a73                                          "/kube-dns --domai..."   About a minute ago   Up About a minute                       k8s_kubedns_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
1195d46ba6c1        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 About a minute ago   Up About a minute                       k8s_POD_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
4ab3164f900a        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 About a minute ago   Up About a minute                       k8s_POD_kubernetes-dashboard-hvr8b_kube-system_e112aabe-ebf3-11e7-a7f8-0800278459df_0
09fca79b2aee        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 About a minute ago   Up About a minute                       k8s_POD_storage-provisioner_kube-system_e0b5c968-ebf3-11e7-a7f8-0800278459df_0
6c04d444c578        0a951668696f                                          "/opt/kube-addons.sh"    About a minute ago   Up About a minute                       k8s_kube-addon-manager_kube-addon-manager-minikube_kube-system_7b19c3ba446df5355649563d32723e4f_0
e3a281b3f484        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 About a minute ago   Up About a minute                       k8s_POD_kube-addon-manager-minikube_kube-system_7b19c3ba446df5355649563d32723e4f_0
```

extract NAMES
```
$ docker ps --format "table {{.Names}}"

NAMES
k8s_kubernetes-dashboard_kubernetes-dashboard-hvr8b_kube-system_e112aabe-ebf3-11e7-a7f8-0800278459df_0
k8s_storage-provisioner_storage-provisioner_kube-system_e0b5c968-ebf3-11e7-a7f8-0800278459df_0
k8s_sidecar_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
k8s_dnsmasq_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
k8s_kubedns_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
k8s_POD_kube-dns-86f6f55dd5-tdz5s_kube-system_e12a8a1c-ebf3-11e7-a7f8-0800278459df_0
k8s_POD_kubernetes-dashboard-hvr8b_kube-system_e112aabe-ebf3-11e7-a7f8-0800278459df_0
k8s_POD_storage-provisioner_kube-system_e0b5c968-ebf3-11e7-a7f8-0800278459df_0
k8s_kube-addon-manager_kube-addon-manager-minikube_kube-system_7b19c3ba446df5355649563d32723e4f_0
k8s_POD_kube-addon-manager-minikube_kube-system_7b19c3ba446df5355649563d32723e4f_0
```

```
$ ps -aux | grep localkube

root      3594  9.2 18.0 11287024 370328 ?     Ssl  17:23   0:43 /usr/local/bin/localkube --dns-domain=cluster.local --node-ip=192.168.99.100 --generate-certs=false --logtostderr=true --enable-dns=false
docker    5480  0.0  0.0   9120   472 pts/0    S+   17:31   0:00 grep localkube
```

```
eval $(minikube docker-env)
```

## Minikube usage

usage sample

```
minikube dashboard : open the dashboard
minikube ip : get the IP address of your minikube VM.
```


```
Usage:
  minikube [command]

Available Commands:
  addons           Modify minikube's kubernetes addons
  cache            Add or delete an image from the local cache.
  completion       Outputs minikube shell completion for the given shell (bash or zsh)
  config           Modify minikube config
  dashboard        Opens/displays the kubernetes dashboard URL for your local cluster
  delete           Deletes a local kubernetes cluster
  docker-env       Sets up docker env variables; similar to '$(docker-machine env)'
  get-k8s-versions Gets the list of Kubernetes versions available for minikube when using the localkube bootstrapper
  ip               Retrieves the IP address of the running cluster
  logs             Gets the logs of the running localkube instance, used for debugging minikube, not user code
  mount            Mounts the specified directory into minikube
  profile          Profile sets the current minikube profile
  service          Gets the kubernetes URL(s) for the specified service in your local cluster
  ssh              Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
  ssh-key          Retrieve the ssh identity key path of the specified cluster
  start            Starts a local kubernetes cluster
  status           Gets the status of a local kubernetes cluster
  stop             Stops a running local kubernetes cluster
  update-check     Print current and latest version number
  update-context   Verify the IP address of the running cluster in kubeconfig.
  version          Print the version of minikube
```


## Lab3: Docker networking

```
docker run -d --name=source busybox sleep 3600
docker run -d --name=same-ip --net=container:source busybox sleep 3600
```

check the ip address of the each container

```
# Container 1
docker exec -ti source ifconfig
# container 2
docker exec -ti same-ip ifconfig
```

## Start your K8s head node

run etcd

```
docker run -d --name=k8s -p 8080:8080 gcr.io/google_containers/etcd:3.1.10 etcd --data-dir /var/lib/data
```

start the API server using "hyperkube"
 (But it doesn't up container. Error?)

```
docker run -d --net=container:k8s gcr.io/google_containers/hyperkube:v1.7.6 /apiserver --etcd-servers=http://127.0.0.1:2379 --service-cluster-ip-range=10.0.0.1/24 --insecure-bind-address=0.0.0.0 --insecure-pord=8080 --admission-control=AlwaysAdmit


docekr ps -a

CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS                       PORTS                                                       NAMES
33f968550713        gcr.io/google_containers/hyperkube:v1.7.6   "/apiserver --etcd..."   2 minutes ago       Exited (1) 2 minutes ago                                                                 youthful_almeida
```

Start the Admission contoroller

```
docker run -d --net=container:k8s gcr.io/google_containers/hyperkube:v1.7.6 /controller-manager --master=127.0.0.1:8080
```

To test that, use etcdctl in the etcd container

```
docker exec -ti k8s /bin/sh
export ETCDCTL_API=3
etcdctl get “/registry/api” --prefix=true
```

You can reach your Kubenetes API server and start exploring the API.
 (But it doesn't return. Error?)

```
curl http://127.0.0.1:8080/api/v1

curl: (52) Empty reply from server
```

## K8s application via Dashborad
Open Dashboad
- http://192.168.99.100:30000/#!/overview?namespace=default

```
minikube dashboard
```

Start App

- Workloads > Deployments > Deploy a Containerized App
- redis

## Install K8s with kubeadmin

1. "kubeadm init" on head nodes"
2. "kubeadm join --token <token> <head node IP address>" on worker nodes
3. "kubectl apply -f http:// ..."
    - use resource manifest of the network

## other installation mechanism
- kubespray
    - ansible playbook for k8s cluster
- kops
    - crate a k8s on AWS via CLI
- kube-aws
    - AWS cloud Formation to Provision K8s on AWS
- kubicorn
    - golong libray
    - provision and manage k8s culuster

Insltall K8s suing step by steo manual command
- https://github.com/kelseyhightower/kubernetes-the-hard-way


## Installation Collection
Single node
- Minikube
- kube-solo

## For deployment configuration
- Single-node deployment
    - all components run on the same server.
- Single head node and multiple workers
    - sigle node etcd instance running on the head node with, scheduler, API, controller manager
- Multiple head nodes in asn HA configuration and multiple workers
    -  API server will be fronted by a load balancer, scheduler and controller-manager will elect a leader
    - congitured via flags
    - etcd setp can be single-node
- HA etcd cluster, with HA head nodes and mutiple workers
    - etcd would run as true cluster
    - cluster provide HA and would run separate nodes than the K8s head nodes.

## Running components via containers using Hyperkube
- kubeadam can run the API server, scheduler, controller-manager as container
- hyperkube: all-in-one binary of them, as container images
    - gcr.io/google_containers/hyperkube:v1.7.6
- installation 
    - running kubelet as a system daemon

```
docker run --rm gcr.io/google_containers/hyperkube:v1.7.6 /hyperkube apiserver --help

docker run --rm gcr.io/google_containers/hyperkube:v1.7.6 /hyperkube scheduler --help

docker run --rm gcr.io/google_containers/hyperkube:v1.7.6 /hyperkube controller-manager --help
```
## Compiliing K8s from source

build natively with Golang

```
cd $GOPATH/src/github.com
git clone https://github.com/kubernetes/kubernetes
cd kubenetes
make
```

make quick-release
ls -al _output/bin

## Minikube Demo

```
which minikube
which kubectl
    - k8s client
which gcloud
```

```
kubectl logs xxx
kubectl run ghost --images-ghost
kubectl get pods
kubectl get deploymentes
kubectl config use-context minikube
kubectl expose deployment/ghost --port=2368 --type=NodePort 
kubectl get svc
kubectl describe svc ghost
minikube service ghost
kubectl config view
kubectl config use-context <contaier_id>
kubectl config use-context minikube
```

## Kubeadm demo
create k8s on ubuntu on DegitalOcean

https://kubernetes.io/docs/setup/independent/install-kubeadm/

https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#14-installing-kubeadm-on-your-hosts

kubectl get ndoes
kubectl get ndoes --watch


## Lab4: Installtion using Minukube, kubeadm, manual
https://lms.quickstart.com/custom/858487/Lab4.pdf


# Chapter5 Accessing k8s cluster using API

## The API Endopoint
make sure that Minikube is running

```
$ minikube status

minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```

run a local proxy to connect to the API server

```
$ kubectl proxy 

Starting to serve on 127.0.0.1:8001
```

```
$ curl http://localhost:8001

{
  "paths": [
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/admissionregistration.k8s.io",
    "/apis/admissionregistration.k8s.io/v1alpha1",
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1beta1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1beta1",
    "/apis/apps",
    "/apis/apps/v1beta1",
    "/apis/apps/v1beta2",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authentication.k8s.io/v1beta1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/authorization.k8s.io/v1beta1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/autoscaling/v2beta1",
    "/apis/batch",
    "/apis/batch/v1",
    "/apis/batch/v1beta1",
    "/apis/batch/v2alpha1",
    "/apis/certificates.k8s.io",
    "/apis/certificates.k8s.io/v1beta1",
    "/apis/extensions",
    "/apis/extensions/v1beta1",
    "/apis/networking.k8s.io",
    "/apis/networking.k8s.io/v1",
    "/apis/policy",
    "/apis/policy/v1beta1",
    "/apis/rbac.authorization.k8s.io",
    "/apis/rbac.authorization.k8s.io/v1",
    "/apis/rbac.authorization.k8s.io/v1alpha1",
    "/apis/rbac.authorization.k8s.io/v1beta1",
    "/apis/scheduling.k8s.io",
    "/apis/scheduling.k8s.io/v1alpha1",
    "/apis/settings.k8s.io",
    "/apis/settings.k8s.io/v1alpha1",
    "/apis/storage.k8s.io",
    "/apis/storage.k8s.io/v1",
    "/apis/storage.k8s.io/v1beta1",
    "/healthz",
    "/healthz/autoregister-completion",
    "/healthz/etcd",
    "/healthz/ping",
    "/healthz/poststarthook/apiservice-openapi-controller",
    "/healthz/poststarthook/apiservice-registration-controller",
    "/healthz/poststarthook/apiservice-status-available-controller",
    "/healthz/poststarthook/bootstrap-controller",
    "/healthz/poststarthook/ca-registration",
    "/healthz/poststarthook/generic-apiserver-start-informers",
    "/healthz/poststarthook/kube-apiserver-autoregistration",
    "/healthz/poststarthook/start-apiextensions-controllers",
    "/healthz/poststarthook/start-apiextensions-informers",
    "/healthz/poststarthook/start-kube-aggregator-informers",
    "/healthz/poststarthook/start-kube-apiserver-informers",
    "/logs",
    "/metrics",
    "/swagger-2.0.0.json",
    "/swagger-2.0.0.pb-v1",
    "/swagger-2.0.0.pb-v1.gz",
    "/swagger.json",
    "/swaggerapi",
    "/ui",
    "/ui/",
    "/version"
  ]
}
```

## API Resources

find a pod.
API group a resource ia in specifies the apiVersion

```
$ curl http://localhost:8001/api/v1
{
    (snip)

    {
      "name": "pods",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "proxy",
        "update",
        "watch"
      ],
      "shortNames": [
        "po"
      ],
      "categories": [
        "all"
      ]
    }

    (snip)
  ]
}%
```

Typically a resource manifests(available in JSON or YAML formats) will basic skeleton.
- kind:
- apiVersion:
- metadata:
- spec:

API Conventions
- https://github.com/kubernetes/community/blob/master/contributors/devel/api-conventions.md


## Your First Pod

an expample of a pod manifest in YAML format.

```
apiVersion: v1
kind: Pod
metadata:
    name: busybox
    namespace: default
spec:
    containers:
    - name: busybox
      image: busybox
      command:
        - sleep
        - "3600"
```

create this pod in K8s from YAML format.
 
```
$ kubectl create -f sample_pod.yaml

pod "busybox" created
```

```
kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
busybox                  1/1       Running   0          29s
```

## Managing Resource with REST
REST = managed via HTTP calls
- GET
- POST
- PATCH
- DELETE
- etc

TO discovery the API

```
kubectl --v=99 get pods busybox

(snip)

I0104 07:53:27.135814   63287 round_trippers.go:417] curl -k -v -XGET  -H "Accept: application/json" -H "User-Agent: kubectl/v1.8.1 (darwin/amd64) kubernetes/f38e43b" https://192.168.99.100:8443/api/v1/namespaces/default/pods/busybox
I0104 07:53:27.147042   63287 round_trippers.go:436] GET https://192.168.99.100:8443/api/v1/namespaces/default/pods/busybox 200 OK in 11 milliseconds
```

If you delete this pod, HTTP method changes from GET to DELETE

URI : /api/v1/namespaces/default/pods/busybox

```
kubectl --v=99 delete pods busybox

(snip)

I0104 07:59:04.029377   63336 round_trippers.go:417] curl -k -v -XDELETE  -H "Accept: application/json, */*" -H "User-Agent: kubectl/v1.8.1 (darwin/amd64) kubernetes/f38e43b" https://192.168.99.100:8443/api/v1/namespaces/default/pods/busybox
I0104 07:59:04.032576   63336 round_trippers.go:436] DELETE https://192.168.99.100:8443/api/v1/namespaces/default/pods/busybox 200 OK in 3 milliseconds
```

## Namespaces

Every Request is namespaced
- ex) GET https://192.168.99.100:84443/api/v1/namespaces/default/pods


```
kubectl get ns

NAME          STATUS    AGE
default       Active    6d
kube-public   Active    6d
kube-system   Active    6d
```

```
kubectl create ns linuxcon

namespace "linuxcon" created
```

```
kubectl get ns/linuxcon

NAME       STATUS    AGE
linuxcon   Active    53s
```

```
kubectl get ns/linuxcon -o yaml

apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: 2018-01-04T16:07:19Z
  name: linuxcon
  resourceVersion: "190082"
  selfLink: /api/v1/namespaces/linuxcon
  uid: 5773f71a-f169-11e7-a7f8-0800278459df
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
```

```
kubectl get ns/linuxcon -o json

{
    "apiVersion": "v1",
    "kind": "Namespace",
    "metadata": {
        "creationTimestamp": "2018-01-04T16:07:19Z",
        "name": "linuxcon",
        "resourceVersion": "190082",
        "selfLink": "/api/v1/namespaces/linuxcon",
        "uid": "5773f71a-f169-11e7-a7f8-0800278459df"
    },
    "spec": {
        "finalizers": [
            "kubernetes"
        ]
    },
    "status": {
        "phase": "Active"
    }
}
```

```
kubectl delete ns/linuxcon

namespace "linuxcon" delete
```

sample: create a pod in the linuxcon namespace:

```
piVersion: v1
kind: Pod
metadata:
  name: redis
  namespace: linuxcon
...
```

## Check API Resource with kubectl

- kubectl get pods
- kubectl get namespaces
- kubectl get replicasets
- kubectl get deployments
- kubectl get services

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
redis-7f5f77dc44-qmx56   1/1       Running   0          8d
```

```
kubectl get namespaces

NAME          STATUS    AGE
default       Active    9d
kube-public   Active    9d
kube-system   Active    9d
```

```
kubectl get replicasets
NAME               DESIRED   CURRENT   READY     AGE
redis-7f5f77dc44   1         1         1         8d
```

```
kubectl get deployments
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
redis     1         1         1            1           8d
```

```
kubectl get services
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP    9d
redis        ClusterIP   10.108.112.135   <none>        8002/TCP   8d
```

detailed manuffest of a specific resource
- kubectl get pods busybox -o yaml

```
kubectl get pods redis-7f5f77dc44-qmx56 -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:

(snip)
```

## Swagger specification and Open API

K8s APU use a Swagger specification
- https://swagger.io/specification/
- this evolving toward the OpenAPI

OpenAPI initiative
- It is usefule, auto-generate client code
- https://www.openapis.org/

## Additional Resource Management Methods
access log with endpoins
- GET /api/v1/namespaces/{namespace}/pods/{name}/exec
- GET /api/v1/namespaces/{namespace}/pods/{name}/logs
- GET /api/v1/namespaces/{namespace}/pods/{name}/portforward
- GET /api/v1/watch/{namespace}/pods/{name}

Kubectl: access the logs of a container
- kubectl logs --help
- kubectl exec --help
- kubectl port-forward --help
- kubectl attach --help

```
kubectl logs --help
Print the logs for a container in a pod or specified resource. If the pod has only one container, the container name isoptional.

Aliases:
logs, log

Examples:
  # Return snapshot logs from pod nginx with only one container
  kubectl logs nginx

  # Return snapshot logs for the pods defined by label app=nginx
  kubectl logs -lapp=nginx

  # Return snapshot of previous terminated ruby container logs from pod web-1
  kubectl logs -p -c ruby web-1

  # Begin streaming the logs of the ruby container in pod web-1
  kubectl logs -f -c ruby web-1

  # Display only the most recent 20 lines of output in pod nginx
  kubectl logs --tail=20 nginx

  # Show all logs from pod nginx written in the last hour
  kubectl logs --since=1h nginx

  # Return snapshot logs from first container of a job named hello
  kubectl logs job/hello

  # Return snapshot logs from container nginx-1 of a deployment named nginx
  kubectl logs deployment/nginx -c nginx-1

Options:
  -c, --container='': Print the logs of this container
  -f, --follow=false: Specify if the logs should be streamed.
      --include-extended-apis=true: If true, include definitions of new APIs via calls to the API server. [default true]
      --interactive=false: If true, prompt the user for input when required.
      --limit-bytes=0: Maximum bytes of logs to return. Defaults to no limit.
      --pod-running-timeout=20s: The length of time (like 5s, 2m, or 3h, higher than zero) to wait until at least one pod is running
  -p, --previous=false: If true, print the logs for the previous instance of the container in a pod if it exists.
  -l, --selector='': Selector (label query) to filter on.
      --since=0s: Only return logs newer than a relative duration like 5s, 2m, or 3h. Defaults to all logs. Only one ofsince-time / since may be used.
      --since-time='': Only return logs after a specific date (RFC3339). Defaults to all logs. Only one of since-time /since may be used.
      --tail=-1: Lines of recent log file to display. Defaults to -1 with no selector, showing all log lines otherwise 10, if a selector is provided.
      --timestamps=false: Include timestamps on each line in the log output

Usage:
  kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

```
kubectl exec --help
Execute a command in a container.

Examples:
  # Get output from running 'date' from pod 123456-7890, using the first container by default
  kubectl exec 123456-7890 date

  # Get output from running 'date' in ruby-container from pod 123456-7890
  kubectl exec 123456-7890 -c ruby-container date

  # Switch to raw terminal mode, sends stdin to 'bash' in ruby-container from pod 123456-7890
  # and sends stdout/stderr from 'bash' back to the client
  kubectl exec 123456-7890 -c ruby-container -i -t -- bash -il

  # List contents of /usr from the first container of pod 123456-7890 and sort by modification time.
  # If the command you want to execute in the pod has any flags in common (e.g. -i),
  # you must use two dashes (--) to separate your command'sflags/arguments.
  # Also note, do not surround your command and its flags/arguments with quotes
  # unless that is how you would execute it normally (i.e.,do ls -t /usr, not "ls -t /usr").
  kubectl exec 123456-7890 -i -t -- ls -t /usr

Options:
  -c, --container='': Container name. If omitted, the firstcontainer in the pod will be chosen
  -p, --pod='': Pod name
  -i, --stdin=false: Pass stdin to the container
  -t, --tty=false: Stdin is a TTY

Usage:
  kubectl exec POD [-c CONTAINER] -- COMMAND [args...] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

```

➜  Kubernetes-memo git:(master) ✗ kubectl port-forward --help
Forward one or more local ports to a pod.

Examples:
  # Listen on ports 5000 and 6000 locally, forwarding data to/from ports 5000 and 6000 in the pod
  kubectl port-forward mypod 5000 6000

  # Listen on port 8888 locally, forwarding to 5000 in the pod
  kubectl port-forward mypod 8888:5000

  # Listen on a random port locally, forwarding to 5000 in the pod
  kubectl port-forward mypod :5000

Options:
  -p, --pod='': Pod name

Usage:
  kubectl port-forward POD [LOCAL_PORT:]REMOTE_PORT [...[LOCAL_PORT_N:]REMOTE_PORT_N] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

```
kubectl attach --help
Attach to a process that is already running inside an existing container.

Examples:
  # Get output from running pod 123456-7890, using the first container by default
  kubectl attach 123456-7890

  # Get output from ruby-container from pod 123456-7890
  kubectl attach 123456-7890 -c ruby-container

  # Switch to raw terminal mode, sends stdin to 'bash' in ruby-container from pod 123456-7890
  # and sends stdout/stderr from 'bash' back to the client
  kubectl attach 123456-7890 -c ruby-container -i -t

  # Get output from the first pod of a ReplicaSet named nginx
  kubectl attach rs/nginx

Options:
  -c, --container='': Container name. If omitted, the firstcontainer in the pod will be chosen
      --pod-running-timeout=1m0s: The length of time (like 5s, 2m, or 3h, higher than zero) to wait until at least one pod is running
  -i, --stdin=false: Pass stdin to the container
  -t, --tty=false: Stdin is a TTY

Usage:
  kubectl attach (POD | TYPE/NAME) -c CONTAINER [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

## Pod Logs and Exec
check log of the container

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
redis-7f5f77dc44-qmx56   1/1       Running   0          8d
```

```
kubectl logs redis-7f5f77dc44-qmx56

1:C 29 Dec 19:34:39.877 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 29 Dec 19:34:39.877 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 29 Dec 19:34:39.877 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 29 Dec 19:34:39.878 * Running mode=standalone, port=6379.
1:M 29 Dec 19:34:39.879 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 29 Dec 19:34:39.879 # Server initialized
1:M 29 Dec 19:34:39.879 * Ready to accept connections
```

```
kubectl exec -ti redis-7f5f77dc44-qmx56 bash

root@redis-7f5f77dc44-qmx56:/data#
root@redis-7f5f77dc44-qmx56:/data# redis-cli
127.0.0.1:6379>
```

## Connecting to a Pod with Port Forwarding (Not exec)

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
redis-7f5f77dc44-qmx56   1/1       Running   0          9d
```

```
kubectl port-forward redis-7f5f77dc44-qmx56 6379:6379

Forwarding from 127.0.0.1:6379 -> 6379
```

## API demo

check API 

```
minikube ssh
```

```
curl localhost:8080
curl localhost:8080/version
curl localhost:8080/api/v1
```

```
exit
```

namespace

```
kubectl get ns

NAME          STATUS    AGE
default       Active    10d
kube-public   Active    10d
kube-system   Active    10d
```

```
 kubectl get pods --all-namespaces


NAMESPACE     NAME                          READY     STATUS    RESTARTS   AGE
default       redis-7f5f77dc44-qmx56        1/1       Running   0          9d
kube-system   kube-addon-manager-minikube   1/1       Running   0          10d
kube-system   kube-dns-86f6f55dd5-tdz5s     3/3       Running   0          10d
kube-system   kubernetes-dashboard-hvr8b    1/1       Running   0          10d
kube-system   storage-provisioner           1/1       Running   0          10d
```

create namespace

```
kubectl create ns foobar

namespace "foobar" created
```

```
kubectl get ns

NAME          STATUS    AGE
default       Active    10d
foobar        Active    2m
kube-public   Active    10d
kube-system   Active    10d
```

create namespace from yaml file

```
vi ns.yaml

apiVersion: v1
kind: Namespace
metadata:
  name: lfs258
```

```
kubectl create -f ns.yaml

namespace "lfs258" created
```

```
kubectl get ns

NAME          STATUS    AGE
default       Active    10d
foobar        Active    6m
kube-public   Active    10d
kube-system   Active    10d
lfs258        Active    2m
```

## LAB5
https://lms.quickstart.com/custom/858487/Lab5.pdf


create pod in a namespace

```
vi redis-lfs258.yaml

apiVersion: v1
kind: Pod
metadata:
  namespace: lfs258
  name: redis
spec:
  containers:
    - name: redis
      image: redis
```

```
kubectl create -f redis-lfs258.yaml

pod "redis" created
```

```
kubectl get pods --namespace=lfs258

NAME      READY     STATUS    RESTARTS   AGE
redis     1/1       Running   0          5m
```

```
kubectl get pods --all-namespaces
NAMESPACE     NAME                          READY     STATUS    RESTARTS   AGE
default       redis-7f5f77dc44-qmx56        1/1       Running   0          9d
kube-system   kube-addon-manager-minikube   1/1       Running   0          10d
kube-system   kube-dns-86f6f55dd5-tdz5s     3/3       Running   0          10d
kube-system   kubernetes-dashboard-hvr8b    1/1       Running   0          10d
kube-system   storage-provisioner           1/1       Running   0          10d
lfs258        redis                         1/1       Running   0          5m
```

log

```
kubectl logs redis --namespace=lfs258
1:C 07 Jan 20:04:26.215 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 07 Jan 20:04:26.215 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 07 Jan 20:04:26.215 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 07 Jan 20:04:26.216 * Running mode=standalone, port=6379.
1:M 07 Jan 20:04:26.217 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 07 Jan 20:04:26.217 # Server initialized
1:M 07 Jan 20:04:26.217 * Ready to accept connections
```

delete namespace, pods

```
kubectl delete pods redis

pod "redis" deleted
```

```
kubectl delete pods redis --namespace=lfs258

pod "redis" deleted
```

```
kubectl delete ns lfs258

namespace "lfs258" deleted
```

# Chapter 6: Replication Controllers and Deployments

## Reviewing Pods

A pod shares a simple IP address among all the containers in the pods.

minimalistic pods

```
vi redis.yaml

apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - image: redis:3.2
    imagePullPolicy: IfNotPresent
    name: redis
  restartPolicy: Always
```

```
kubectl create -f redis.yaml

pod "redis" created
```

```
kubectl get pod

NAME                     READY     STATUS    RESTARTS   AGE
redis                    1/1       Running   0          4m
```

## Replication Controllers
Doc
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/

Replica Sets: new class of Replicatin Controllers
Replication Contollers
- Define: what a pod should be
- Define: how may you want running

Sample: redis Relication Controller

```
kubectl get rc redis -o yaml
```

scalability : change the number of desited replicas

```
kubectl scale rc redis --replicas=5
```

--watch: keep to display result

```
kubectl get pods --watch
```

## Replication Controller Specifications

labels
- to count the number of pods. 
- key-value pair.

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: redis
  namespace: default
spec:
  replicas: 2
  selector:
    app: redis
  template:
    metadata:
      name: redis
      labels:
        app: redis
    spec:
      containers:
         - image: redis:3.2
           name: redis
```

## Towrds Deployments
Rolling updates has many problem: lose connectivity, leave the cluster in a bad state.  

Deployments
- To avoid problems when scaling the RC on client side.
- extensions/v1beta1 API group. API extentionsv
- allow server-side updates to pods at a specified rate
- Used for canary and otehr deployment patterns

```
curl http://127.0.0.1:8080/apis/extensions/v1beta1

{
    "kind": "APIresourceList",
    "groupVersion": "extentions/v1beta1",
    "resources": [
        {
            "name": "deployments",
            "namespaced": true,
            "kind": "Deployment"
        },
    ]

}
```

Cretea a Delployment with a sigle contaier per pods

```
kubectl run nginx --image=nginx

deployment "nginx" created
```

```
kubectl get deployments

NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx     1         1         1            0           4m
redis     1         1         1            1           12d
```

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
nginx-d5dc44cf7-9wd68    1/1       Running   0          5m
redis                    1/1       Running   0          1d
redis-7f5f77dc44-kfnsh   1/1       Running   0          2d
```


Replica Sets(RS)
- Deployments generate Replica Sets(RS)
- the new generation of RC.
- it bring set-base pods selector

## Labels
Every resource cancontain labels in its metadata

```
apiVersion: v1
kind: Pod
metadata:

...
    labels:
        pod-template-hash: "3378155678"
        run : ghost
```


By default, Deployment with "kubectl run" adds label to the pods

guery by label, and display labels

```
kubectl get pods -l run=nginx

NAME                    READY     STATUS    RESTARTS   AGE
nginx-d5dc44cf7-9wd68   1/1       Running   0          1d
```

```
kubectl get pods -Lrun

NAME                     READY     STATUS    RESTARTS   AGE       RUN
nginx-d5dc44cf7-9wd68    1/1       Running   0          1d        nginx
redis                    1/1       Running   0          2d
redis-7f5f77dc44-kfnsh   1/1       Running   0          4d
```

define labels in pod templates and in specificatins of Deployments


```
kubectl label pods ghost-3378155678-eq5i6 foor=bar
```

add label "foo = bar" to nginx-d5dc44cf7-9wd68 Pods

```
kubectl label pods nginx-d5dc44cf7-9wd68 foor=bar

pod "nginx-d5dc44cf7-9wd68" labeled
```

```
kubectl get pods --show-labels
NAME                     READY     STATUS    RESTARTS   AGE       LABELS
nginx-d5dc44cf7-9wd68    1/1       Running   0          1d        foor=bar,pod-template-hash=818700793,run=nginx
redis                    1/1       Running   0          2d        <none>
redis-7f5f77dc44-kfnsh   1/1       Running   0          4d        app=redis,pod-template-hash=3919338700
```

query and select resource using labels

use a "nodeSelector" in a pod definition, add specific labels to certain nodes in cluseter and use those labels in the pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
  nodeSelector:
    disktype: ssd
```

## How Do Deployments work

Deployments create Replica Sets

Replica Setes
- new version of replication Controllers
- They make sure the right amount of pods is running at all times.

Deployment allows to create multiple Replica Sets for rolling updates and rollbacks.
- By scaling RS up and down.

see deployment status
- K8s dashbord
- CLI

```
kubectl get deployments

NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx     1         1         1            1           2d
redis     1         1         1            1           14d
```

```
kubectl get rs

NAME               DESIRED   CURRENT   READY     AGE
nginx-d5dc44cf7    1         1         1         2d
redis-7f5f77dc44   1         1         1         14d
```

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
nginx-d5dc44cf7-9wd68    1/1       Running   0          2d
redis                    1/1       Running   0          3d
redis-7f5f77dc44-kfnsh   1/1       Running   0          4d
```

Deployments can be scaled

```
kubectl scale deployment/nginx --replicas=4

deployment "nginx" scaled
```

```
kubectl get deployments

NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx     4         4         4            4           2d
redis     1         1         1            1           14d
```

```
kubectl get rs

NAME               DESIRED   CURRENT   READY     AGE
nginx-d5dc44cf7    4         4         4         2d
redis-7f5f77dc44   1         1         1         14d
```

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
nginx-d5dc44cf7-4w8v9    1/1       Running   0          39s
nginx-d5dc44cf7-9kk55    1/1       Running   0          39s
nginx-d5dc44cf7-9wd68    1/1       Running   0          2d
nginx-d5dc44cf7-dvgk4    1/1       Running   0          39s
redis                    1/1       Running   0          3d
redis-7f5f77dc44-kfnsh   1/1       Running   0          4d
```

update all your pods to a specific image version.   
"latest" is not a version number.


```
kubectl set image deployment/nginx nginx=nginx:1.10 --all

deployment "nginx" image updated
```

```
kubectl get rs --watch

NAME               DESIRED   CURRENT   READY     AGE

nginx-5f8bcd574c   2         2         0         5s  << New
nginx-d5dc44cf7    3         3         3         2d  << Old
redis-7f5f77dc44   1         1         1         14d
```

a few minitues later.
New Replica Set was created.
- new RS was scale up
- original RS was scale down.

```
kubectl get rs --watch

NAME               DESIRED   CURRENT   READY     AGE
nginx-5f8bcd574c   4         4         4         1m  << New
nginx-d5dc44cf7    0         0         0         2d  << Old
redis-7f5f77dc44   1         1         1         14d
```

To trigger a rolling update by edit deployment resrouce

```
kubectl edit deployment/nginx
```

## Deployment Rollbacks

rollback to a previous revision by scaling up and down the RS
- kubectl run --record

```
# create a deployment
kubectl run ghost --image=ghost --record
```

```
kubectl get deployments

NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ghost     1         1         1            1           46d
```

```
 kubectl get replicaset
NAME               DESIRED   CURRENT   READY     AGE
ghost-688d7cc85f   0         0         0         36m
ghost-8449997474   1         1         1         46d
```

```
kubectl get deployments ghost -o yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
>>  deployment.kubernetes.io/revision: "1"
>>  kubenetes.io/change-cause: kubectl run ghost --image=ghost --record
  creationTimestamp: 2018-01-12T20:43:44Z
  generation: 1
  labels:
    run: ghost
  name: ghost
  namespace: default
  resourceVersion: "485769"
  selfLink: /apis/extensions/v1beta1/namespaces/default/deployments/ghost
  uid: 47abc22c-f7d9-11e7-a7f8-0800278459df
spec:
  replicas: 1
  selector:
    matchLabels:
      run: ghost
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: ghost
    spec:
      containers:
      - image: ghost
        imagePullPolicy: Always
        name: ghost
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  conditions:
  - lastTransitionTime: 2018-01-12T20:43:44Z
    lastUpdateTime: 2018-01-12T20:43:44Z
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  observedGeneration: 1
  replicas: 1
  unavailableReplicas: 1
  updatedReplicas: 1
```

You can now roll back

```
kubecrtl rollout undo
```

```
# update Container image
kubectl set image deployment/ghost ghost=ghost:0.9 --all

deployment "ghost" image updated
```

```
kubectl rollout history deployment/ghost

deployments "ghost"
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
ghost-688d7cc85f-6nktz   1/1       Running   0          3m
```

```
kubectl rollout undo deployment/ghost

deployment "ghost" rolled back
```

```
kubectl get pods

NAME                     READY     STATUS              RESTARTS   AGE
ghost-688d7cc85f-6nktz   0/1       Terminating   0          5m
ghost-8449997474-wtrgq   1/1       Running       0          46s
```

a few seconds later

```
kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE
ghost-8449997474-wtrgq   1/1       Running   0          1m
```

```
kubectl rollout history deployment/ghost

deployments "ghost"
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
```

## 6.12. Deployments

roll back to a specific revision

```
kubecrtl rollout undo --to-revision=2 deployment/ghost
```

Edit a Deployment

```
kubectl edit deployment/ghost
```

Pause and resume a Deployment

```
kubectl rollout pause deployment/ghost
```

```
kubectl rollout resume deployment/ghost
```

Rolling updade on Replication Controllers
(Note! Done on the client side. If you close your client. The rolling update will stop)

```
kubectl rolling-update
```

## Deployments demo
- minikube status
- kubectl get pods
- kubectl run ghost --image=ghost
  - create deployment
- kubectl get deployments
- kubectl get replicaset
- kubectl get pods
- kubectl expose deploy/ghost --port=2368 --type=NodePort
  - open port
- minikube service ghost
- kubectl get pods
- kubectl scale deployment/ghost --replicas=10
  - make 10 pods
- kubectl get pods
- kubectl get deployments ghost -o json
- kubectl get deployments ghost -o yaml
- kubectl get pods -Lrun
- kubectl label pods ghost-8449997474-wtrgq run- 
  - remove lable
- kubectl get pods -Lrun
- kubectl set image deployment/ghost ghost=ghost:0.9
  - change image ghost:0.9
- kubectl get rs ( = kubectl get replicaset)
- kubectl get rs --watch
- kubectl get pods

Rolling update

- kubectl rollout history deployment/ghost
- kubectl get pods ghost-8449997474-wtrgq -o yaml
  - check "image: ghost:0.9"
- kubectl run redis --image=redis --record
- kubectl rollout history deployment/redis
- kubectl get pods
- kubectl get rs
- kubectl get pods
- kubeclt rollout undo deployment/ghost
- kubectl get rs --watch
- kubectl get rs
- kubectl get pods
- kubectl get pods ghost-8449997474-wtrgq -o yaml
  - check "image: ghost"

## Lab6
https://lms.quickstart.com/custom/858487/Lab6.pdf

# Chapter 7: Volumes and Application Data

## Introducing Volumes
single container, single volume(scratch-volume)

```
apiVersion: v1
kind: Pod
metadata:
    name: busybox
    namespace: default
spec:
    containers:
    - image: busybox
      name: busy
      command:
        - sleep
        - "3600"
      volumeMounts: <<
    - mountPath: /scratch << 
    volumes: <<
    - name: scratch-volume << single-volume
      emptyDir: {} << Volume type
```

kubelet will crate an empty directry and will delete directry.

## Volume Types

emptyDIR: an emppty directory that gets erased when the Pods deis.
hostPath: volumes servive Pod deletion

NFS(Network File System) and iSCSI(Internete Small Computer System Interface)
are choice for multiple readers scenarios.

CephFS or GlusterFS can be a choisce for multiple writers in your kubenetes cluster.

GCEpersitentDisk : volumes of GCE
awsElasticBlockStore: volumes of AWS

## Shared Volume Example

2 containers with access to a shared volume

```
containers:
    - image: busybox
  volumeMounts:
    - mountPath: /busy
  name: test
  name: busy
    - image: busybox
  volumeMounts:
    - mountPath: /box
  volumes:
    - name: test
    emptyDir: {}
```

```
kubectl exec -ti busybox -c box -- touch /box/foobar

kubectl exec -ti busybox -c box -- ls -l /busy total 0
```

## Persistent Volumes and Claims

Persistent Volume : PV
- Storage abstraction
- Pods define a volume type persistentVolumeClaim(PVC)
- The cluster then attached the persistentVolume

Phases to persistent storage
1. Provisioning
    - created from PV by the cluster administrator
    - Requested from a dynamic souce(Cloud Provider)x
2. Binding
    - a control loop on the master notice the PVC
    - containing an amount of storage, access request, a particular StorageClass.
    - The watcher locates a matching PV or waits for the StorageClass provisioner to create one.
    - the PV must match at least the strage amount requested
3. Use
    - the bound volume is mounted for the Pod to use.
4. Releasing
    - the Pod is done with the volume and an API request is sent, deleting the PVC.
    - The volume remains in the state from when the claim is deleted until available to a new claim.
    - The resident data remains depending on the persistentVolumeReclaimPolicy

## Persistent Volume

PersistentVolume using the hostPath type.

```
kind: PersistentVolume
apiVersion: v1
metadata:
    name: 10Gpv01
    labels:
        type: local
spec:
    capacity:
        storage: 10Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: "/somepath/data01"
```

- Persustebt volumes are not a namespace objects.
- v1.9: allows static provisioning of Raw Block Volume. support the Fibre Channel plugin.

## Persistent Volume Claim

persistentVolumeClaim
- With a persistent volume created in your cluster, you can then write a manifest for a claim and use that claim in your pod definition.

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    Rsouces:
        requests:
            storage: 8GI
(In the Pod)

...

spec:
    continers:

...

```

the Pod configuration (complex sample?)

```
volumeMounts:
    - name: Cephpd
    mountPath: /data/rbd

  volumes:
    - mame: rbdpd
      rbd:
        monitors:
        - '10.19.14.22:6789'
        - '10.19.14.23:6789'
        - '10.19.14.24:6789'
        pool: k8s
        image: client
        fsType: ext4
        readOnly: true
        user: admin
        keyring: /etc/ceph/keyring
        imageformat: "2"
        imagefeatures: "layering"
```

## Dynamic Provisioning

Dynamic Provisioning
- starting with K8s v1.4
- allow cluster to request storage from an exterior, pre-configured source
- StorageClass API resource allows an administrator to define a persistent volume provisioner of a certain type, passing storage-specific parameters.
- With StorageClass created,  a user can request a claim, which the API server fills via Autoprovisioning.
- dynamic Strage: AWS, GCE and Ceph cluster, iSCSI

Storage Class using GCE

```
apiVersion: storage.k8s.io/v1       # Recently became stable
kind: StorageClass
metadata:
    name: fast                      # Could be any name
provisioner: kubernetes.io/gce-pd
parameters:
    type: pd-ssd
```

## Secrets
Secret API
- The password could be encoded.
- a casual reading would not give away the password

```
kubectl get secrets

NAME                  TYPE                                  DATA      AGE
default-token-tx78l   kubernetes.io/service-account-token   3         118d
```

Mnually encoded with kubectl create secret

```
kubectl create secret generic mysql --from-literal=password=root

secret "mysql" created
```

```
kubectl get secrets

NAME                  TYPE                                  DATA      AGE
default-token-tx78l   kubernetes.io/service-account-token   3         118d
mysql                 Opaque                                1         8m
```

- A secret is not encrypted, only base64-encoded.You can see the encoded string inside the secret with kubectl.
- The secret will be decoded and be presented as a string saved to a file.


## Using Secrets via Environment Variables

Configuring secret variable in Pods

```
...

spec:
    containers:
    - image: mysql:5.5
      env:
      - name: MYSQL_ROOT_PASSWORD
        valueFrom:
            secretKeyRef:
                name: mysql
                key: password
          name: mysql
```

- the number of Secrets is 1MB limit to theier size
- Each secert occupy memory, along with other API, objects...
- They are stored in the tmpfs storage on the host node, and are only sent to the host running Pod.
  All volumes requested by a Pod must be mounted before the containers within the Pod are started
- a Secret must exist prior to being requested

## Mounting Secrets as Volumes

Mount secrets as files using a volume definition in a pod manifest

```
...

spec:
    containers:
    - image: busybox
      command:
        - sleep
        - "3600"
      volumeMounts:
      - mountPath: /mysqlpassword
        name: mysql
      name: busy
    volumes:
    - name: mysql
        secret:
            secretName: mysql
```

once the pod is running, you can verify that the secret is indeed accessible in the container.

```
kubectl exec -ti busybox -- cat /mysqlpassword/password
```

## Portable Data with ConfigMaps

Secrets with ConfigMaps, except the data is not encoded.

ConfigMap can be used in several way
- A pod can use the data as environment variables from one or more sources.
- The values countained inside can be passed to commands inside the pods.
- A Volume or a file in a Volume can be created, including different names and object will have a "data" section countaining the content of the file.

config.js

```
kubectl get configmap foobar -o yaml

kind: ConfigMap
apiVersion: v1
metadata:
    name: foobar
data:
    config.js: |  <<
        {
```

ConfigMpas can be consumed in various ways.
- Pod environmental variables from single or multiple ConfigMaps
- Populate Volume from ConfigMap
- Add ConfigMap data to specific path in Volume
- Set file names and access mode in Volume from ConfigMap data
- Can be used by system components and controllers.

## Using ConfigMaps

You can use ConfigMaps as environmental variables or using a volume mount.

In the case of environment variables, your pod manifest will use the valueFrom key and the configMapKeyRef value to read the values

```
env:
- name: SPECIAL_LEVEL_KEY
  valueFrom:
    configMapKeyRef:
      name: special-config
      key: special.how
```

with volumes, you define a volume with the configMap type in your pod and mount it where it needs to be used.

```
volumes:
    - name: config-volume
       configMap:
         name: special-config
```

## LAB
1. https://lms.quickstart.com/custom/858487/LAB_9.1.p
2. https://lms.quickstart.com/custom/858487/LAB_9.2.pdf
3. https://lms.quickstart.com/custom/858487/LAB_9.3.pdf
4. https://lms.quickstart.com/custom/858487/LAB_9.4.pdf

# Chapter 10 ingress

- LoadBalancer: route traffic based on the request host or path.
- Ingress Controller
    - it does not run as part of the kube-controller-manager binary.
    - deploy multiple controllers, each with unique configurations
    - A controller uses Ingress Rules to handle traffic to and from outside the cluster.
    - Theare are two supported controller: GCE, nginx.HAProxy, Traefix.io

## nginx

ingress-nginx
- https://training.linuxfoundation.org/cas?destination=portal
- Customization via ConfigMap, Annnotations, a Custom Template
- Comfig Template
    - Easy integration with RBAC
    - uses the annotation kubernetes.io/ingress.class: "nginx"
    - L7 traffic : proxy-real-ip-cidr
    - Bypasses kube-proxy to allow session affinity
    - Does not use conntrack entries for iptables DNAT
    - TLS requires the host field to be defined

## Google Load Balancer Controller(GLBC)

GCE Ingress Controller
- YAML file to make the process easy

GLBC Controller
- must create a ReplicationController with a single replica, 3 services for the application Pod, an Ingless with 2 hostnames and 3 endpoints for each services.

The multi-pool path
- Global Forwarding Rule -> Target HTTP Proxy -> URL map -> Backend Service -> Instance Group

TLS Ingress
- only supports port 443 and assumes TLS termination, currently.
- no suport SNI, only using the first Cetificate.
- The TLS secret must contain keys named tls.crt and tls.key

## Ingress API Resources

Ingress objects are Deployments and ReplicaSet.

POST to the API server

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: ghost
spec:
rules:
    - host: ghost.192.168.99.100.nip.io
http:
paths:
    - backend:
        serviceName: ghost
        servicePort: 2368
```

manage ingress resources

```
kubectl get ingress

No resources found.
```

```
kubectl delete ingress <ingress_name>
```

```
kubectl edit ingress <ingress_name>
```

## Deploying the Ingress Controller

Creating with kubectl.
default HTTP backend which serves 404 pages

```
kubectl create -f backend.yaml
```

```
kubectl get pods,rc,svc (dummy result)

NAME                             READY     STATUS    RESTARTS   AGE
po/default-http-backend-xvep8    1/1       Running   0         4d
po/nginx-ingress-cotroller-fkshm 1/1       Running   0         4d


NAME             TYPE        DESIRED    CURRENT    READY     AGE
rc/default-http-backend      1          1          0         122d


NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
svc/default-http-backend     10.0.0.212      <none>        80/TCP     122d
svc/kubernetes   ClusterIP   10.0.0.1        <none>        443/TCP    124d
```

## Creating as Ingress Rule
Ingress quckly get exposed

```
kubectl run ghost --image=ghost
```

```
kubectl expose deployments ghost --port=2368
```

Deployment exposed and the Ingress rule. (a single rule)

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ghost
spec:
    rules:
    - host: ghost.192.168.99.100.nip.io
      http:
      paths:
      - backend:
            serviceName: ghost
            servicePort: 2368
```

## Multiple Rules

```
rules:
- host: ghost.192.168.99.100.nip.io
    http:
    paths:
    - backend:
        serviceName: ghost
        servicePort: 2368
- host: ghost.192.168.99.100.nip.io
    http:s
    paths:
    - backend:
        serviceName: nginx
        servicePort: 80
```

# Chapter 11 Scheduling

## kube-scheduler
kube-scheduler
- determins which nodes will run a Pod
- using a topology-aware algorithm.
- Scheduler tracks the set of nodes in your cluster, filter
- uses priority functions to determin
- Send to the kubelet on the node for creation.

Default sheduling
- decition can be affected through the use of "Labels" on node or Pods.
- Labels of "podAffinity(類似)", "taints(汚染)", "pod bindings" allow for configuration from the Pod/Nodes
- "Tolerations(忍耐)", allow a Pod to work with a node.
- "taint" preclude a Pod being shedule

Evict(強制退去) Pods from a node should the required condition no longer be true
- "RequiredDuringScheduling"
- "RequiredDuringExecution"

## Predicates
"Predicates" to find available nodes, then ranks each node using "Priority function"
- "PodFitsHost"
- "NoDiskConflict"
- "Predicates" are evalueated in a particular and configurable order.

example "predicate"

HostNamePred ( as known HostName )
- filters out nodes that do not match the node name, specified in the pod specification.

PodFitsResources
- make sure that the available CPU and memory can fit the resources required by the Pod.

Policy which can order predicates, give special weights to priorities.

hardPodAffinitySymmetricWeight
- deploys Pods
- exsample: set Pod A to run with Pod B, then Pod B should automatically be run with Pod A.


## Priorities

Priorities
- functions used to weight resources.

"SelectorSpreadPriority" setting.
- ranks nodes based on the number of existing runnning pods.
- will select the node with the least amount of Pods.
- This is a basic way to spead Pods across the cluster.

"ImageLocalityPriorityMap"
- favors nodes which aleady have downloaded container images.
- The total sum of image size is compared with the largest having the highest priority.
- does not check the images about to be used.

a list of priroties
- master/pkg/scheduler/algorithm/priorities


## Scheduing Policies
The default sheduler contains a number of "predicates" and "priorities".

Scheduler policy file

```
"kind" : "Policy",
"apiVersion" : "v1",
"predicates" : [
        {"name" : "MatchNodeSelector",      "oeder" : 6},
        {"name" : "PodFitsHostPorts",       "oeder" : 2},
        {"name" : "PodFitsResources",       "oeder" : 3},
        {"name" : "NoDiskConflict",         "oeder" : 4},
        {"name" : "PodToleratesNodeTaints", "oeder" : 5},
        {"name" : "PodFitsHost",            "oeder" : 1}
    ],
"priorities" : [
        {"name" : "LeastRequestedPriority",     "weight" : 1},
        {"name" : "BalanceResourceAllocation",  "weight" : 1},
        {"name" : "ServiceSpreadingPriority",   "weight" : 2},
        {"name" : "EqualPriority",              "weight" : 1},
    ],
"hardPodAffinitySymmetricWeight" : 10
}
```

configure a scheduler with this policy
- using the "--policy-config-file" parameter
- using the "--scheduler-name" parameter

Multiple scheduler
- conflict in the Pod allocation.
- the current solutuion for avoid conflict, for the local "kubelet" to return the Pods to the scheduler for reassignment

## Pod Specification

Most scheduling decisions can be made as part of the Pod specification.
A pod specification contain several fields
- nodeName
    - allow a Pod to be assinged to a single/group node with labels
- nodeSelector
    - allow a Pod to be assinged to a single/group node with labels
- affinity / anti-affinity
    - Used to "require" or "prefer" witch nodes is used by the scheduler.
- tolerations
    - "tains" allows a node to be labeled such that Pods would not be scheduled for some reason.
    - A tolerations allows a Pod to ignore the "taint" and be scheduled assuming other requirements are met.
- schedulerName
    - Deploy a custom scheduler


## Specifying the Node Label
"nodeSelector"

```
spec:
    containers:
    - name: redis
      image: redis
    nodeSelector:
      net: fast
```

- Setting the "nodeSelector" tells the sheduler to place the pod on a node that matches the labels.
- All listed selectors must be met, but the node could have more labels.
- any node with a key or "net" set to "fast" would be a candidate for scheduling.

nodeSelector will be deprecated(廃止予定)

## Pod Affinity Rules
a form of affinity(相性)
- communicate a lot or share data

anti-affinity
- For greater fault tolerance, Pods to be as separate as possible.
- interrogate each node and track the label of running Pods.

Pod affinity rurle
- use "In" / "NotIn" / "Exists" / "DoesNotExist"

"requiredDuringSchedulingIgnoredDuringExecution"
- Pod wil nobe scheduled on a node unless following operator is true.
- if the operator changes to become false in the future, the Pod will continue to run. this cloud be senn as a "hard" rule

"preferredDuringSchedulingIgnoredDuringExecution"
- choose a node with the desired setting.
- if no properlu-labbeled nodes are availabe, the Pod will execute anyway.
- This is more of a soft setting, which declares a preference instead of a requirements

"podAffinity"
- the scheduler will try to schedule Pods together.

"podAntiAffinity"
- cause the scheduler to keep Pods on different nodes

"topologyKey"
- allows a general grouping of Pods deployments
- Affinity / inverse anti-affinity will try to run on nodes with the declared topology key and running Pods with a particular label.
- could be any legal key.

"requredDuringScheduling" and admission controller "LimitPodHardAntiAffinityTopoligy" setting => "topologykey" must be set to "kubernetes.io/hostname"

"PrefrredDuringScheduling", an empty "topoligyKey" is assumed to be all, or the combination of kubernetes.io/hostname, failure-domain.beta.kubernetes.io/zone, and failure-domain.beta.kuberntes.io/region

## podAffinity Example
example of "affonity" and "podAffinity". 
- but not required if the label is later removed.
- if this requirement(key:secuirty, value:S1) is not met, the Pod will remain in a "Pending" state.

```
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
        matchExpressions:
        - key: security
          operator: In
          values: - S1
        topologyKey: failure-domain.beta.kubernetes.io/zone
```

## podAffinity Example

Avoid a node with a particular label.
In this case this scheduler will prefer to avoid a node with a key: "security" and valuse:"S2"

```
podAntiAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - weight: 100
    podAffintyTerm
      labelSelector:
        matchExpressions:
        - key: security
          operator: In
          values: - S2
        topologyKey: kubernetes.io/hostname
```

"weight" : 1 - 100
- Scheduler then tries to choose, or avoid the node with the greatest combined value.

## Node Affinity Rules

nodeAffinity(affinity/anti-affinity) is similar and will to replace to nodeSelector.

Node affinity rules
- use operators(演算子), "In" / "NotIn" / "Exists" / "DoesNotExists" / "Gt" / "Lt"
- requiredDuringSchedulingIgnoredDuringExecution
- preferredDuringSchedulingIgnoredDuringExecution
- requiredDuringSchedulingRequiredDuringExecution (planned)

"nodeSelector" は非推奨(deprecatesd)

## Node Affinity Example

```
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        # require a node with a key of "kubernetes.io/e2w-az-name" has value: tx-aus or tx-dal
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/colo-tx-name
            operator: In
            values:
            - tx-aus
            - tx-dal
      preferredDuringSchedulingIgnoredDuringExecution:
      # extra weight to nodes with key:disk-speed with value:fast or quick
      - weight: 1
        preference:
          matchExpressions:
          - key: disk-speed
            operator: In
            values:
            - fast
            - quick
```

## Taints(汚染, 傷)
A node with "taint" will repel(寄せ付けない,はじく) Pod without "tolerations(容認)" for theat tiant.

taint esxpressed
- "key=value:effect"
    - key and value are created by the admin

Handle Pod Scheduling
- NoSchedule
    - The scheduler will not schedule a Pod on this node, unless the Pod has this "toration".
    - Existing Pods continue to run, regardless of tolerations
- PreferNoSchedule
    - The scheduler will aboud using this node, unless there are no untainted nodes for the Pods "toleration". Existing Pods are unaffected.
- NoExecute
    - This taint will cause existing Pods to be evacuated and no future Pods scheduled.
    - Sould an existing Pod have a toleration, it will continue to run.
    - If the Pod "tolerationsSeconds" is set, they will remain for that many seconds, then be evicted(退去).
    - Certian node issues will cause the "kubectl" to add 3000 second "tolerations" to avoid unneccesassy evictions.

if node has multiple "taints", the scheduler ignore those with matching "tolerations". the remaining unignored "taints" have htier typical effect.

"TaintBaseEvictions(立ち退き)"
- alpha feature
- kubelet use taints to rate-limit evictions when the node has problems.

## Tolerations

"tolerations(容認)" on a node are used to schedule Pods on taited nodes.
- easy way to avaid Pods using the node.
- Only those with a particular "toleration" would be scheduled

Tolerations operators
- "Equal"  : default. require a vlue to match
- "Exists" : not be specified. empty key use "Exist", it will tolerate every taint.
- "effect" : all "effects" are matched with the declared key.

```
tolerations:
- key: "server"
  operator: "Equal"
  value: "ap-east"
  effect: "NoExecute"
  tolerationSeconds: 3600
```

- Pod remain on the server with a key:server, value: ap-east for 3600 sec, after the node has been tainted with NoExecute.
- When the time runs out, the Pod will be evicted

## Custom Scheduler

default scheduling mecahnisms(affinity, taints, policies).
You can write your own scheduler.(Go language)
- https://github.com/kubernetes/kubernetes/tree/master/pkg/scheduler

If Pod declares a scheduler and container is not running, Pod would remain in a "Pending" state forever.

The end result of the scheduling process is that a pod gets a "binding" that specifies which node it should run on.

A binding is a K8s API primitive in the "api/v1" group.

You can also run mulitple schedulers simultaneously.

You can view the scheduler and other information with.

```
kubectl get events

kubectl get events
LAST SEEN   FIRST SEEN   COUNT     NAME                        KIND      SUBOBJECT                  TYPE      REASON                         SOURCE                 MESSAGE
16m         6d           46        busybox.152914b23b8ef1f9    Pod       spec.containers{busybox}   Normal    Pulling                        kubelet, minikube      pulling image "busybox"
16m         6d           46        busybox.152914b2d10b40fa    Pod       spec.containers{busybox}   Normal    Pulled                         kubelet, minikube      Successfully pulled image "busybox"
16m         6d           46        busybox.152914b2d23866ef    Pod       spec.containers{busybox}   Normal    Created                        kubelet, minikube      Created container
16m         6d           46        busybox.152914b2d63b8234    Pod       spec.containers{busybox}   Normal    Started                        kubelet, minikube      Started container
6m          6d           2751      minikube.152912a7a6bcdf5d   Node                                 Warning   FailedToStartNodeHealthcheck   kube-proxy, minikube   Failed to start node healthz on 0: listen tcp: address 0: missing port in address
```

# Chapter 12 Logging and Troubleshooting

## Overview

Monitoring tool(not include in K8s): collect key metrics(CPU, memory, disk usage, network bandwidth)
- Heapstear: OSS. using "Helm Chart"

Logging tool: across all the nodes is another featear not part of K8s.
- Fluentd
    - Unifided logginig layer.
    - Aggregated logs

Prometheus
- combines logging, monitoring, alerting
- time-series database

## Basic Troubleshuooting Steps
App amay have a shell you an use.

```
kubectl run busybox --image=busybox --comand sleep 3600
```

```
kubectl exec -ti <busybox_pod> -- /bin/sh
```

```
kubectl logs pod-name
```

Without logs, you may consider deploying a "sidecar" container in Pod to generate and handle logging.

To check is networking, including DNS, firewalls and general connectivity
- using starndard Linux commands and tools.

Security setting
- "RBAC": provides mandatory or discretionary(一任された) access controll in a granular manner
- "SELinux", "AppArmor" are comm issues with network-centric applications.

Summary: trouble shooting
- Errors from the command line
- Pod logs and state of Pods
- Use shell to troubleshoot Pod DNS and network
- Check node logs for errors, make sure there are enough resources allocated.
- RBAC, SELinux or AppArmor for security settings
- API calls to and from contollers to kube-apiserver
- Inter-node network issues, DNS and firewall
- Master server controllers
    - control Pods in pending or error state
    - error in log files
    - sufficient resources


## Monitoring
- Heapster
    - part of the Kubernetes project. https://github.com/kubernetes/heapster
    - used for the Horizontal Pod Autoscaling feature
    - integraed with the Kubernetes dashboard

- Prometheus
    - part of the CNCF.
    - Kubernetes plugin
    - It has several client libraries to collect aplication level metrics.


## Logging Tools

Common logging tool
- Elasticsearch
- Logstash
- Kibana Stak(ELK)

Kubelet in Kubernetes
- writes container logs to local files(via the Docker logging driver)

kubectl logs in k8s
- retrieve these logs

Fluentd
- cluster-wide
- aggregate logs
- part of the CNCF

Fluentd for K8s
- Fluentd agents run on each node via a DaemonSet
- aggregate the logs and feed to Elasticsearch instance to visualization in a Kibana dashboard.


# Chapter 13 Custom Resource Definitions

## Custom Resource of K8s
Once Custom Resouces have been added, the objects can be created and accessed using kubectl, etcd, kube-apiserver

To make a new custom resource part of a declarative API
- Controller or "operator" is an agent that creates and manages one or more instance of a specific stateful app.
- worked with built-in contoller such as "Deployments", "DaeminSets" and other Rearces.

To add custom resource to your Kubernetes
- adding a "Custom Resource Definition(CRD)" to the cluster
    - The easiest, but less flexible
- Use of "Aggregated APIs(AA)"
    - more flexible
    - requires a new API server to be written and added to the cluster
- Use "Custom Resrouce", adding a new object to the cluster
    - distinct from built-in resouce

if use RBAC for authorization, you need to grant(承諾) access to the new "Custom Resource Definition(CRD)" resource and controller

if use "Aggregated APIs(AA)", you can use the same or a different authentication process.


## Custom Resource Difinitions

if you add a new API object and controller
- you can use the exsiting "kube-apiserver" to monitor and contorol the object.
- "Custom Resource Definition(CRD)" will be added to the cluster API path, currently under "apiextentions.k8s.io/v1beta1"

add a new object to the cluster
- exsiting API functionarity can be used. 
- Object must respond to REST request
- have their configuration state validated and stored in the same manner as built-in objects
- 

"Custom Resource Definition(CRD)"
- allows the resouce to be deployed in a namespace, or be available in the entire cluster.
- the YAML file sets this with the "scope:" parameter, which can be set to "Namespaced" or "Cluster"

Prior to K8s v1.8
- resouce type called "ThirdPartyResource(TPR)"
- Deprecated, no longer available.
- All rerouces need to be rebuilt as "Custom Resource Definition(CRD)".

## Configuration Example

```
apiVersion: apiextentions.k8s.io/v1beta1
  # should match the current level of stability

kind: CustomResourceDefinition
  # CRD: the object type being inserted by the kube-apiserver

metadata:
  name: backups.stable.linux.com
    # the name must match the "spec" feild. 
    # the syntax must be <plural name><group>

spec:
  group: stable.linux.com
    # group name wil become part of the REST API
    # under '/apis/<group>/<version>' or '/apis/ stable/v1'
  version: v1
  scope: Namespaced
    # determin if the object exists in a single namespace or is cluster-wide.
  names:
    plural: backups
      # Defines the last part of the API URL, like a 'apis/stable/v1/backups'
    singular: backup
      # represent the name with displayed and make CLI usage easier
    shortNames:
    - bks
      # represent the name with displayed and make CLI usage easier

    kind: Backup
      # camelcased singuler type used in resource manifests
```

## New Object Configuration

```
apiVersion: "stable.linux.com/v1"  # CRD object
kind: BackUp                       # CRD object
metadata:
  name: a-backup-object
spec: # depend on the controller(Without validation)
  timeSpec: "* * * * */5"
  image: linux-backup-image
  replicas: 5
```

## Optional Hooks
Finalaizer: asynchronous pre-delete hook
1. an API "delete" request is recieved
2. "metadata.deletionTimestamp" is updated.
3. The controller then triggers whichever finalizer has been configured.
4. When the finalizer completes, it is removed from the list.
5. The controller continues to complete and remove finalizers until the string is empty.

Finalaizer:

```
metadata:
  finalizers:
  - finalizer.stable.linux.com
```

Validation:

```
validation:
    openAPIV3Schema:
      properties:
        spec:
          properties:
            timeSpec: # must be a string matching a particular pattern
              type: string
              pattern: '^(\d+|\*)(/\d+)?(\s+(\d+|\*)(/\d+)?){4}$'
          replicas:
            type: integer
            minimum: 1
            maximum: 10
```

starting with v1.9 for validation of custom objects via OpenAPI v3 schema.

## Understanding Aggregated API (AA)
- alloows adding additional Kubernetes-type API servers to the cluster.
- a subordinate to kube-apiserver

enable AA
- kube-apiserver to include "--enable-aggregator-routing=true"

## LAB13.1
https://lms.quickstart.com/custom/858487/LAB_13.1.pdf


# Chapter14 Kerbenetes Fededations

## Cluster Federation

Federation: API endpoint, control plane, which accepts API call and passes the calls to member API servers.
- avoiding Cloud Vendor lockin
- Regional & Global scaralibility and hiher availability

deploy federated resouces
- "Federated Services"
- "Federated Ingress"

## Federation Control Plane

```
kubectl,etcd
-> Federation API Server
-> "federation controller manager"
-> etcd, API server in each area
```

Every 40 sec, by default, the nodes "ClusterController" will sync ClusterStates.

## Switching between Clusters
multiple clister in deffierent zone.
- Not federation.
- manually switch endpoint targets for kubectl.
- none of the scalability or HA features.
- allow for quickly changing between clusters.

```
kubectl config use-context sanfrancisco
kubectl get nodes

kubectl config use-context chicago
kubectl get nodes

kubectl --context=paris get nodes
```

each context and authentications keys would be in kubectl configuration file.
- usually, '$HOME/.kube/config'
- authentication: SSL/TLS

## Building a Federation with kubefed

kubefed (federation)
- New feature, starting in v1.6
- a federation must begin with the context to a host cluster.
- values in '~/.kube/config'. 
- 'kubectl config view'

creation of the control plane
- interacts with cluster, local storage, DNS.
- 'kubefed init fellowship'

```
kubefed init fellowship \
    --host-cluster-context=rivendell \
    --dns-provider="google-clouddns" \
    --dns-zone-name="example.com." \
    --api-server-service-type="NodePort" \
    --api-server-advertise-address="10.0.10.20"
```

'kubefed join'
- add cluster to the federation

```
kubefed join gonder \
    --host-cluster-context=rivendell
    --cluster-context=gonder_needs_no_king
    --secret-name=11kingdom
```

## Using Federated Resources

"Federated Ingress"
- Only run on GCE

10 replicas / 5 clusters
-> 2 replicas per 1 cluster

## Federation API Resources

Each of the Kubernetes resources may have slight diffrecence used with Federation.
- cluster, ConfigMap, DaemonSet, Deployment, events, Horizontal Pod Autoscalers, HPA, Ingress, Jobs, Namespaces, ReplicaSets, Secrets
- use federated server API

namespaces with federation (same as some resources)
- when created with the federated server, will be created on each cluster.
- when removed, they are only removed from the federated API server.
- Each cluster retains the namespace and all objects created within.
- An administrator would need to change the context to each cluster and delete the namespace manually to fully remove it.

## Balancing ReplicaSets

re-balance the replicas
- by default, replicas be spread. you can re-balance
- scheduling mode pods on 1 cluster, and less on another.


Adusting weight
- Kuberntes will re-balance the pods across the federated cluster.
- you can start with even weights, and then use "kubectl apply" to update the values.

```
annotations:
    federation.kubernetes.io/replica-set-prederence: |
      {
          "rebalance": true,
          "clusters": {
              "new-york": {
                  "minReplicas": 0,
                  "maxReplicas": 20,
                  "weight": 10 # if zero, all the replicas would run on other nodes.
              },
              "berlin": {
                  "minReplicas": 1,
                  "maxReplicas": 200,
                  "weight": 20
              }
          }
      }
```

# HELM

## Deploying Complex Applications

HELM: Package Manager for kubernetes
- like yum, apt
- to deploy simple Docker applications
- Starting with the v1.4
- Chart: pakage (= deb, rpm). Manifests template.
- Tiller:  Cluster compornet to deploy. run on each cluster.

you can package all the manifests and make them available as a single tarball.
  - Manifests for deployments, services, ConfigMaps
  - you will create some secrets, Ingress, other objects.

## Tiller and Helm Client

Helm Client on local machine
-> searh from Chart Repositroy
-> init and install by Tiller server on cluster
-> create pods,svc, secrets on cluster

## Chart Cntents

chart : archived set of Kubernetes resouce manifests.
- Chart.yaml : metadata about the Chart (name, version, keyword, ...)
- README.md
- templates : resource manefests that make up applicaion. 
    - NOTES.txt
    - _helpers.tpl
    - configmap.yaml
    - deployment.yaml
    - pvc.yaml
    - secrets.yaml
    - svc.yaml
- values.yaml: keys and values, used to generate the release in the cluster.

## Templates

templates
- resource manigests that use the "Go" templating systax.
- Variables defined in the "values.yaml" file

sample: MariaDB

```
apiVersion: v1
kind: Secret
metadata:
    name: {{ template "fullname" . }}
    labels:
        app: {{ template "fullname" . }}
        chart: {{ .Chart.Name }}--{{ .Chart.Version }}
        release: {{ .Release.Name }}
        heritage: {{ .Release.Service }}
type: Opaque
data:
    mariadb-root-password: {{ default "" .Values.mariadbRootPassword | b64enc | quote }}
    mariadb-password: {{ default "" .Values.mariadbPassword | b64enc | quote }}
```

## Initilizing Helm

```
$ helm init

...
Tiller (the helm server side component) has been installed into your Kubernetes cluster.
Happy Helming!
```

helm initialization should have created a new "tiller-deploy" pod in the cluster.

```
$ kubectl get deployments --namespace=kube-system

NAMESPACE   NAME            ....
kube-system tiller-deploy   ....
```

## Chart Repositories

Repositories are simple HTTP server that contain an index file and a tarball

```
$ hekm repo add testing \
http://storage.googleapis.com/kubernetes-charts-testing


$ helm repo list

NAME    URL
stable  http://....
local   http://....
testing http://....
```

search keyword

```
$ helm search redis

NAME                    VERSION     DESCRIPTION
testing/redis-cluster   0.0.5       ....
teting/redis-standalone 0.0.1       ....
```

## Deploying a Chart

'helm install' : to deploy a Chart( = package)

```
$ helm install testing/redis-standalone
```

```
$ helm list

NAME        REVISION    UPDATED     STATUS      CHART
amber-eel   1           Fri Oct ..  DEPLOYED    redis-standalone-0.0.1
```

# Chapter 16 Security

## Overview

Authorization
- like ABAC, RBAC
- kubeadm
- "admittion control" system

Network Policies
- Fkannel or Calico

## Accessing the API

"kube-apiserver" goes through a 3-step process
1. Authentication (X509 certifacte, a token, LDAP server)
2. Authorization (ABAC or RBAC: Role-Based Access Control)
3. Admission control

## Authentication

Authentication 3 main points
- authenticate with with certificates, tokens or basic authentication(username/password)
- users are not created bythe API. by an external system
- used by access the API
- more advanced authentication mechanisms
    - "Webhooks" to verify bearer tokens
    - connection with an external "OpenID" provider

"kube-apiserver" authentification options
- "--basic-auth-file"
- "--oidc-issuer-url"
- "--token-auth-file"
- "--authorization-webhook-config-file"

## Authorization

main authorization mode. Deny / Allow setting
- ABAC (Atribute-Based Access Control)
- RBAC (Role-Based Access Control)
- WebHook

"kube-apiserver" startup options
- "--ahthorization-mode=ABAC"
- "--ahthorization-mode=RBAC"
- "--ahthorization-mode=Webhook"
- "--ahthorization-mode=AlwaysDeny"
- "--ahthorization-mode=AlwaysAllow"

## ABAC

ABAC: Attribute Based Access Control
- the first authorization mode
- Today, default is "RBAC"

Policies are define a JSON file.
"kube-apiserver"
- "--authorization-policy-file=my_policy.json"

Pplicy file, authorizes user "bob"

```
{
    "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
    "kind": "Policy",
    "spec": {
        "user": "bob",
        "namespace": "foobar",
        "resource": "pods",
        "readonly": true
    }
}
```

## RBAC

RBAC: Role Based Access Control
- writhing rules to allow/deny operations by users, roles or groups

All resources(Pods, Namespaces) belong "API Group".
- allow operations such as Create, Read, Update, Delete(CRUD)
- Operations are called "verbs" inside YAML files
- "Roles" are a group of rule.
- "ClusterRoles" have a scope of the entire cluster.
- "User Accounts", "Service Accounts", "Groups"
    - when using kubectl "clusterrolebinding"

## RBAC Process Overview

RBAC process summary
1. Determin or create namespace
2. Create certificate credentials for user
3. Set the credentials for the user to the namespace using a context
4. Create a role for the expected task set
5. Bind the user to the role
6. Verify the user has limited access

basic flow to create a certificate for a user.
1. require outside authentication, such as a OpenSSL certificates.
2. generate the certificate against the cluster ertificate authority.
3. Using a "context", set that sredential for the user.

Role can then be used to configure
- "apiGroups"
- "resources"
- "verbs"
- the user can then be bound to a role limiting what and where they can work in the cluster.


## Admission Controller
Admission Control: The last Step in letting an API request into Kubernetes.

"kube-apiserver" uses this set of contollers by default
- "--admission-control=Initializers, NamespaceLifecycle, LimitRaanger, \"
  "ServiceAccount, DefaultStorageClass, DefaultTolerationSeconds \"
  "NodeRestricton, ResourceQuota"

"Initializers"
- the first controller
- allow the dynamic modification of the API request
- providing great flexibility

"ResourceQuota" cotroller
- the one of admission controller functionality.
- ensure that the object created does not violate any of exisiting quotas.

## Security Contexts
"Security context" = The security limitation
- example
    - UID of ghe process
    - Linux capabilities
    - filesystem group

Enforce a policy that contianers cannot run theier process

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  securityContext:
    runAsNonRoot: true
  containers:
  - image: nginx
    name: nginx

```

It it not allowed. Hence, the Pod will never run.

```
$ kubectl get pods

NAME    READY   STATUS
nginx   0/1     container has runAsNoRoot and image will run as root
```

## Pod Security Policies

"PodSecurityPolicies(PSP)"
- To automate the enforcement of "security context"
- A PSP is defined via k8s manifest following the PSP API schema

PSP example
- need to configure the admission controller of the "controller-manager" to contain "PodSecurityPolity"


```
apiVersion: extensions/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: MustRunAsNonRoot
  fsGroup:
    rule: RunAsAny
```

## Network Security Policies

"NetworkPolicy"
- ingree/egress traffic can be allowed / dropped.
- "spec" : narrow down the effect to a particular namespace
- "pddSelector" or label : narrow down the Pods
- ingress/egress setting the traffic to/from IP address/ports.

"NetworkPolicies"
- network providers support. (Not all providers)
- 'Calico', 'Romana', 'Cilium', 'kube-router', 'WeaveNet'

## Network Security Policy Example

narrow down the policy to affect the default namespace.

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
   name: ingress-egress-policy
   namespace: default
spec:
  podSelectpr:
    matchLabels:
      role: db  # only pods with the labake of "role: db"

  policyTypes:  # affect by this policy, both Ingress and Egress 
  - Ingress
  - Egress

  ingress:  # 'ingress' includes a 172.17 network. 
  - from:
    - ipBlock:
      cidr: 172.17.0.0/16
      except:
      - 172.17.1.0/24  # 172.17.1.0 IPs beinig excluded from this traffic. 
  
    - namepaceSlector:
        matchLabels:
            project: myproject

    - podSelector:
        matchLabels:
            role: frontend  # need to match label "role: frontend"

    ports:
    - protocol: TCP
        port: 6379  # allow TCP traffic on port 6379 from these Pods

  egress:
  # allow 10.0.0.0/24 range TCP traffic on port 5978 to these Pods

  - to:
    - ipBlock:
        cider: 10.0.0.0/24

    ports:
    - protocol: TCP
      port: 5978  
```

empty ingree/egress rule
- deny all type of traffic for the Pods.
- Not suggeested.


'matchExpressions' statement

```
podSelector:
  matchExpressions:
```

## Default Policy Example

default = empty {}
- not allow ingress traffic
- Egress traffic unaffected by this policy

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyType:
  - Ingress
```

Some plugin, such as "WeaveNet" 
- require annotation of the Namespace
- "DefaultDeny" for the 'myns' namespace:

```
kind: Namespace
apiVersion: v1
metadata:
  name: myns
  annotations:
    net.beta.kubernetes.io/network-policy: |
    {
        "ingress": {
            "isolation": "DefaultDeny"
        }
    }
```


# Others
Resource
- https://training.linuxfoundation.org/cm/LFS258/
Discussion Board
- https://www.linux.com/forums/lfs258-class-forum


# TODO
- suevery Network namespace

# Keywords
## kubadm
- a tool built to provide as best-practice "fast paths" for creating k8s cluster
- networking

## Hyperkube
- hyperkube is an all-in-one binary for the Kubernetes server components
- https://github.com/kubernetes/kubernetes/tree/master/cluster/images/hyperkube

# mypage
https://training.linuxfoundation.org/cas?destination=portal
