#az aks create --resource-group AKS_RG  --name LearnAKSCluster  --service-principal
{
  "aadProfile": null,
  "addonProfiles": {
    "httpApplicationRouting": {
      "config": {
        "HTTPApplicationRoutingZoneName": "5f6c014178f64e819c34.centralus.aksapp.io"
      },
      "enabled": true
    }
  },
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  "dnsPrefix": "LearnAKSCl-AKSRG-881ac3",
  "enableRbac": true,
  "fqdn": "learnakscl-aksrg-881ac3-5a330c7a.hcp.centralus.azmk8s.io",
  "id": "/subscriptions/881ac365-d417-4791-b2a9-48789acbb88d/resourcegroups/AKS_RG/providers/Microsoft.ContainerService/managedClusters/LearnAKSCluster",
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
  "name": "LearnAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_AKS_RG_LearnAKSCluster_centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "AKS_RG",
  "servicePrincipalProfile": {
    "clientId": "b6e1ebcd-3491-4f00-a995-061b9783b176",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}