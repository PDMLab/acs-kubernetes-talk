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

## browse nginx
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

## Get ACR credentials
```
az acr list --resource-group myResourceGroup --query "[].{acrName:name,acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query passwords[0].value -o tsv
```

## Login to ACR
```
docker login --username=<acrName> --password=<acrPassword> <acrLoginServer>
```

## Build images
```
docker build ./azure-voting-app/azure-vote -t azure-vote-front
docker build ./azure-voting-app/azure-back -t azure-vote-back
```

## tag images
```
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

## push images
```
docker push <acrLoginServer>/azure-vote-back:v1
docker push <acrLoginServer>/azure-vote-front:v1
```

## list images
```
az acr repository list --name <acrName> --username <acrName> --password <acrPassword> --output table
az acr repository show-tags --name <acrName> --username <acrName> --password <acrPassword> --repository azure-vote-front --output table
```

## deploy application
```
kubectl create -f storage-resources.yaml
kubectl create -f pod-secrets.yaml
kubectl create -f azure-vote-deployment.yaml
kubectl create -f services.yaml
```

## wait for deployment
```
kubectl get service -w
```

## Scale pods
```
kubectl scale --replicas=2 deployment/azure-vote-front
```

## Scale agents
```
az acs scale --resource-group=hellokubernetes --name=azuresaturday --new-agent-count 2
```

## update deployment
```
change config_file.cfg
```

## rebuild front-image
```
docker build --no-cache ./azure-voting-app/azure-vote -t azure-vote-front:v2
```

## re-tag front-image
```
docker tag azure-vote-front:v2 <acrLoginServer>/azure-vote-front:v2
```

## push front-image v2
```
docker push <acrLoginServer>/azure-vote-front:v2
```
