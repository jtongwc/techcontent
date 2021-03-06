<!-- Remove best-practices-rm-security -->
<properties
   pageTitle="通过 Resource Manager 模板使用密钥保管库机密 | Azure"
   description="说明在部署期间如何以参数形式从密钥保管库传递机密。"
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   wacn.date="08/15/2016"
   ms.author="tomfitz"/>

# 在部署期间传递安全值

当你需要在部署期间以参数形式传递安全值（例如密码）时，可以将该值存储为 [Azure 密钥保管库](/documentation/articles/key-vault-whatis/)中的机密，并在其他资源管理器模板中引用该值。你只会在模板中包含对机密的引用，因此该机密永远都不会公开，并且不需要每次部署资源时都手动输入该机密的值。指定哪些用户或服务主体可以访问该机密。

> [AZURE.NOTE] 目前，只有 Azure CLI 支持引用密钥保管库机密的功能。Azure PowerShell 中即将添加此功能。

## 部署密钥保管库和机密

若要创建可从其他 Resource Manager 模板引用的密钥保管库，必须将 **enabledForTemplateDeployment** 属性设置为 **true**，并且必须向相应的用户或服务主体授予访问权限，使其能够执行引用机密的部署。

若要了解如何部署密钥保管库和机密，请参阅[密钥保管库架构](/documentation/articles/resource-manager-template-keyvault/)和[密钥保管库机密架构](/documentation/articles/resource-manager-template-keyvault-secret/)。

## 通过静态 ID 引用机密

从用于将值传递到模板的参数文件内部引用机密。你可以通过传递密钥保管库的资源标识符和机密的名称来引用机密。在本示例中，密钥保管库机密必须已存在，而且你用的是其资源 ID 的静态值。

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

整个参数文件可能如下所示：

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

接受机密的参数应是 **securestring**。以下示例显示了某个模板的相关部分，该模板将部署需要管理员密码的 SQL Server。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## 通过动态 ID 引用机密

上一部分介绍了如何传递密钥保管库机密的静态资源 ID。但是，在某些情况下，你需要引用随当前部署而变的密钥保管库机密。在这种情况下，你不能在参数文件中对资源 ID 进行硬编码。遗憾的是，你不能在参数文件中动态生成资源 ID，因为参数文件中不允许模板表达式。

若要动态生成密钥保管库机密的资源 ID，必须将需要机密的资源移至嵌套式模板中。你需要在主模板中添加嵌套式模板，然后传入包含动态生成的资源 ID 的参数。

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## 后续步骤

- 有关密钥保管库的一般信息，请参阅 [Azure 密钥保管库入门](/documentation/articles/key-vault-get-started/)。
<!-- - 有关对虚拟机使用密钥保管库的信息，请参阅 [Azure Resource Manager 的安全注意事项](/documentation/articles/best-practices-resource-manager-security/)。 -->
- 有关引用密钥机密的完整示例，请参阅[密钥保管库示例](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)。

<!---HONumber=Mooncake_0808_2016-->