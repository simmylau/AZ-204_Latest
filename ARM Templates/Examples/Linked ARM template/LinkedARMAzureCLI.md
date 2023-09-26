# AZ CLI commands for Linked ARM templates. 

Deploying a linked template:

`

    az deployment group create \
        --name linkedTemplateWithRelativePath \
        --resource-group myResourceGroup \
        --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/linked-template-relpath/mainTemplate.json"

`
To deploy linked templates with relative path stored in an Azure storage account, use the QueryString/query-string parameter to specify the SAS token to be used with the TemplateUri parameter. This parameter is only supported by Azure CLI version 2.18 or later and Azure PowerShell version 5.4 or later.

`

  az deployment group create \
    --name linkedTemplateWithRelativePath \
    --resource-group myResourceGroup \
    --template-uri "https://stage20210126.blob.core.windows.net/template-staging/mainTemplate.json" \
    --query-string $sasToken

`