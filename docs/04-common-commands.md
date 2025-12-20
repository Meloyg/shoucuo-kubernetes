# Kubernetes Common Commands

This document contains commonly used `kubectl` commands for managing Kubernetes clusters and resources.

## Cluster Information

```bash
# Get cluster information
kubectl cluster-info

# View cluster details
kubectl cluster-info dump

# Get list of nodes in the cluster
kubectl get nodes

# Show detailed information about a node
kubectl describe node <node-name>

# Check the version of kubectl and cluster
kubectl version

# Display cluster configuration
kubectl config view

# Get current context
kubectl config current-context

# List all contexts
kubectl config get-contexts

# Switch context
kubectl config use-context <context-name>
```

## Working with Pods

```bash
# List all pods in current namespace
kubectl get pods

# List all pods in all namespaces
kubectl get pods --all-namespaces
kubectl get pods -A

# Get detailed information about a pod
kubectl describe pod <pod-name>

# Get pod logs
kubectl logs <pod-name>

# Get logs from a specific container in a pod
kubectl logs <pod-name> -c <container-name>

# Follow logs in real-time
kubectl logs -f <pod-name>

# Execute command in a pod
kubectl exec <pod-name> -- <command>

# Get an interactive shell in a pod
kubectl exec -it <pod-name> -- /bin/bash

# Delete a pod
kubectl delete pod <pod-name>

# Force delete a pod
kubectl delete pod <pod-name> --force --grace-period=0

# Port forward to access pod locally
kubectl port-forward <pod-name> <local-port>:<pod-port>
```

## Working with Deployments

```bash
# List all deployments
kubectl get deployments

# Create a deployment from a YAML file
kubectl apply -f deployment.yaml

# Create a deployment imperatively
kubectl create deployment <name> --image=<image-name>

# Get detailed information about a deployment
kubectl describe deployment <deployment-name>

# Update deployment image
kubectl set image deployment/<deployment-name> <container-name>=<new-image>

# Scale a deployment
kubectl scale deployment <deployment-name> --replicas=<number>

# Autoscale a deployment
kubectl autoscale deployment <deployment-name> --min=<min> --max=<max> --cpu-percent=<percentage>

# Check rollout status
kubectl rollout status deployment/<deployment-name>

# View rollout history
kubectl rollout history deployment/<deployment-name>

# Rollback to previous version
kubectl rollout undo deployment/<deployment-name>

# Rollback to specific revision
kubectl rollout undo deployment/<deployment-name> --to-revision=<revision-number>

# Delete a deployment
kubectl delete deployment <deployment-name>
```

## Working with Services

```bash
# List all services
kubectl get services
kubectl get svc

# Create a service
kubectl apply -f service.yaml

# Expose a deployment as a service
kubectl expose deployment <deployment-name> --port=<port> --type=<service-type>

# Get detailed information about a service
kubectl describe service <service-name>

# Delete a service
kubectl delete service <service-name>
```

## Working with Namespaces

```bash
# List all namespaces
kubectl get namespaces
kubectl get ns

# Create a namespace
kubectl create namespace <namespace-name>

# Delete a namespace
kubectl delete namespace <namespace-name>

# Set default namespace for current context
kubectl config set-context --current --namespace=<namespace-name>

# Get resources in specific namespace
kubectl get pods -n <namespace-name>
```

## Working with ConfigMaps and Secrets

```bash
# List all ConfigMaps
kubectl get configmaps
kubectl get cm

# Create ConfigMap from literal values
kubectl create configmap <name> --from-literal=<key>=<value>

# Create ConfigMap from file
kubectl create configmap <name> --from-file=<file-path>

# Describe ConfigMap
kubectl describe configmap <name>

# Delete ConfigMap
kubectl delete configmap <name>

# List all Secrets
kubectl get secrets

# Create Secret from literal values
kubectl create secret generic <name> --from-literal=<key>=<value>

# Create Secret from file
kubectl create secret generic <name> --from-file=<file-path>

# Describe Secret (values are not shown)
kubectl describe secret <name>

# Get Secret value (base64 encoded)
kubectl get secret <name> -o jsonpath='{.data.<key>}'

# Decode Secret value
kubectl get secret <name> -o jsonpath='{.data.<key>}' | base64 --decode

# Delete Secret
kubectl delete secret <name>
```

## Working with Persistent Volumes

```bash
# List Persistent Volumes
kubectl get pv

# List Persistent Volume Claims
kubectl get pvc

# Describe PVC
kubectl describe pvc <pvc-name>

# Create PVC from file
kubectl apply -f pvc.yaml

# Delete PVC
kubectl delete pvc <pvc-name>
```

## Resource Management

```bash
# Apply configuration from file
kubectl apply -f <file.yaml>

# Apply all YAML files in a directory
kubectl apply -f <directory>/

# Create resource from file
kubectl create -f <file.yaml>

# Delete resource from file
kubectl delete -f <file.yaml>

# Replace resource
kubectl replace -f <file.yaml>

# Edit resource in default editor
kubectl edit <resource-type> <resource-name>

# Get resource in YAML format
kubectl get <resource-type> <resource-name> -o yaml

# Get resource in JSON format
kubectl get <resource-type> <resource-name> -o json

# Get all resources
kubectl get all

# Get all resources in all namespaces
kubectl get all --all-namespaces
```

## Debugging and Troubleshooting

```bash
# Get events
kubectl get events

# Get events sorted by timestamp
kubectl get events --sort-by='.lastTimestamp'

# Watch resources in real-time
kubectl get pods --watch
kubectl get pods -w

# Get resource usage (requires metrics-server)
kubectl top nodes
kubectl top pods

# Describe resource for debugging
kubectl describe <resource-type> <resource-name>

# Get logs from previous container instance
kubectl logs <pod-name> --previous

# Copy files from pod to local
kubectl cp <pod-name>:<path-in-pod> <local-path>

# Copy files from local to pod
kubectl cp <local-path> <pod-name>:<path-in-pod>

# Create a debug pod
kubectl run debug --image=busybox -it --rm -- sh
```

## Labels and Selectors

```bash
# List pods with labels
kubectl get pods --show-labels

# Filter by label
kubectl get pods -l <label-key>=<label-value>
kubectl get pods --selector=<label-key>=<label-value>

# Multiple label selectors
kubectl get pods -l <key1>=<value1>,<key2>=<value2>

# Label a resource
kubectl label <resource-type> <resource-name> <key>=<value>

# Remove a label
kubectl label <resource-type> <resource-name> <key>-

# Update label (overwrite)
kubectl label <resource-type> <resource-name> <key>=<value> --overwrite
```

## Advanced Commands

```bash
# Patch a resource
kubectl patch <resource-type> <resource-name> -p '<json-patch>'

# Apply strategic merge patch
kubectl patch deployment <name> --patch-file patch.yaml

# Diff configuration
kubectl diff -f <file.yaml>

# Explain resource fields
kubectl explain <resource-type>
kubectl explain <resource-type>.<field>

# Get API resources
kubectl api-resources

# Get API versions
kubectl api-versions

# Dry run (test without applying)
kubectl apply -f <file.yaml> --dry-run=client
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml

# Generate YAML without creating
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml > deployment.yaml
```

## Rollout Management

```bash
# Pause rollout
kubectl rollout pause deployment/<deployment-name>

# Resume rollout
kubectl rollout resume deployment/<deployment-name>

# Restart deployment (rolling restart)
kubectl rollout restart deployment/<deployment-name>
```

## Resource Quotas and Limits

```bash
# Get resource quotas
kubectl get resourcequota

# Describe resource quota
kubectl describe resourcequota <quota-name>

# Get limit ranges
kubectl get limitrange

# Describe limit range
kubectl describe limitrange <limitrange-name>
```

## Helpful Aliases

Add these to your `.bashrc` or `.zshrc` for faster command execution:

```bash
alias k=kubectl
alias kgp='kubectl get pods'
alias kgs='kubectl get services'
alias kgd='kubectl get deployments'
alias kgn='kubectl get nodes'
alias kdp='kubectl describe pod'
alias kds='kubectl describe service'
alias kdd='kubectl describe deployment'
alias kl='kubectl logs'
alias klf='kubectl logs -f'
alias kex='kubectl exec -it'
alias ka='kubectl apply -f'
alias kd='kubectl delete -f'
```

## Quick Reference Tips

1. Use `-o wide` for more detailed output: `kubectl get pods -o wide`
2. Use `-o yaml` or `-o json` to see full resource definitions
3. Use `--dry-run=client` to test commands without executing
4. Use `-w` or `--watch` to watch resources in real-time
5. Use `--all-namespaces` or `-A` to see resources across all namespaces
6. Use `--help` with any command to see available options

## Next Steps

- Practice these commands with the [Examples](../examples/)
- Refer to [Core Concepts](02-core-concepts.md) for understanding resources
- Check [Architecture](03-architecture.md) for system understanding
