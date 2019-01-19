VNET in CBREVNET_RG 
AKS in HOST_RG



a. Create CBREVNET_RG and Create a VNET
b. Create AKS Cluster with Service Principal and attach it to 




az group create --location centralus --name HOST_RG

az group create --location centralus --name CBREVNET_RG

az network vnet create -g CBREVNET_RG -n CBREVNET --address-prefix 10.2.0.0/16 \
                            --subnet-name hostclustersubnet --subnet-prefix 10.2.1.0/24

az network vnet subnet list --resource-group CBREVNET_RG --vnet-name CBREVNET --query [].id --output tsv

az aks create --resource-group HOST_RG  --name hostAKSCluster  --node-count 1  --service-principal b6e1ebcd-3491-4f00-a995-061b9783b176  --client-secret a2548f5d-e34f-4ff9-b788-db5be25acc96 --network-plugin azure --vnet-subnet-id /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/hostclustersubnet --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24

#Assign the Service Principal as a Network Contributor to SUBNET
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/hostclustersubnet --role "Contributor"


#Assign jay_padmanabhan@hotmail.com as the Contributor to HOST_RG
az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/HOST_RG  --role Contributor

#Assign jay_padmanabhan@hotmail.com as the Network Contributor to AKS Subnet
az role assignment create --assignee-object-id 9cea7346-2b62-495e-9551-ea2e9704944b --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/hostclustersubnet --role "Network Contributor"

#Assign the Service Principal as a  Contributor to VNET Resource Group
az role assignment create --assignee b6e1ebcd-3491-4f00-a995-061b9783b176 --scope /subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG --role "Contributor"

























{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 30,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": "/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourceGroups/CBREVNET_RG/providers/Microsoft.Network/virtualNetworks/CBREVNET/subnets/hostclustersubnet"
    }
  ],
  "dnsPrefix": "hostAKSClu-HOSTRG-881ac3",
  "enableRbac": true,
  "fqdn": "hostaksclu-hostrg-881ac3-df4c8f64.hcp.centralus.azmk8s.io",
  "id": "/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourcegroups/HOST_RG/providers/Microsoft.ContainerService/managedClusters/hostAKSCluster",
  "kubernetesVersion": "1.9.11",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2JuV3z0fopWIkb+T69ORpeMlnW/7GXFTBhrvNCfnAVgVteVDGeQphYj5zhg8AybuaMiC5IBHM6LOgUNNzp5lBeNb5fTCcrEHYGWGkw0aAU3O1YBZQsyxAMN52yx5MH9iNJESldiwDJZnfXwetd4kd92bAV8JdcE9qdxQueS1gbNngGkJMpg+1pxf8jDijQEEbTDCCS+X07oUiGrdJop+ehT6+edBXv0hJJWg4L1taAjLHtn86zDjMyLwS/m9CwsvKWjTwvPWJHowPtpLhpqSTcOLwkS1shwVGEfFL6JYc66cLmUChX26/YfQwNglZJ9mVsiTC30xdmkjbI5uKnSz/"
        }
      ]
    }
  },
  "location": "centralus",
  "name": "hostAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.2.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "azure",
    "networkPolicy": null,
    "podCidr": null,
    "serviceCidr": "10.2.0.0/24"
  },
  "nodeResourceGroup": "MC_HOST_RG_hostAKSCluster_centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "HOST_RG",
  "servicePrincipalProfile": {
    "clientId": "b6e1ebcd-3491-4f00-a995-061b9783b176",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
