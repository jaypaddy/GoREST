https://www.thepolyglotdeveloper.com/2016/07/create-a-simple-restful-api-with-golang/

https://cloudblogs.microsoft.com/opensource/2018/11/27/tutorial-azure-devops-setup-cicd-pipeline-kubernetes-docker-helm/

az aks get-credentials --resource-group HOST_RG --name hostAKSCluster --overwrite-existing

#Enabled HTTP Routing
az aks enable-addons --resource-group HOST_RG --name hostAKSCluster --addons http_application_routing
az aks show --resource-group HOST_RG --name hostAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

#90507860c32c471d9b35.centralus.aksapp.io

kubectl apply -f aks-basic.yaml











az aks get-credentials --resource-group HOST_RG --name nonVNETaksCluster --overwrite-existing
az aks enable-addons --resource-group HOST_RG --name nonVNETaksCluster --addons http_application_routing
az aks show --resource-group HOST_RG --name nonVNETaksCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table


DFW Appliance Services
817-281-8324