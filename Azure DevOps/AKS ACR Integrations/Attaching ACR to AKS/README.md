## Configure ACR integration for existing AKS clusters

This directory is intended for the procedures to attach ACR with AKS Cluster

```
#Set ACR NAME
export ACR_NAME=acrforaksdemo2
echo $ACR_NAME

# Template
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-name>

# Replace Cluster, Resource Group and ACR Repo Name
az aks update -n aksdemo2 -g aks-rg2 --attach-acr $ACR_NAME
```

## Update & Deploy to AKS & Test

### Update Deployment Manifest with Image Name
```yaml
    spec:
      containers:
        - name: acrdemo-localdocker
          image: acrforaksdemo2.azurecr.io/app1/kube-nginx-acr:v1
          imagePullPolicy: Always
          ports:
            - containerPort: 80
```

## Detach ACR from AKS Cluster (Optional)

```
#Set ACR NAME
export ACR_NAME=acrforaksdemo2
echo $ACR_NAME

# Detach ACR with AKS Cluster
az aks update -n aksdemo2 -g aks-rg2 --detach-acr $ACR_NAME

# Delete ACR Repository
Go To Services -> Container Registries -> acrforaksdemo2 -> Delete it
```
