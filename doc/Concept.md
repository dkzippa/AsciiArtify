# AsciiArtify

## Comparison of k8s tools for local development

#### Minikube
- Single-node Kubernetes cluster simulator on local machine. 
  Provides a convenient and accessible way for developers and learners to work with Kubernetes in a local environment.
  Best for individuals new to Kubernetes, needing a comprehensive environment.
- **Pros**: 
	- Supports different virtualization technologies
	- Has addons
	- Can mount code from the host directly into the running Kubernetes node
	- `--kubernetes-version` can be chosen easily
- **Cons**:
	- Limited features: Minikube is a simplified version of Kubernetes
	- Not a production-grade solution, does not provide high availability, security, and robust monitoring and logging
	- Too minimal
- **License**: 
	- *Apache License 2.0 (+Commercial use, +Modification, +Distribution, +Patent use, +Private use)*

#### Kind(Kubernetes in Docker)
- Designed for testing Kubernetes itself, but may be used for local development or CI. 
  Allows to run Kubernetes clusters using Docker containers as nodes.
  Ideal for CI/CD pipelines and testing Kubernetes features.
- **Pros**: 
	- Runs a kubeadm cluster inside a docker environment.
	- Maintained by the Kubernetes team
	- Sharing cluster configurations
- **Cons**:
	- Cannot mount code from the host directly into the running Kubernetes node
	- More complex to set up and use compared to competitors
	- Network external access to the cluster more complicated
	- No addons
- **License**: 
	- *Apache License 2.0 (+Commercial use, +Modification, +Distribution, +Patent use, +Private use)*

#### k3d
- Lightweight CNCF-certified Kubernetes distribution.
  Designed for edge computing and IoT devices.
  Can do everything required of a standard Kubernetes cluster.
  Recommended for lightweight, quick, and iterative development environments.

- **Pros**: 
	- Single binary (<70MB) that reduces the dependencies and steps needed to install, run and auto-update a production Kubernetes cluster.
	- Faster than Minikube and has a smaller footprint.
	- Has good support for GitHub Actions
	- Sharing cluster configurations
	- Can mount code from the host directly into the running Kubernetes node
	- Hot reloading capabilities
	- Doesn’t have any dependencies
- **Cons**:
	- No distributed database by default, uses SQLite
	- Own networking stack (flannel) 
	- No addons
- **License**: 
	- *MIT License (+Commercial use, +Modification, +Distribution, +Private use)*  

## Features compared (minikube style): 

|   |   [minikube](https://minikube.sigs.k8s.io/docs/)  |   [kind](https://kind.sigs.k8s.io/)   |   [k3d](https://k3d.io/v5.6.0/)         |
| --------------------------------- | :-------------------------------: | :-------------------: | :-----------------: |
|   Supported OS and Architectures    |   linux, macos, widnows. <br />arm, armv5, armv6, arm64, amd64, 386                |          linux, macos, widnows. <br />arm, armv5, armv6, arm64, amd64, 386          |        linux, macos, widnows. <br />arm, armv5, armv6, arm64, amd64, 386       |
| Drivers, providers                |                docker, podman, qemu, virtualbox, etc                |          docker, podman         |         docker, podman         |
| Runtimes                |                containerd, docker, cri-o                 |          docker         |         docker, containerd, NVIDIA Container Runtime         |
| Hardware requirements                |              2+ CPUs<br/>2GB+ RAM<br/>20GB+ storage                  |          6GB+ RAM for VM with Docker on MacOS or Windows         |        512MB RAM (server)<br/> 75MB RAM (node)<br/>200MB storage         |
| Configuration and usage                |                very easy and simple                |         moderate         |        simple and easy        |
| Multi-node cluster                 |                ✔                |          ✔          |         ✔         |
| Several clusters on one host               |                minikube profiles                |          ✔          |         ✔         |
| Scalability                |                ⭐️                |          ⭐️          |         ⭐️⭐️⭐️         |
| CI Platforms support(+GitHub Actions)                |                ⭐️⭐️⭐️⭐️⭐️                |          ⭐️          |         ⭐️⭐️⭐️         |
| Documentation and community                |                ⭐️⭐️⭐️⭐️⭐️                |          ⭐️⭐️⭐️          |         ⭐️⭐️⭐️⭐️⭐️         |
| Specific IDE plugins                |                🚫 <br/>uses Kubernetes one               |          ✔          |         ✔         |




## Demo of hellow-world image deployment in k3d cluster
![Image](concept_demo.gif)

```
alias k=kubectl

curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

k3d --version
docker ps -a

k3d cluster create asciiartify --servers 1 --agents 3
k3d cluster list
k3d node list
docker ps -a
k cluster-info

k create deployment hello-world --image=gcr.io/google-samples/hello-app:2.0
k get all -A
k get po
k get events

k logs -f $(k get po| tail -n 1 | awk {'print $1'})

kubectl expose deployment hello-world --type=LoadBalancer --port=8080
k get svc
kubectl port-forward deployments/hello-world 8080&
curl localhost:8080

k3d cluster stop asciiartify
k3d cluster detete asciiartify
```


## Conclusions and recommendations:

After deep investigation and exploration, I recommend k3d for AsciiArtify. Due to its small size and high availability, K3s is ideal for creating production-grade Kubernetes clusters in local/remote dev environments. k3d allows to easily create single and multi-node k3s clusters for seamless local development and testing of Kubernetes applications while enabling easy scaling of workloads. It also provides simple commands that ease the management. 

#### Also, considering Docker licensing restrictions, Podman can be used as a drop-in replacement for Docker, as it has full support in k3d.


