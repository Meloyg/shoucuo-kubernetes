# Kubernetes Architecture

## Overview

Kubernetes follows a master-worker architecture with multiple components working together to manage containerized applications.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Control Plane (Master)                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  API Server  │  │   Scheduler  │  │  Controller  │  │
│  └──────────────┘  └──────────────┘  │   Manager    │  │
│  ┌──────────────┐                     └──────────────┘  │
│  │     etcd     │                                        │
│  └──────────────┘                                        │
└─────────────────────────────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
┌───────▼──────┐  ┌───────▼──────┐  ┌──────▼───────┐
│ Worker Node 1 │  │ Worker Node 2 │  │ Worker Node 3│
│ ┌──────────┐ │  │ ┌──────────┐ │  │ ┌──────────┐ │
│ │  Kubelet │ │  │ │  Kubelet │ │  │ │  Kubelet │ │
│ └──────────┘ │  │ └──────────┘ │  │ └──────────┘ │
│ ┌──────────┐ │  │ ┌──────────┐ │  │ ┌──────────┐ │
│ │Kube-proxy│ │  │ │Kube-proxy│ │  │ │Kube-proxy│ │
│ └──────────┘ │  │ └──────────┘ │  │ └──────────┘ │
│ ┌──────────┐ │  │ ┌──────────┐ │  │ ┌──────────┐ │
│ │Container │ │  │ │Container │ │  │ │Container │ │
│ │ Runtime  │ │  │ │ Runtime  │  │  │ │ Runtime  │ │
│ └──────────┘ │  │ └──────────┘ │  │ └──────────┘ │
│              │  │              │  │              │
│   [Pods]     │  │   [Pods]     │  │   [Pods]     │
└──────────────┘  └──────────────┘  └──────────────┘
```

## Control Plane Components

The Control Plane (Master Node) makes global decisions about the cluster and detects/responds to cluster events.

### 1. API Server (kube-apiserver)

**Role**: The front-end for the Kubernetes control plane

**Responsibilities**:
- Exposes the Kubernetes API
- Handles REST operations
- Validates and processes API requests
- Updates etcd with cluster state
- Acts as the central communication hub

**Key Points**:
- All components communicate through the API server
- Horizontally scalable
- Stateless (state stored in etcd)

### 2. etcd

**Role**: Consistent and highly-available key-value store

**Responsibilities**:
- Stores all cluster data
- Acts as the source of truth for cluster state
- Stores configuration data, state data, and metadata

**Key Points**:
- Critical component - requires backup strategy
- Distributed consensus using Raft algorithm
- Only API server directly communicates with etcd

### 3. Scheduler (kube-scheduler)

**Role**: Watches for newly created Pods and assigns them to nodes

**Responsibilities**:
- Selects optimal node for Pod placement
- Considers resource requirements, constraints, and policies
- Performs feasibility checks and scoring

**Scheduling Factors**:
- Resource requirements (CPU, memory)
- Hardware/software policy constraints
- Affinity and anti-affinity specifications
- Data locality
- Taints and tolerations

### 4. Controller Manager (kube-controller-manager)

**Role**: Runs controller processes that regulate cluster state

**Key Controllers**:
- **Node Controller**: Monitors node health
- **Replication Controller**: Maintains correct number of pods
- **Endpoints Controller**: Populates Endpoints objects
- **Service Account & Token Controllers**: Create default accounts and API access tokens

**Responsibilities**:
- Watch cluster state via API server
- Make changes to move current state toward desired state
- Implement control loops

### 5. Cloud Controller Manager

**Role**: Embeds cloud-specific control logic

**Responsibilities**:
- Node management (add/remove based on cloud provider)
- Route management
- Load balancer management
- Volume management

## Worker Node Components

Worker nodes run containerized applications and are managed by the control plane.

### 1. Kubelet

**Role**: Primary node agent that runs on each worker node

**Responsibilities**:
- Registers node with API server
- Monitors Pods assigned to its node
- Ensures containers are running and healthy
- Reports node and Pod status to API server
- Executes container lifecycle hooks

**Key Points**:
- Communicates with API server
- Uses container runtime to manage containers
- Performs health checks (liveness and readiness probes)

### 2. Kube-proxy

**Role**: Network proxy that maintains network rules on nodes

**Responsibilities**:
- Implements Services abstraction
- Manages network rules for Pod communication
- Forwards traffic to appropriate Pods
- Performs load balancing

**Modes**:
- **iptables mode**: Uses iptables rules for packet forwarding
- **IPVS mode**: Uses IP Virtual Server for better performance
- **userspace mode**: Legacy mode

### 3. Container Runtime

**Role**: Software responsible for running containers

**Supported Runtimes**:
- **containerd**: Industry-standard container runtime
- **CRI-O**: Lightweight container runtime for Kubernetes
- **Docker**: (via dockershim, deprecated in newer versions)

**Responsibilities**:
- Pulling container images
- Running containers
- Managing container lifecycle

## Add-ons

Add-ons are additional components that provide cluster-level features.

### Common Add-ons:

1. **DNS (CoreDNS)**
   - Provides DNS service for cluster
   - All Services get DNS names

2. **Dashboard**
   - Web-based UI for cluster management
   - Deploy, troubleshoot applications

3. **Monitoring (Metrics Server)**
   - Collects resource metrics
   - Enables horizontal pod autoscaling

4. **Logging**
   - Centralized logging for containers
   - Examples: Fluentd, Elasticsearch, Kibana (EFK stack)

5. **Ingress Controller**
   - Manages external access to services
   - Provides HTTP/HTTPS routing

## Communication Flow

### Pod Creation Flow:

1. User submits Pod specification via `kubectl` to API server
2. API server validates request and stores in etcd
3. Scheduler watches for unscheduled Pods
4. Scheduler selects appropriate node and updates API server
5. Kubelet on selected node watches API server
6. Kubelet instructs container runtime to pull image and start container
7. Kubelet reports Pod status back to API server
8. API server updates etcd with current state

### Service Request Flow:

1. Client makes request to Service
2. Kube-proxy intercepts the request
3. Kube-proxy routes to appropriate Pod backend
4. Pod processes request and returns response

## High Availability

For production environments, Kubernetes components should be highly available:

- **Multiple master nodes**: 3 or 5 masters for quorum
- **etcd cluster**: Replicated across multiple nodes
- **Load balancing**: Distribute API server traffic
- **Multiple worker nodes**: Distribute workload

## Security Considerations

1. **RBAC**: Role-Based Access Control for API authorization
2. **Network Policies**: Control traffic between Pods
3. **Secrets Management**: Encrypt secrets at rest
4. **Pod Security Policies**: Define security constraints
5. **API Server Authentication**: Multiple authentication methods

## Next Steps

- Review [Common Commands](04-common-commands.md)
- Practice with [Examples](../examples/)
- Explore official Kubernetes documentation for deeper understanding
