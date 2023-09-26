# ARM template basics

type arm! in a .json file to have vs populate the initial arm template components.
Suggestions can be triggered by ctrl + spacebar.

## ARM template major components that need to be present

`

    {
        "$schema": "<https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#>",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "functions": {},
        "resources": {},
        "outputs": {}
    }

`

|Component|Description|
|----|---|
|`$schema` (required)| Location of the schema that describes the version of the language template to be used *note*: the filename `deploymentTemplate` shows that the schema used is for deployment. to perform a subscription deployment you use `subscriptionDeploymentTemplate`, management group deployment `managementGroupDeployment`, etc. *TIP:* the VS Code extension **ARM Tools** will populate the empty template, including pre-populating `$schema` and `contentVersion`.|
|`contentVersion` (required)| Versioning of your template.|
|`parameters` (optional)| Input parameters for the template to use. Parameters need to have a name and a type. Allowed types include: *array, bool, int, object, secureObject, secureString, string*. Optional elements along with a `parameter`: *defaultValue, allowedValue, minValue, maxValue, minLength, maxLength, description*.|
|`functions` (optional)| Create functions to be used in the template. Prevents you from typing out complex expressions multiple times. Functions can't access template parameters, only the parameters defined by the function. Each function needs a defined namespace, a name, and an output type and value. Optional: parameter type and value.|
|`variables` (optional)| Allows you to create variables, the type does not need to be defined as this will be inferred from the value. Variables don't have their value defined by input at deployment time.|
|`resources` (required)| Defines the resources to deploy or update as part of this template. Required resource elements are: *type, apiVersion, name, and sometimes location*. Note that the `type` and `name` elements is if the resource is a child of another resource, the type and name of the parent resource are included in those of the child resource. See example 1.|
|`outputs` (optional)| Returns values from deployed resources. Outputs need name and type to be defined.|
|`apiProfile` (optional)| Element allows you define resource apiVersion for all in your template.|

Example 1. Child and parent `resources` syntax.

(Option 1) If the child resource is defined seperate to the parent, you need to add the parents `type` (same with `name`) with /, followed by child's type/name.

`

    {
        "type": "Microsoft.Sql/servers",
        "apiVersion": "2021002-01-preview"
        "name": "parentsqlsrvr"
        "properties": {}
    },
    {
        "type": "Microsoft.Sql/servers/databases",
        "apiVersion":"2021002-01-preview"
        "name": "parentsqlsrvr/childsqldb"
        "properties": {}
    }
`

(Option 2) If child resource is deployed in the same template as the parent, you can define the child resource with the parent resource definition, and define the type and name of the the child resource w/o adding type/name of the parent.

A function uses "[]".

## Deploying multiple resources

1. Multi-tiered templates: deploy all resources in one template. Used for less complex solutions.
2. Nested templates: Subset of resource templates with a parent template linking them all together. Allows for granual reuse of templates across multiple solutions.

Tag gets added to the application settings area as a setting.

![applicationn settings](<Images/application settings.png>)

 Deploy the template by navigating to the same directory that the template is in and then set these:
`az deployment group create --resource-group "RG-AZ-204" --name "myDeployment1" --template-file templateName.json --parameters appPrefix="myPrefix"`

[Deploying different types of ARM templates](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates?tabs=azure-powershell#linked-template)

## Linked template 

To link a template, add a deployments resource to your main template. In the `templateLink` property, specify the URI of the template to include. The following example links to a template that is in a storage account.

When referencing a linked template, the value of `uri` can't be a local file or a file that is only available on your local network. Azure Resource Manager must be able to access the template. Provide a URI value that downloadable as HTTP or HTTPS.

You may reference templates using parameters that include HTTP or HTTPS. For example, a common pattern is to use the _artifactsLocation parameter. You can set the linked template with an expression like:

`

    "uri": "[format('{0}/shared/os-disk-parts-md.json{1}', parameters('_artifactsLocation'), parameters('_artifactsLocationSasToken'))]"

`

### Parameters for linked template

You can provide the parameters for your linked template either in an external file or inline. When providing an external parameter file, use the parametersLink property:

`

    "parametersLink": {
        "uri": "https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.parameters.json",
        "contentVersion": "1.0.0.0"
    }

`

To pass parameter values inline, use the parameters property:

`

    "parameters": {
        "storageAccountName": {
        "value": "[parameters('storageAccountName')]"
        }
    
    }

`

*You can't use both inline parameters and a link to a parameter file.* The deployment fails with an error when both parametersLink and parameters are specified.