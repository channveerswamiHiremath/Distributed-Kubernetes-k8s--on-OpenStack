
# Distributed Kubernetes Cluster Deployment on OpenStack

## Project Overview

Deployed a distributed Kubernetes cluster on OpenStack using four virtual machines: one control-plane node and three worker nodes.

The cluster supports containerized application deployment, networking, persistent storage, dashboard access, and remote management of assigned Kubernetes namespaces.

## Cluster Architecture

```text
             OpenStack Infrastructure
                      |
          -------------------------
          |           |           |
       Control     Worker 1    Worker 2
        Plane                    |
          |                   Worker 3
          |
     Kubernetes
        Cluster
          |
   -------------------
   |        |        |
Chatwoot  Coder  Dashboard
```

## Cluster Configuration

| Component | Configuration |
|---|---|
| Infrastructure | OpenStack |
| Number of Nodes | 4 |
| Cluster Topology | 1 Control Plane + 3 Workers |
| Resources per VM | 6 vCPUs, 24 GB RAM |
| Operating System | Ubuntu Server 22.04.5 LTS |
| Kubernetes | v1.30.14 |
| Installation Tool | kubeadm |
| Container Runtime | containerd 2.2.1 |
| Network Plugin | Calico v3.28.0 |
| Pod Network | 10.244.0.0/16 |

## Technologies Used

- **Infrastructure:** OpenStack
- **Orchestration:** Kubernetes
- **Cluster Setup:** kubeadm
- **Container Runtime:** containerd
- **Networking:** Calico
- **Ingress:** NGINX Ingress Controller
- **Storage:** Rancher local-path-provisioner
- **Remote Access:** Cloudflare Tunnel
- **Operating System:** Ubuntu Server
- **CLI:** kubectl
- **Access Control:** Kubernetes RBAC

## Implementation

### 1. Infrastructure Provisioning

- Provisioned four Ubuntu virtual machines on OpenStack.
- Configured one control-plane node and three worker nodes.
- Allocated 6 vCPUs and 24 GB RAM to each VM.

### 2. Kubernetes Cluster Setup

- Installed and configured containerd.
- Installed kubeadm, kubelet, and kubectl.
- Initialized the Kubernetes control plane.
- Joined three worker nodes to the cluster.

### 3. Networking and Storage

- Configured Calico for pod networking.
- Installed NGINX Ingress Controller for application routing.
- Configured persistent storage using Rancher local-path-provisioner.

### 4. Application Deployment

Deployed the following applications:

- **Chatwoot:** Deployed with Web, Sidekiq, PostgreSQL, and Redis components.
- **Coder:** Deployed within the Kubernetes cluster.
- **Kubernetes Dashboard:** Provides a web-based interface for managing Kubernetes resources.

## Kubernetes Dashboard

The Kubernetes Dashboard provides a graphical interface to view and manage cluster resources, subject to the permissions of the account being used.

**Dashboard URL:** https://kubernetes.openstacklab.me

## Remote Kubernetes API Access

Configured Cloudflare Tunnel to make the Kubernetes API endpoint remotely accessible.

This enables users to connect to the cluster from their own computers using `kubectl` through Windows Command Prompt or PowerShell.

Key capabilities:

- Remote Kubernetes API access through Cloudflare Tunnel.
- Access using individual kubeconfig credentials.
- Namespace-level access control using Kubernetes RBAC.
- Users can manage their assigned namespaces without SSH access to the Kubernetes virtual machines.

## Self-Healing Validation

Tested Kubernetes workload self-healing by deleting a running Chatwoot Web pod.

- Kubernetes detected that the required pod was missing.
- The ReplicaSet created a replacement pod.
- The application recovered after approximately 4–5 seconds.

This validated automatic pod recreation and workload recovery. It does not guarantee zero downtime.

## Challenges and Troubleshooting

- Resolved DNS and outbound connectivity issues affecting container image pulls.
- Adjusted network MTU to account for VXLAN overhead.
- Troubleshot OpenStack routing, forwarding, and NAT.
- Verified container runtime connectivity before deploying workloads.

## High Availability Considerations

The cluster consists of one control-plane node and three worker nodes.

Workload-level pod recovery was tested successfully. However, the control plane is not highly available. Failure of the control-plane node can affect cluster management and scheduling.

## Key Outcomes

- Successfully deployed a four-node Kubernetes cluster on OpenStack.
- Configured container networking, ingress, and persistent storage.
- Deployed and managed containerized applications.
- Provided a web-based Kubernetes Dashboard.
- Enabled remote Kubernetes API access through Cloudflare Tunnel.
- Established namespace-level access management using Kubernetes RBAC.
- Validated automatic pod recreation through a self-healing test.

