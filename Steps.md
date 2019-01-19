
#Create a Service Principal
https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster#create-a-service-principal
jay@Azure:~$ az ad sp create-for-rbac --skip-assignment
{
  "appId": "b6e1ebcd-3491-4f00-a995-061b9783b176",
  "displayName": "azure-cli-2018-12-27-15-31-32",
  "name": "http://azure-cli-2018-12-27-15-31-32",
  "password": "a2548f5d-e34f-4ff9-b788-db5be25acc96",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}

#Configure ACR Authentication
https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster#configure-acr-authentication

az acr show --resource-group acrRG --name paddycontainers --query "id" --output tsv
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/acrRG/providers/Microsoft.ContainerRegistry/registries/paddycontainers --role Reader

#Create a Kubernetes Cluster
https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster#create-a-kubernetes-cluster
az aks create \
    --resource-group AKS_RG \
    --name LearnAKSCluster \
    --node-count 1 \
    --service-principal b6e1ebcd-3491-4f00-a995-061b9783b176 \
    --client-secret a2548f5d-e34f-4ff9-b788-db5be25acc96 \
    --generate-ssh-keys

#Connect to AKS Cluster
https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl

az aks get-credentials --resource-group AKS_RG --name LearnAKSCluster

#Update azure-vote-all-in-one-redis.yaml to point to your Container Registry
kubectl apply -f azure-vote-all-in-one-redis.yaml

##UNABLE TO ACCESS THE EXTERNAL IP
jay@Azure:~$ kubectl get service azure-vote-front
NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.68.109   23.99.218.151   80:32103/TCP   27m

jay@Azure:~$ kubectl get pods
NAME                                READY     STATUS             RESTARTS   AGE
azure-vote-back-655476c7f7-pklm7    1/1       Running            0          4m
azure-vote-front-76bd76c57f-qhnwt   0/1       ImagePullBackOff   0          4m


#Deploy HTTP Routing
https://docs.microsoft.com/en-us/azure/aks/http-application-routing#deploy-http-routing-cli

az aks enable-addons --resource-group AKS_RG --name LearnAKSCluster --addons http_application_routing


###HELM
Create a Service Account
https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm#create-a-service-account

#Configure Helm
https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm#configure-helm
helm init --service-account tiller


#Create an ingress controller
https://docs.microsoft.com/en-us/azure/aks/ingress-basic#create-an-ingress-controller

helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2



#Service Principal Enablement

#Followed : #Create a Service Principal
#Specify a service principal for an AKS cluster 
https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal#specify-a-service-principal-for-an-aks-cluster
az aks create --resource-group AKS_RG  --name LearnAKSCluster  --service-principal b6e1ebcd-3491-4f00-a995-061b9783b176  --client-secret a2548f5d-e34f-4ff9-b788-db5be25acc96


#Delegate access to other Azure resources
https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal#delegate-access-to-other-azure-resources
#subscriptionID : 881ac365-d417-4791-b2a9-48789acbb88d
#resourceScope : /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/myResourceGroup
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/AKS_RG --role Contributor
#The --scope for a resource needs to be a full resource ID, such as /subscriptions/<guid>/resourceGroups/myResourceGroup or #/subscriptions/<guid>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet





VNET
#a. Create a VNET
#b. Deploy AKS with Service Principal
#c. Assign jay_padmanabhan@hotmail.com as Contributor to VNETAKS_RG
#d. Login as jay_padmanabhan@hotmail.com and Scale
#ERROR
Failed to save container service 'VNETAKS'. Error: The client 'live.com#Jay_padmanabhan@hotmail.com' with object id '9cea7346-2b62-495e-9551-ea2e9704944b' has permission to perform action 'Microsoft.ContainerService/managedClusters/write' on scope '/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/VNETAKS_RG/providers/Microsoft.ContainerService/managedClusters/VNETAKS'; however, it does not have permission to perform action 'Microsoft.OperationalInsights/workspaces/sharedkeys/read' on the linked scope(s) '/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourcegroups/defaultresourcegroup-eus/providers/microsoft.operationalinsights/workspaces/defaultworkspace-881ac365-d417-4791-b2a9-48789acbb88d-eus'.

#e. Delegate access to other Azure resources
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/VNETAKS_RG  --role Contributor

#f. Assigned jay_padmanabhan@hotmail.com Reader access to Operationalinsights 
az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourcegroups/defaultresourcegroup-eus/providers/microsoft.operationalinsights/workspaces/defaultworkspace-881ac365-d417-4791-b2a9-48789acbb88d-eus --role Reader

#ERROR
Failed to save container service 'VNETAKS'. Error: The client 'live.com#Jay_padmanabhan@hotmail.com' with object id '9cea7346-2b62-495e-9551-ea2e9704944b' has permission to perform action 'Microsoft.ContainerService/managedClusters/write' on scope '/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/VNETAKS_RG/providers/Microsoft.ContainerService/managedClusters/VNETAKS'; however, it does not have permission to perform action 'Microsoft.OperationalInsights/workspaces/sharedkeys/read' on the linked scope(s) '/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourcegroups/defaultresourcegroup-eus/providers/microsoft.operationalinsights/workspaces/defaultworkspace-881ac365-d417-4791-b2a9-48789acbb88d-eus'.

#Did not Work


#Configure networking - CLI
#https://docs.microsoft.com/en-us/azure/aks/configure-advanced-networking#configure-networking---cli
az network vnet subnet list --resource-group VNETAKS_RG --vnet-name AKSTESTVNET  --query [].id --output tsv
/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/VNETAKS_RG/providers/Microsoft.Network/virtualNetworks/AKSTESTVNET/subnets/default

az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/VNETAKS_RG/providers/Microsoft.Network/virtualNetworks/AKSTESTVNET/subnets/default --role Contributor




#FINAL
#Get SubNet Resource Information
az network vnet subnet list --resource-group CBREVNET_RG --vnet-name CBREVNET  --query [].id --output tsv
#/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/akssubnet


##Attach the Service Principal as Contributor to the Subnet
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/akssubnet  --role Contributor


#Delegate access to other Azure resources (Resource Group Contributor for Service Principal)
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/HOSTaks_RG  --role Contributor

#Create Cluster
az aks create --name hostAksCluster  --resource-group HOSTaks_RG  --location "centralus"  --node-vm-size "Standard_DS2_v2"  --node-count 1  --dns-name-prefix host  --network-plugin azure  --max-pods 30   --service-cidr 10.0.2.0/24  --dns-service-ip 10.0.0.10  --docker-bridge-address 172.17.0.1/16 --vnet-subnet-id //subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/akssubnet --service-principal b6e1ebcd-3491-4f00-a995-061b9783b176 --client-secret a2548f5d-e34f-4ff9-b788-db5be25acc96

#Configure ACR Authentication
https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster#configure-acr-authentication
az acr show --resource-group HOST_RG --name paddycontainers --query "id" --output tsv
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/acrRG/providers/Microsoft.ContainerRegistry/registries/paddycontainers --role Reader



#Add User as Contributor to Resource Group
az role assignment create --assignee 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/HOSTaks_RG  --role Contributor


#Configure networking - CLI
#https://docs.microsoft.com/en-us/azure/aks/configure-advanced-networking#configure-networking---cli
#Grab the Resource Identifier for VNET
az network vnet subnet list --resource-group CBREVNET_RG  --vnet-name CBREVNET  --query [].id --output tsv


az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/default --role "Network Contributor"

az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/akssubnet --role "Network Contributor"



az aks get-credentials --resource-group HOSTaks_RG --name hostAksCluster