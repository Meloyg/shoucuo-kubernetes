# Kubernetes Examples

This directory contains practical YAML examples for various Kubernetes resources.

## Available Examples

### Basic Resources
- [01-simple-pod.yaml](01-simple-pod.yaml) - Basic Pod definition
- [02-deployment.yaml](02-deployment.yaml) - Deployment with multiple replicas
- [03-service.yaml](03-service.yaml) - ClusterIP Service
- [04-nodeport-service.yaml](04-nodeport-service.yaml) - NodePort Service

### Configuration
- [05-configmap.yaml](05-configmap.yaml) - ConfigMap example
- [06-secret.yaml](06-secret.yaml) - Secret example
- [07-pod-with-configmap.yaml](07-pod-with-configmap.yaml) - Pod using ConfigMap

### Storage
- [08-persistent-volume.yaml](08-persistent-volume.yaml) - PersistentVolume
- [09-persistent-volume-claim.yaml](09-persistent-volume-claim.yaml) - PersistentVolumeClaim
- [10-pod-with-volume.yaml](10-pod-with-volume.yaml) - Pod using PVC

### Advanced
- [11-multi-container-pod.yaml](11-multi-container-pod.yaml) - Pod with multiple containers
- [12-deployment-with-resources.yaml](12-deployment-with-resources.yaml) - Deployment with resource limits

## Usage

To apply any example:

```bash
kubectl apply -f <filename>.yaml
```

To delete:

```bash
kubectl delete -f <filename>.yaml
```

## Learning Path

1. Start with simple Pod examples
2. Move to Deployments and Services
3. Learn about ConfigMaps and Secrets
4. Explore storage with PersistentVolumes
5. Try advanced multi-container patterns
