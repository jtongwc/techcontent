<properties
	pageTitle="在流分析中使用引用数据和查找表 | Azure"
	description="在流分析查询中使用引用数据"
	keywords="查找表, 引用数据"
	services="stream-analytics"
	documentationCenter=""
	authors="jeffstokes72"
	manager="jhubbard"
	editor="cgronlun"/>  


<tags
	ms.service="stream-analytics"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-services"
	ms.date="09/26/2016"
	wacn.date="11/14/2016"
	ms.author="jeffstok"/>  


# 在流分析的输入流中使用引用数据或查找表

引用数据（也称为查找表）是一个静态的或本质上缓慢变化的有限数据集，用于执行查找或与你的数据流相关联。为了在 Azure 流分析作业中利用引用数据，你通常会在查询中使用[引用数据联合](https://msdn.microsoft.com/zh-cn/library/azure/dn949258.aspx)。流分析使用 Azure Blob 存储作为引用数据的存储层，并且通过 Azure 数据工厂，可以从<!--[-->基于云和本地的任意数量的数据存储区<!--](/documentation/articles/data-factory-data-movement-activities/)-->将引用数据转换和/或复制到 Azure Blob 存储，以用作引用数据。

## 配置引用数据

若要配置引用数据，首先需要创建一个属于**引用数据**类型的输入。下表介绍你在根据属性说明创建引用数据输入时需要提供的每个属性：

<table>
<tbody>
<tr>
<td>属性名称</td>
<td>说明</td>
</tr>
<tr>
<td>输入别名</td>
<td>一个友好名称会用于作业查询，以便引用此输入。</td>
</tr>
<tr>
<td>存储帐户</td>
<td>存储 blob 文件的存储帐户的名称。如果其订阅与你的流分析作业相同，你可以从下拉菜单中选择它。</td>
</tr>
<tr>
<td>存储帐户密钥</td>
<td>与存储帐户关联的密钥。如果存储帐户的订阅与流分析作业相同，则自动填充此密钥。</td>
</tr>
<tr>
<td>存储容器</td>
<td>容器对存储在 Azure Blob 服务中的 blob 进行逻辑分组。将 blob 上载到 Blob 服务时，必须为该 blob 指定一个容器。</td>
</tr>
<tr>
<td>路径模式</td>
<td>用于对指定容器中的 blob 进行定位的文件路径。在路径中，你可以选择指定一个或多个使用以下 2 个变量的实例：<BR>{date}、{time}<BR>示例 1：products/{date}/{time}/product-list.csv<BR>示例 2：products/{date}/product-list.csv
</tr>
<tr>
<td>日期格式 [可选]</td>
<td>如果在你指定的路径模式中使用了 {date}，则可以从支持格式的下拉列表中选择组织你的文件所用的日期格式。示例：YYYY/MM/DD</td>
</tr>
<tr>
<td>时间格式 [可选]</td>
<td>如果在你指定的路径模式中使用了 {time}，则可以从支持格式的下拉列表中选择组织你的文件所用的时间格式。示例：HH</td>
</tr>
<tr>
<td>事件序列化格式</td>
<td>为确保查询按预计的方式运行，流分析需要了解你对传入数据流使用哪种序列化格式。对于引用数据，所支持的格式是 CSV 和 JSON。</td>
</tr>
<tr>
<td>编码</td>
<td>目前只支持 UTF-8 这种编码格式</td>
</tr>
</tbody>
</table>

## 按计划生成引用数据

如果你的引用数据是缓慢变化的数据集，则使用 {date} 和 {time} 替换令牌在输入配置中指定路径模式即可实现对刷新引用数据的支持。流分析将根据此路径模式选取引用数据定义。例如，使用 `sample/{date}/{time}/products.csv` 模式时，日期格式为“YYYY-MM-DD”，时间格式为“HH:mm”，可指示流分析在 2015 年 4 月 16 日下午 5:30（UTC 时区）提取更新的 blob `sample/2015-04-16/17:30/products.csv`。

> [AZURE.NOTE] 当前，流分析作业仅在计算机时间提前于 blob 名称中的编码时间时才查找 blob 刷新。例如，该作业将尽可能查找 `sample/2015-04-16/17:30/products.csv`，但不会早于 2015 年 4 月 16 日下午 5:30（UTC 时区）。它将*永远不会*查找编码时间早于发现的上一个文件的文件。
> 
> 例如，该作业找到 blob `sample/2015-04-16/17:30/products.csv` 后，它将忽略编码日期早于 2015 年 4 月 16 日下午 5:30 的任何文件，因此，如果晚到达的 `sample/2015-04-16/17:25/products.csv` blob 在同一容器中创建，该作业将不会使用它。
> 
> 同样，如果 `sample/2015-04-16/17:30/products.csv` 仅在 2015 年 4 月 16 日晚上 10:03 生成，但容器中没有更早日期的 blob，则该作业将从 2015 年 4 月 16 日晚上 10:03 起开始使用此文件，而在此之前使用以前的引用数据。
> 
> 这种情况的一个例外是，当作业需要按时重新处理数据时或第一次启动作业时。开始时，作业查找的是在指定的作业开始时间之前生成的最新 blob。这样做是为了确保在作业开始时存在**非空**的引用数据集。如果找不到引用数据集，该作业将显示以下诊断：`Initializing input without a valid reference data blob for UTC time <start time>`。



## 有关刷新引用数据的提示 ##

1. 覆盖引用数据 blob 不会导致流分析重新加载 blob，并且在某些情况下，它可能会导致作业失败。更改引用数据的建议方法是使用作业输入中定义的相同容器和路径模式添加新的 blob，并且使用的日期/时间**大于**序列中最后一个 blob 指定的日期/时间。
2.	引用数据 blob 并不按 blob 的“上次修改”时间排序，而是按 blob 名称中使用 {date} 和 {time} 替换项指定的日期和时间排序。
3.	有时，作业必须按时返回，因此不能变更或删除引用数据 blob。

## 获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home?forum=AzureStreamAnalytics)

## 后续步骤
我们已经向你介绍了流分析，这是一种托管服务，适用于对物联网的数据进行流式分析。若要了解有关此服务的详细信息，请参阅：

- [Azure 流分析入门](/documentation/articles/stream-analytics-get-started/)
- [缩放 Azure 流分析作业](/documentation/articles/stream-analytics-scale-jobs/)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/zh-cn/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/zh-cn/library/azure/dn835031.aspx)

<!--Link references-->

[stream.analytics.developer.guide]: /documentation/articles/stream-analytics-developer-guide/
[stream.analytics.scale.jobs]: /documentation/articles/stream-analytics-scale-jobs/
[stream.analytics.introduction]: /documentation/articles/stream-analytics-introduction/
[stream.analytics.get.started]: /documentation/articles/stream-analytics-get-started/
[stream.analytics.query.language.reference]: https://msdn.microsoft.com/zh-cn/library/dn834998.aspx
[stream.analytics.rest.api.reference]: https://msdn.microsoft.com/zh-cn/library/dn835031.aspx

<!---HONumber=Mooncake_1107_2016-->