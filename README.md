# 手搓K8s

## 01 Prerequisites 实操总结（Azure）

搭建一个完全不依赖托管 Kubernetes的基础设施环境，用于后续手动部署 Kubernetes 控制面和工作节点。

- 一个 Azure VNet（10.240.0.0/16）
- 一个 Subnet（10.240.0.0/24）
- 多台 Ubuntu VM：
    - cp-1, cp-2
    - worker-1, worker-2
- 本地 通过 Public IP + SSH\
- VM 之间通过 Private IP（10.240.0.x）

![alt text](image-2.png)

### 环境选择

- 云平台：Azure
- 操作系统：Ubuntu Server 22.04 LTS

![alt text](image.png)

### 网络规划
Virtual Network

名称：kth-vnet

- Address Space：10.240.0.0/16

- Subnet

名称：kth-subnet

- CIDR：10.240.0.0/24

创建 VNet & Subnet
```bash
az network vnet create \
  --resource-group xxx \
  --name kth-vnet \
  --address-prefix 10.240.0.0/16 \
  --subnet-name kth-subnet \
  --subnet-prefix 10.240.0.0/24
```

![alt text](image-1.png)

### 虚拟机规划
最小可用集群规模（学习用）
角色	名称	数量
Control Plane	cp-1, cp-2	2
Worker Node	worker-1, worker-2	2

### 创建第一台 VM

```
az vm create \
  --resource-group xxx \
  --name cp-1 \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --vnet-name kth-vnet \
  --subnet kth-subnet \
  --admin-username kth \
  --ssh-key-values ~/.ssh/id_rsa.pub \
  --public-ip-sku Standard
```

创建剩余节点

```
az vm create -g xxx -n cp-2      --image Ubuntu2204 --size Standard_B2s --vnet-name kth-vnet --subnet kth-subnet --admin-username kth --ssh-key-values ~/.ssh/id_rsa.pub

az vm create -g xxx -n worker-1  --image Ubuntu2204 --size Standard_B2s --vnet-name kth-vnet --subnet kth-subnet --admin-username kth --ssh-key-values ~/.ssh/id_rsa.pub

az vm create -g xxx -n worker-2  --image Ubuntu2204 --size Standard_B2s --vnet-name kth-vnet --subnet kth-subnet --admin-username kth --ssh-key-values ~/.ssh/id_rsa.pub
```


