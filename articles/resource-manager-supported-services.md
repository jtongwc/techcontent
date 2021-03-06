<!-- Remove availability -->
<properties
   pageTitle="Resource Manager 支持的服务 | Azure"
   description="介绍支持资源管理器的资源提供程序及其架构和可用 API 版本，以及可托管资源的区域。"
   services="azure-resource-manager"
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
   ms.date="07/18/2016"
   wacn.date="12/15/2016"
   ms.author="magoedte;tomfitz"/>

# 资源管理器提供程序、区域、 API 版本和架构

Azure 资源管理器为你提供了一种新的方式来部署和管理构成应用程序的服务。大多数（但并非所有）服务都支持资源管理器，有些服务仅部分支持资源管理器。Microsoft 将为每个服务启用资源管理器，这对于未来的解决方案而言很重要，但在全面提供支持之前，你需要了解每个服务的当前支持状态。本主题提供支持 Azure 资源管理器的资源提供程序列表。

部署资源时，你还需要知道哪些区域支持这些资源，以及哪些 API 版本可用于资源。[支持的 API 版本](#supported-api-versions)部分说明了如何判断可以使用哪些 API 版本。

<!-- 若要查看 Azure 门户和经典门户支持哪些服务，请参阅 [Azure 门户可用性图表](/features/azure-portal/availability/)。-->若要查看哪些服务支持移动资源，请参阅[将资源移到新的资源组或订阅](/documentation/articles/resource-group-move-resources/)。

下表列出哪些服务可通过资源管理器支持部署和管理，哪些则不可以。“快速入门模板”列中的链接向指定资源提供程序的 Azure 快速入门模板存储库发送查询。快速入门模板中经常会添加和更新数据。即使特定的服务存在链接，也并不一定代表查询将从存储库返回模板。


## 计算

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------------------------ |-------- | ------ | ------ |
| 批处理 | 是 | [Batch REST](https://msdn.microsoft.com/zh-cn/library/azure/dn820158.aspx) | | [Microsoft.Batch](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Batch%22&type=Code) |
| Service Fabric | 是 | [Service Fabric Rest](https://msdn.microsoft.com/zh-cn/library/azure/dn707692.aspx) | | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| 虚拟机 | 是 | [VM REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [Microsoft.Compute](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%22&type=Code) |
| 虚拟机（经典）| 有限制 | - | - |
| 远程应用 | 否 | - | - |
| 云服务（经典）| 有限制（参阅下文）| - | - | - |

虚拟机（经典）是指已通过经典部署模型部署的资源，而不是通过资源管理器部署模型部署的资源。一般而言，这些资源不支持资源管理器操作，但已启用某些操作。有关这些部署模型的详细信息，请参阅[了解资源管理器部署和经典部署](/documentation/articles/resource-manager-deployment-model/)。

云服务（经典）与其他经典资源配合使用；但是，经典资源不能充分利用所有的 Resource Manager 功能，并且不太适合用于将来的解决方案。应该考虑将你的应用程序基础结构更改为使用 Microsoft.Compute、Microsoft.Storage 和 Microsoft.Network 命名空间中的资源。


## 联网

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | -------  | -------- | ------ | ------ |
| 应用程序网关 | 是 | | | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS | 是 | [DNS REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163862.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | 是 | [ExpressRoute REST](https://msdn.microsoft.com/zh-cn/library/azure/mt586720.aspx) | | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| 负载均衡器 | 是 | [负载均衡器 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| 流量管理器 | 是 | [流量管理器 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json) | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| 虚拟网络 | 是| [虚拟网络 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN 网关 | 是 | [网络网关 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163859.aspx) | | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[connections](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |



## 数据和存储

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | 是 | [DocumentDB REST](https://msdn.microsoft.com/zh-cn/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json) | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis Cache | 是 | | [2014-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| 存储 | 是 | [存储 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163683.aspx) | [存储帐户](/documentation/articles/resource-manager-template-storage/) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| SQL 数据库 | 是 | [SQL 数据库 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt163571.aspx) | [2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL 数据仓库 | 是 | | |
| StorSimple | 否 | - | - | - |

## Web 和移动

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| Mobile Apps | 是 | | | |
| Web Apps | 是 | | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## 分析

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | -------  | -------- | ------ | ------ |
| HDInsights | 是 | [HDInsights REST](https://msdn.microsoft.com/zh-cn/library/azure/mt622197.aspx) | | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| 流分析 | 是 | [流分析 REST](https://msdn.microsoft.com/zh-cn/library/azure/dn835031.aspx) | | [Microsoft.StreamAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.StreamAnalytics%22&type=Code) |

## 物联网

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| 事件中心 | 是 | [事件中心 REST](https://msdn.microsoft.com/zh-cn/library/azure/dn790674.aspx) | | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code) |
| IoTHubs | 是 | [IoT 中心 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| 通知中心 | 是 | [Notification Hub REST](https://msdn.microsoft.com/zh-cn/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## 媒体和 CDN

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| CDN | 是 | [CDN REST](https://msdn.microsoft.com/zh-cn/library/azure/mt634456.aspx) | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| 媒体服务 | 是 | 媒体服务 REST | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## 混合集成

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| 服务总线 | 是 | | | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## 标识和访问管理 

Azure Active Directory 可以使用 Resource Manager 为订阅启用基于角色的访问控制。若要了解如何使用基于角色的访问控制和 Active Directory，请参阅 [Azure 基于角色的访问控制](/documentation/articles/role-based-access-control-configure/)。

## 开发人员服务 

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| Visual Studio 帐户 | 是 | | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) | [Microsoft.VisualStudio](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.VisualStudio%22&type=Code) |

## 管理和安全性

| 服务 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------- | ------ | ------ |
| 自动化 | 是 | [自动化 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json) | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| 密钥保管库 | 是 | [密钥保管库 REST](https://msdn.microsoft.com/zh-cn/library/azure/dn903609.aspx) | [密钥保管库](/documentation/articles/resource-manager-template-keyvault/)<br />[密钥保管库机密](/documentation/articles/resource-manager-template-keyvault-secret/) | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| 操作见解 | 是 | | | |
| 计划程序 | 是 | [计划程序 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt629143.aspx) | 2014-08-01 | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| 安全性（预览版） | 是 | [安全 REST](https://msdn.microsoft.com/zh-cn/library/azure/mt704034.aspx) | | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code) |

## 资源管理器

| 功能 | 已启用资源管理器 | REST API | 架构 | 快速入门模板 |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| 授权 | 是 | [管理锁](https://msdn.microsoft.com/zh-cn/library/azure/mt204563.aspx)<br >[基于角色的访问控制](https://msdn.microsoft.com/zh-cn/library/azure/dn906885.aspx) | [资源锁](/documentation/articles/resource-manager-template-lock/)<br />[角色分配](/documentation/articles/resource-manager-template-role/) | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| 资源 | 是 | [链接的资源](https://msdn.microsoft.com/zh-cn/library/azure/mt238499.aspx) | [资源链接](/documentation/articles/resource-manager-template-links/) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a> 资源提供程序和类型

部署资源时，经常需要检索有关资源提供程序和类型的信息。可以通过 REST API、Azure PowerShell 或 Azure CLI 检索此信息。

若要使用资源提供程序，该资源提供程序必须已注册到你的帐户。默认情况下，会自动注册多个资源提供程序；但是，你可能需要手动注册某些资源提供程序。下面的示例演示如何获取资源提供程序的注册状态，并注册资源提供程序（如果需要）。

### REST API

若要获取所有可用的资源提供程序，包括其类型、位置、API 版本和注册状态，请使用[列出所有资源提供程序](https://msdn.microsoft.com/zh-cn/library/azure/dn790524.aspx)操作。如果需要注册资源提供程序，请参阅 [Register a subscription with a resource provider](https://msdn.microsoft.com/zh-cn/library/azure/dn790548.aspx)（将订阅注册到资源提供程序）。

### PowerShell

以下示例演示如何获取所有可用的资源提供程序。

    Get-AzureRmResourceProvider -ListAvailable
    

以下示例演示如何获取特定资源提供程序的资源类型。

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
输出结果将会类似于：

    ResourceTypeName                Locations                                         ApiVersions
    ----------------                ---------                                         ------
    sites/extensions                {China East, China North} {20...
    sites/slots/extensions          {China East, China North} {20...
    ...
    
若要注册资源提供程序，请提供命名空间：

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### Azure CLI

以下示例演示如何获取所有可用的资源提供程序。

    azure provider list
    
输出结果将会类似于：

    info:    Executing command provider list
    + Getting ARM registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

可以使用以下命令将特定资源提供程序的信息保存到文件。

    azure provider show Microsoft.Web -vv --json > c:\temp.json

若要注册资源提供程序，请提供命名空间：

    azure provider register -n Microsoft.ServiceBus

### REST API

若要发现哪些区域可供订阅中的特定资源类型使用，请使用[列出所有资源提供程序](https://msdn.microsoft.com/zh-cn/library/azure/dn790524.aspx)操作。

### PowerShell

以下示例演示如何获取支持网站的区域。

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
输出结果将会类似于：

	中国东部、中国北部

### Azure CLI

以下示例返回每个资源类型支持的所有位置。

    azure location list

也可以使用 [jq](https://stedolan.github.io/jq/) 之类的 JSON 实用工具来筛选位置结果。

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

将返回：

    {
      "name": "Microsoft.Web/sites",
      "location": "China North, China East"
    }

## <a name="supported-api-versions"></a> 支持的 API 版本

部署模板时，必须指定要用于创建每个资源的 API 版本。API 版本对应于资源提供程序发布的 REST API 操作版本。资源提供程序启用新功能时，将会发布 REST API 的新版本。因此，在模板中指定的 API 版本会影响你可以在模板中指定的属性。通常，在创建新模板时，你需要选择最新的 API 版本。对于现有模板，你可以决定是要继续使用以前的 API 版本，还是要选择最新版本来更新模板以利用新功能。

### REST API

若要发现哪些 API 版本可供资源类型使用，请使用[列出所有资源提供程序](https://msdn.microsoft.com/zh-cn/library/azure/dn790524.aspx)操作。

### PowerShell

以下示例演示如何获取特定资源类型可用的 API 版本。

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
输出结果将会类似于：
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### Azure CLI

可以使用以下命令将资源提供程序的信息（包括可用的 API 版本）保存到文件。

    azure provider show Microsoft.Web -vv --json > c:\temp.json

你可以打开该文件并查找 **apiVersions** 元素

## 后续步骤

- 若要了解如何创建资源管理器模板，请参阅[创作 Azure Resource Manager 模板](/documentation/articles/resource-group-authoring-templates/)。
- 若要了解如何部署资源，请参阅[使用 Azure Resource Manager 模板部署应用程序](/documentation/articles/resource-group-template-deploy/)。

<!---HONumber=Mooncake_0912_2016-->