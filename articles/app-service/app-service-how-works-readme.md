<properties
    pageTitle="Azure 应用服务工作方式"
    description="了解应用服务工作方式"
    keywords="应用服务, azure 应用服务, 缩放, 可缩放, 应用服务计划, 应用服务成本"
    services="app-service"
    documentationcenter=""
    author="yochay"
    manager="wpickett"
    editor="" />  

<tags
    ms.assetid="ae74fc32-969e-4580-8d61-02c922f1f184"
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="02/10/2016"
    wacn.date="12/05/2016"
    ms.author="yochay" />  


# 应用服务工作方式
Azure 应用服务是一项云服务，旨在解决工程师目前面临的实际问题。应用服务主要考虑如何大幅提高开发人员的工作效率，同时又不影响云级别的应用程序交付需求。

应用服务还提供创建企业级业务线应用程序所需的功能和框架，同时为开发人员提供最常用的开发语言（例如，Microsoft .NET、Java、PHP、Node.js 和 Python）。开发人员可以通过应用服务执行下述操作：

* 构建高度可缩放的 Web 应用。
* 通过一组易用的移动功能（例如，数据后端、用户身份验证和推送通知），快速构建移动应用后端。
* 通过 API 应用实现、部署和发布 API。
* 将业务应用程序一起关联到工作流中。

> [AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

所有应用类型都依赖于可缩放的、灵活的 Web 应用平台，开发人员可利用该平台优化从应用设计到应用维护的完整生命周期体验。生命周期功能可实现以下项：

* **快速创建应用**。从头开始创建，或者从 Azure 应用商店选取操作支持系统 (OSS) 包。
* **连续部署**。从常用的源代码控制解决方案（例如 GitHub）自动部署新代码。
* **在生产中测试**。顺利创建预生产环境，管理流向其中的流量。根据需要在云中进行调试，发现问题时回退即可。
* **运行异步任务和批处理作业**。在后台进程中运行代码，或根据事件（例如在 Azure 存储队列中排队的消息）和计划的时间 (CRON) 激活代码。
* **缩放应用**。使用多个选项之一，根据流量和资源使用情况自动对服务进行水平缩放和垂直缩放。配置专用环境，专用于用户的应用。
* **维护应用**。使用多个调试和诊断功能来预测问题，并通过自动修复和实时调试等功能来实时、有效地解决它们，或者在事后通过分析日志和内存转储予以解决。

总而言之，应用服务功能可以让开发人员专注于代码，快速进入一个稳定且伸缩性极强的生产状态。利用 API 应用功能，开发人员可以构建实际的企业应用程序，顺利实现业务解决方案从本地到云的集成。

[AZURE.INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]

<!---HONumber=Mooncake_1128_2016-->