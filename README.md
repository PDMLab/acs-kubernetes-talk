# k8s in ACS

## Install Azure CLI
```
curl -L https://aka.ms/InstallAzureCli | bash
```

## Login
```
az login
```

## Select subscription
```
az account set --subscription <subscriptionid>
```

## Create Resoure Group
```
az group create --name myResourceGroup --location eastus
```

## Create Cluster
```
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup   --name=myK8sCluster --agent-count=2 --generate-ssh-keys
```

## Install kubectl
```
az acs kubernetes install-cli
```

## Connect kubectl to cluster
```
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

## Get nodes
```
kubectl get nodes
```

## Get services
```
kubectl get svc
```

## Get pods
```
kubectl get pods
```

## GUI
```
az acs kubernetes browse -g <resourceGroup> -n <clusterName>
```

## run nginx
```
kubectl run nginx --image nginx
```

## expose nginx
```
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

## browser nginx
```
kubectl get svc
```

## pod details
```
kubectl describe pod <id>
```

## Create ACR
```
az acr create --resource-group azure-meetup-ka --name AzureMeetupKAACR --sku Basic --admin-enabled true
```

## Scale pods
```
kubectl scale --replicas=2 deployment/azure-vote-front
```

## Scale agents
```
az acs scale --resource-group=hellokubernetes --name=azuresaturday --new-agent-count 2
```
