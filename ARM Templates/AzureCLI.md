# Azure CLI commands 

 Deploy the template by navigating to the same directory that the template is in and then set these:
`az deployment group create --resource-group "RG-AZ-204" --name "myDeployment1" --template-file templateName.json --parameters appPrefix="myPrefix"`
If you set `-Mode complete` any resource that is not part of the template deployment but is present in Azure will be deleted. Makes sure that all resources are set up through an ARM template instead. The defaul is `incremental` which doesn't affect any existing resources that are not part of the template.

To delete the resource group `az group delete --name "RG-AZ-204"`