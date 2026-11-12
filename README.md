Distributed Kubernetes Cluster Deployment on OpenStack
Project Overview
Deployed a distributed Kubernetes cluster on OpenStack using four virtual machines: one control-plane node and three worker nodes.

The cluster supports containerized application deployment, networking, persistent storage, dashboard access, and remote management of assigned Kubernetes namespaces.
Cluster Configuration
- Infrastructure: OpenStack
- Number of nodes: 4
- Cluster topology: 1 control-plane node and 3 worker nodes
- Resources per VM: 6 vCPUs and 24 GB RAM
- Operating system: Ubuntu Server 22.04.5 LTS
- Kubernetes version: v1.30.14
- Cluster installation: kubeadm
- Container runtime: containerd 2.2.1
- Network plugin: Calico v3.28.0
- Pod network: 10.244.0.0/16
Implementation
1. Provisioned four Ubuntu virtual machines on OpenStack.
2. Installed and configured containerd, kubeadm, kubelet, and kubectl.
3. Initialized the Kubernetes control plane and joined three worker nodes.
4. Configured Calico for pod networking.
5. Installed NGINX Ingress Controller for application routing.
6. Configured persistent storage using Rancher local-path-provisioner.
7. Deployed applications including Chatwoot and Coder.
8. Deployed the Kubernetes Dashboard.
9. Configured Cloudflare Tunnel to provide remote access to the Kubernetes API endpoint.
Kubernetes Dashboard
Dashboard URL: https://kubernetes.openstacklab.me

The dashboard provides a web interface for viewing and managing Kubernetes resources, subject to the permissions of the account being used.
Remote Kubernetes Access
Configured Cloudflare Tunnel to make the Kubernetes API endpoint remotely reachable.

Users can connect from their own computers using Windows Command Prompt or PowerShell with kubectl and an individual kubeconfig file.

Namespace-scoped Kubernetes RBAC can be used to restrict each user's access to their assigned namespace. Users do not need SSH access to the Kubernetes virtual machines to use kubectl.
Deployed Applications
- Chatwoot: Deployed with Web, Sidekiq, PostgreSQL, and Redis components.
- Coder: Deployed within the Kubernetes cluster.
- Kubernetes Dashboard: Provides a web-based interface for cluster resources.
Self-Healing Test
Tested workload self-healing by deleting a running Chatwoot Web pod.

Kubernetes detected that the required pod was missing and created a replacement through the ReplicaSet. The application recovered after approximately 4–5 seconds.

This test demonstrates pod recovery. It does not mean that the application guarantees zero downtime.
Challenges and Troubleshooting
- Resolved DNS and outbound connectivity issues affecting image pulls.
- Adjusted network MTU to account for VXLAN overhead.
- Troubleshot OpenStack routing, forwarding, and NAT.
- Verified container runtime connectivity before deploying workloads.
High Availability
The cluster has one control-plane node and three worker nodes. Pod recovery was tested, but the control plane is not highly available. Failure of the control-plane node can affect cluster management and scheduling.
Technologies Used
OpenStack, Kubernetes, kubeadm, kubectl, kubelet, containerd, Calico, Ubuntu Server, NGINX Ingress Controller, local-path-provisioner, Cloudflare Tunnel, and Kubernetes RBAC.
