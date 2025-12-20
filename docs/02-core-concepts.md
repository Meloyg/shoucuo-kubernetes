# Kubernetes Core Concepts

## Cluster

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

## Node

A Node is a worker machine in Kubernetes (can be virtual or physical). Each node contains the services necessary to run Pods and is managed by the control plane.

### Types of Nodes:
- **Master Node (Control Plane)**: Manages the cluster
- **Worker Node**: Runs the actual applications

## Pod

A Pod is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in your cluster.

### Key Points:
- A Pod can contain one or more containers
- Containers in a Pod share the same network namespace
- Each Pod gets its own IP address
- Pods are ephemeral - they are created and destroyed dynamically

### Example Pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

## Deployment

A Deployment provides declarative updates for Pods and ReplicaSets. It manages the deployment and scaling of a set of Pods.

### Key Features:
- Maintains desired state of applications
- Rolling updates and rollbacks
- Scaling up or down
- Self-healing capabilities

### Example Deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
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
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## Service

A Service is an abstract way to expose an application running on a set of Pods as a network service.

### Types of Services:

1. **ClusterIP** (default)
   - Exposes the Service on an internal IP in the cluster
   - Only accessible from within the cluster

2. **NodePort**
   - Exposes the Service on each Node's IP at a static port
   - Accessible from outside the cluster

3. **LoadBalancer**
   - Exposes the Service externally using a cloud provider's load balancer

4. **ExternalName**
   - Maps the Service to a DNS name

### Example Service:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

## Namespace

Namespaces provide a mechanism for isolating groups of resources within a single cluster.

### Common Namespaces:
- `default`: The default namespace for objects with no other namespace
- `kube-system`: For objects created by the Kubernetes system
- `kube-public`: Publicly accessible namespace
- `kube-node-lease`: For node heartbeat data

## ReplicaSet

A ReplicaSet ensures that a specified number of pod replicas are running at any given time.

### Key Points:
- Maintains stable set of replica Pods
- Usually managed by Deployments
- Ensures specified number of identical Pods are running

## ConfigMap

ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable.

### Use Cases:
- Store configuration files
- Store command-line arguments
- Store environment variables

### Example ConfigMap:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_url: "mongodb://localhost:27017"
  app_mode: "production"
```

## Secret

Secrets are used to store sensitive information, such as passwords, OAuth tokens, and SSH keys.

### Example Secret:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=  # base64 encoded
  password: cGFzc3dvcmQ=  # base64 encoded
```

## Volume

Volumes provide persistent storage to containers. Unlike container storage, volumes persist beyond the container lifecycle.

### Common Volume Types:
- `emptyDir`: Temporary storage that exists as long as Pod exists
- `hostPath`: Mounts a file or directory from the host node
- `persistentVolumeClaim`: Claims storage from a PersistentVolume
- `configMap` / `secret`: Mounts ConfigMaps and Secrets as volumes

## Labels and Selectors

Labels are key-value pairs attached to objects (like Pods). Selectors allow you to identify a set of objects.

### Example:
```yaml
metadata:
  labels:
    app: nginx
    environment: production
    tier: frontend
```

## Annotations

Annotations are used to attach arbitrary non-identifying metadata to objects.

### Use Cases:
- Build/release timestamps
- Contact information
- Tool-specific configuration

## Next Steps

- Learn about [Kubernetes Architecture](03-architecture.md)
- Explore [Common Commands](04-common-commands.md)
- Practice with [Examples](../examples/)
