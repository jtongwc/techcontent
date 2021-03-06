<properties
	pageTitle="Site Recovery 中的故障转移 | Azure" 
	description="Azure Site Recovery 可以协调虚拟机和物理服务器的复制、故障转移与恢复。了解有关故障转移到 Azure 或辅助数据中心的信息。" 
	services="site-recovery" 
	documentationCenter="" 
	authors="rayne-wiselman" 
	manager="jwhit" 
	editor=""/>  


<tags 
	ms.service="site-recovery" 
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery" 
	ms.date="10/05/2016" 
	wacn.date="11/17/2016" 
	ms.author="raynew"/>  


# Site Recovery 中的故障转移

Azure Site Recovery 服务有助于业务连续性和灾难恢复 (BCDR) 策略，因为它可以协调虚拟机和物理服务器的复制、故障转移和恢复。虚拟机可复制到 Azure 中，也可复制到本地数据中心中。如需快速概览，请阅读[什么是 Azure Site Recovery？](/documentation/articles/site-recovery-overview/)

## 概述

本文提供有关恢复（故障转移和故障回复）使用 Site Recovery 保护的虚拟机和物理服务器的信息与说明。

请将任何评论或问题发布到本文底部，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home?forum=hypervrecovmgr)。


## 故障转移类型

为虚拟机和物理服务器启用保护并且在它们进行复制后，可以根据需要运行故障转移。Site Recovery 支持多种类型的故障转移。

**故障转移** | **运行时机** | **详细信息** | **过程**
---|---|---|---
**测试故障转移** | 运行以验证复制策略，或执行灾难恢复演练 | 无数据丢失或停机时间。<br/><br/>不影响复制<br/><br/>不影响生产环境 | 启动故障转移<br/><br/>指定在故障转移后如何将测试计算机连接到网络<br/><br/>在“作业”选项卡上跟踪进度。创建测试计算机，并在辅助位置将它启动<br/><br/>Azure - 在 Azure 经典管理门户中连接到计算机<br/><br/>辅助站点 - 在同一主机和云中访问计算机<br/><br/>完成测试，并自动清理测试故障转移设置。
**计划的故障转移** | 运行以满足合规性要求<br/><br/>针对计划的维护运行<br/><br/>在出现已知的服务中断（例如预期的断电或者恶劣天气）时运行，以故障转移数据，使工作负载保持运行<br/><br/>在从主要位置故障转移到辅助位置后运行，以进行故障回复 | 无数据丢失<br/><br/>在主要位置关闭虚拟机以及在辅助位置打开虚拟机时会发生停机。<br/><br/>在故障转移后，当目标计算机变为源计算机时，虚拟机会受影响。 | 启动故障转移<br/><br/>在“作业”选项卡上跟踪进度。源计算机已关闭<br/><br/>副本计算机在辅助位置启动<br/><br/>Azure - 在 Azure 经典管理门户中连接到副本计算机<br/><br/>辅助站点 - 在同一主机和云中访问计算机<br/><br/>提交故障转移
**未计划的故障转移** | 当主站点由于意外事件（例如断电或病毒攻击）而不可访问时，以响应的方式运行该类型的故障转移<br/><br/>即使主站点不可用，你也可以运行非计划的故障转移。 | 是否丢失数据取决于复制频率设置<br/><br/>数据将根据上次同步时间保持最新 | 启动故障转移<br/><br/>在“作业”选项卡上跟踪进度。可选择尝试关闭虚拟机并同步最新数据<br/><br/>在辅助位置启动副本计算机<br/><br/>Azure - 在 Azure 经典管理门户中连接到副本计算机<br/><br/>辅助站点 - 在同一主机和云中访问计算机<br/><br/>提交故障转移


支持的故障转移类型取决于你的部署方案。

**故障转移方向** | **测试故障转移** | **计划的故障转移** | **未计划的故障转移**
---|---|---|---
主 VMM 站点到辅助 VMM 站点 | 支持 | 支持 | 支持 
辅助 VMM 站点到主 VMM 站点 | 支持 | 支持 | 支持
云到云（单个 VMM 服务器） | 支持 | 支持 | 支持
VMM 站点到 Azure | 支持 | 支持 | 支持 
Azure 到 VMM 站点 | 不支持 | 支持 | 不支持 
Hyper-V 站点到 Azure | 支持 | 支持 | 支持
Azure 到 Hyper-V 站点 | 不支持 | 支持 | 不支持
物理服务器到 Azure | 支持（增强版方案）<br/><br/>不支持（旧方案） | 不支持 | 支持

## 故障转移和故障回复

根据你的部署，将虚拟机故障转移到本地辅助站点或 Azure。故障转移到 Azure 的计算机将作为 Azure 虚拟机创建。可以故障转移单个虚拟机、物理服务器或恢复计划。恢复计划由一个或多个包含受保护虚拟机或物理服务器的有序组构成。它们用于根据多个虚拟机所在的组协调这些虚拟机的故障转移。[详细了解](/documentation/articles/site-recovery-create-recovery-plans/)恢复计划。

完成故障转移并且计算机在辅助位置正常运行后，请注意：

- 如果故障转移到了 Azure，在故障转移后，计算机将不受保护，也不会在主要位置或辅助位置中复制。在 Azure 中，虚拟机存储在提供复原能力但不提供复制的地域复制存储空间中。
- 如果你执行了到辅助站点的非计划故障转移，在故障转移后，辅助位置中的计算机将不受保护或进行复制。
- 如果你执行了到辅助站点的计划故障转移，在故障转移后，辅助位置中的计算机将受保护。
 

### 从辅助站点故障回复

从辅助站点到主站点的故障回复所用的过程，与从主站点到辅助站点的故障转移相同。提交并完成从主数据中心到辅助数据中心的故障转移后，你可以在主站点可用后启动反向复制。反向复制会通过同步增量数据，在辅助站点与主站点之间启动复制。反向复制会使虚拟机进入受保护状态，但辅助数据中心仍是活动位置。为了使主站点成为活动位置，可以启动从辅助站点到主站点的计划故障转移，然后再次启动反向复制。

### 从 Azure 故障回复

如果已故障转移到 Azure，针对虚拟机的 Azure 复原功能将保护你的虚拟机。若要使原始主站点成为活动位置，可以运行计划的故障转移。如果原始站点不可用，可以故障回复到原始位置或备用位置。若要在故障回复到主位置后再次启动复制，你可以启动反向复制。

### 故障转移注意事项

- **故障转移后的 IP 地址** — 默认情况下，故障转移后计算机的 IP 地址将与源计算机不同。如果想要保留相同的 IP 地址，请参阅：
	- **辅助站点** — 如果要故障转移到辅助站点，并且想要保留 IP 地址，请[阅读](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx)此文章。请注意，只有你的 ISP 支持保留公共 IP 地址，你才可以保留这种 IP 地址。
	- **Azure** — 如果要故障转移到 Azure，你可以在虚拟机属性的“配置”选项卡上指定想要分配的 IP 地址。故障转移到 Azure 后无法保留公共 IP 地址。可以保留用作内部地址的非 RFC 1918 地址空间。
- **部分故障转移** — 如果你想要故障转移一部分站点而不是整个站点，请注意：
	- **辅助站点** — 如果你要将部分主站点故障转移到辅助站点并想要连接回到主站点，你可以使用站点到站点 VPN 连接，将辅助站点上已故障转移的应用程序连接到主站点上运行的基础结构组件。如果故障转移了整个子网，则你可以保留虚拟机 IP 地址。如果故障转移了部分子网，则你无法保留虚拟机 IP 地址，因为无法在站点之间拆分子网。
	- **Azure** — 如果你要将部分站点故障转移到 Azure 并想要连接回到主站点，你可以使用站点到站点 VPN 将 Azure 中已故障转移的应用程序连接到主站点上运行的基础结构组件。请注意，如果故障转移了整个子网，则你可以保留虚拟机 IP 地址。如果故障转移了部分子网，则你无法保留虚拟机 IP 地址，因为无法在站点之间拆分子网。
 
- **驱动器号** — 如果你要在故障转移后保留虚拟机上的驱动器号，可将虚拟机的 SAN 策略设置为“打开”。虚拟机磁盘将自动联机。[了解详细信息](https://technet.microsoft.com/zh-cn/library/gg252636.aspx)。
- **路由客户端请求** — Site Recovery 可与 Azure 流量管理器配合工作，以便在故障转移后将客户端请求路由到你的应用程序。




##<a id="run-a-test-failover"></a> 运行测试故障转移

当你运行测试故障转移时，系统将要求你为测试副本计算机选择网络设置。有多个选项供你选择。

**测试故障转移选项** | **说明** | **故障转移检查** | **详细信息**
---|---|---|---
**故障转移到 Azure — 无网络** | 不要选择目标 Azure 网络 | 故障转移将检查是否在 Azure 中按预期启动测试虚拟机 | 恢复计划中的所有测试虚拟机将添加到单个云服务中，并可相互连接<br/><br/>计算机在故障转移后不会连接到 Azure 网络。<br/><br/>用户可以使用公共 IP 地址连接到测试计算机
**故障转移到 Azure — 有网络** | 选择目标 Azure 网络 | 故障转移将检查测试计算机是否已连接到网络 | 创建与 Azure 生产网络隔离的 Azure 网络，并设置基础结构，使复制的虚拟机按预期工作。<br/><br/>如果发生计划或非计划的故障转移，测试虚拟机的子网基于故障转移的虚拟机预期连接到的子网。
**故障转移到辅助 VMM 站点 — 无网络** | 不要选择 VM 网络 | 故障转移将检查是否已创建测试计算机。<br/><br/>将在副本虚拟机所在的同一主机上创建测试虚拟机。它不会被添加到副本虚拟机所在的云中。 | <p>故障转移的计算机不会连接到任何网络。<br/><br/>计算机在创建后可以连接到 VM 网络
**故障转移到辅助 VMM 站点 — 有网络** | 选择现有的 VM 网络 | 故障转移将检查是否已创建虚拟机 | 将在副本虚拟机所在的同一主机上创建测试虚拟机。它不会被添加到副本虚拟机所在的云中。<br/><br/>创建与生产网络隔离的 VM 网络<br/><br/>如果你使用基于 VLAN 的网络，我们建议你针对此目的在 VMM 中创建独立的逻辑网络（不在生产环境中使用）。此逻辑网络用于创建进行测试故障转移的 VM 网络。<br/><br/>该逻辑网络应与托管虚拟机的所有 Hyper-V 服务器的至少一个网络适配器关联。<br/><br/>对于 VLAN 逻辑网络，应隔离你添加到逻辑网络中的网站。<br/><br/>如果你使用基于 Windows 网络虚拟化的逻辑网络，Azure Site Recovery 将自动创建隔离的 VM 网络。
**故障转移到辅助 VMM 站点 — 创建网络** | 将根据你在“逻辑网络”及其相关网站中指定的设置自动创建一个临时测试网络 | 故障转移将检查是否已创建虚拟机 | 如果恢复计划使用多个 VM 网络，则使用此选项。如果你使用 Windows 网络虚拟化网络，此选项可以使用副本虚拟机网络中的相同设置（子网和 IP 地址池）自动创建 VM 网络。在测试故障转移完成后，将自动清除这些 VM 网络。</p><p>测试虚拟机将在与副本虚拟机所在的相同主机上创建。它不会被添加到副本虚拟机所在的云中。

>[AZURE.NOTE] 如果 IP 地址可用于测试故障转移网络，则在测试故障转移期间指定给虚拟机的 IP 地址与在运行计划或非计划故障转移时收到的 IP 地址相同。如果这个相同的 IP 在测试性故障转移网络中不可用，那么虚拟机会收到可在测试故障转移网络中使用的其他 IP 地址。



### 运行从本地到 Azure 的测试故障转移

本过程描述如何对恢复计划运行测试故障转移。或者，你也可以在“虚拟机”选项卡上对单个计算机运行故障转移。

1. 选择“恢复计划”>“recoveryplan\_name”。单击“故障转移”>“测试故障转移”。
2. 在“确认测试故障转移”页上，指定副本计算机在故障转移后如何连接到 Azure 网络。
3. 如果你要故障转移到 Azure 并且为云启用了数据加密，请在“加密密钥”中，选择你在安装提供者期间启用数据加密时颁发的证书。
4. 在“作业”选项卡上跟踪故障转移进度。在 Azure 经典管理门户中，你应当能够看到测试副本计算机。
5. 可以在 Azure 中通过发起到虚拟机的 RDP 连接，从本地站点访问副本计算机。需要在终结点上为虚拟机打开端口 3389。
6. 完成后，在故障转移到达“完成测试”阶段时，单击“完成测试”以完成故障转移。
7. 在“说明”中，记录并保存与测试故障转移相关联的任何观测结果。
8. 单击“测试故障转移已完成”以自动清理测试环境。完成此操作后，测试故障转移将显示“已完成”状态。

> [AZURE.NOTE] 如果测试故障转移持续了两周以上，系统会强行将其结束。测试故障转移期间自动创建的任何元素或虚拟机将被删除。
  

### 运行从本地主站点到本地辅助站点的测试性故障转移

你需要做好许多准备才能运行测试性故障转移，包括制作域控制器的副本和将测试 DHCP 与 DNS 服务器放入测试环境。可以通过多种方式实现此目的：

- 如果你要使用现有网络运行测试故障转移，则应在该网络中准备 Active Directory、DHCP 和 DNS。
- 如果你要使用自动创建 VM 网络的选项运行测试故障转移，则应在恢复计划中你要用于测试故障转移的组 1 之前添加一个手动步骤，然后向自动创建的网络添加基础结构资源，之后运行测试故障转移。

#### 需要注意的事项

- 复制到辅助站点时，副本计算机使用的网络类型不需要与用于测试故障转移的逻辑网络类型相匹配，但某些组合可能不起作用。如果副本使用 DHCP 和基于 VLAN 的隔离，则副本的 VM 网络不需要静态 IP 地址池。因此，无法为测试故障转移使用 Windows 网络虚拟化，因为没有任何可用的地址池。此外，如果副本网络为“无隔离”并且测试网络为“Windows 网络虚拟化”，则测试故障转移将不起作用。这是因为，“无隔离”网络不提供所需的子网来创建 Windows 网络虚拟化网络。
- 故障转移后副本虚拟机连接到映射 VM 网络的方式取决于 VMM 控制台中 VM 网络的配置方式：
	- **使用无隔离或 VLAN 隔离配置的 VM 网络** — 如果为 VM 网络定义了 DHCP，副本虚拟机将使用为关联逻辑网络中的网络站点指定的设置连接到 VLAN ID。虚拟机将从可用的 DHCP 服务器接收其 IP 地址。你不需要为目标 VM 网络定义静态 IP 地址池。如果为 VM 网络使用静态 IP 地址池，副本虚拟机将使用为关联逻辑网络中的网络站点指定的设置连接到 VLAN ID。虚拟机将从为 VM 网络定义的池接收其 IP 地址。如果未在目标 VM 网络上定义静态 IP 地址池，则 IP 地址分配将会失败。应同时在你要用于保护和恢复的源和目标 VMM 服务器上创建 IP 地址池。
	- **使用 Windows 网络虚拟化的 VM 网络** — 如果使用此设置配置了 VM 网络，应为目标 VM 网络定义静态池，无论源 VM 网络是配置为使用 DHCP 还是静态 IP 地址池。如果你定义 DHCP，目标 VMM 服务器将充当 DHCP 服务器，并从为目标 VM 网络定义的池中提供一个 IP 地址。如果你为源服务器定义了使用静态 IP 地址，则目标 VMM 服务器将从池中分配一个 IP 地址。在这两种情况下，如果未定义静态 IP 地址池，则 IP 地址分配将会失败。

#### 运行测试

本过程描述如何对恢复计划运行测试故障转移。或者，你也可以在“虚拟机”选项卡上对单个虚拟机或物理服务器运行故障转移。

1. 选择“恢复计划”>“recoveryplan\_name”。单击“故障转移”>“测试故障转移”。
2. 在“确认测试故障转移”页上，指定在测试故障转移后，虚拟机应如何连接到网络。
3. 在“作业”选项卡上跟踪故障转移进度。在故障转移到达**完成测试**阶段时，单击“完成测试”以结束测试故障转移。
4. 单击“说明”以记录并保存与测试故障转移相关联的任何观测结果。
4. 完成后，验证虚拟机是否成功启动。
5. 在验证虚拟机成功启动后，完成测试故障转移以清理隔离的环境。如果你选择自动创建 VM 网络，则清理过程将删除所有测试虚拟机和测试网络。

> [AZURE.NOTE] 如果测试故障转移持续了两周以上，系统会强行将其结束。测试故障转移期间自动创建的任何元素或虚拟机将被删除。


####<a name="prepare-dhcp"></a> 准备 DHCP

如果测试故障转移中涉及的虚拟机使用 DHCP，则应在为进行测试故障转移创建的隔离网络中创建测试 DHCP 服务器。


### 准备 Active Directory
要运行测试故障转移以进行应用程序测试，测试中需要生产用 Active Directory 环境的副本。如需更多详细信息，请参阅 [Active Directory 的测试性故障转移注意事项](/documentation/articles/site-recovery-active-directory/#considerations-for-test-failover)部分。


### 准备 DNS

按如下所述准备 DNS 服务器以进行测试故障转移：

- **DHCP** — 如果虚拟机使用 DHCP，则应在测试 DHCP 服务器上更新测试 DNS 的 IP 地址。如果使用的网络类型为 Windows 网络虚拟化，则 VMM 服务器充当 DHCP 服务器。因此，应在测试性故障转移网络中更新 DNS 的 IP 地址。在这种情况下，虚拟机将向相关的 DNS 服务器注册自身。
- **静态地址** — 如果虚拟机使用静态 IP 地址，则应在测试性故障转移网络中更新测试 DNS 服务器的 IP 地址。你可能需要使用测试虚拟机的 IP 地址更新 DNS。你可以使用以下示例脚本实现此目的：

	    Param(
	    [string]$Zone,
	    [string]$name,
	    [string]$IP
	    )
	    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
	    $newrecord = $record.clone()
	    $newrecord.RecordData[0].IPv4Address  =  $IP
	    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## 运行计划的故障转移（主站点到辅助站点）

 本过程描述如何对恢复计划运行计划的故障转移。或者，你也可以在“虚拟机”选项卡上对单个虚拟机运行故障转移。

1. 在开始之前，请确保要故障转移的所有虚拟机已完成初始复制。
2. 选择“恢复计划”>“recoveryplan\_name”。单击“故障转移”>“计划的故障转移”。
3. 在“确认计划的故障转移”页上，选择源和目标位置。请注意故障转移方向。

	- 如果先前的故障转移符合预期要求，并且所有虚拟机服务器要么位于源位置，要么位于目标位置，则故障转移方向详细信息仅供参考。
	- 如果虚拟机在源位置和目标位置上均处于活动状态，则将显示“更改方向”按钮。使用此按钮可以更改和指定故障转移应发生的方向。

4. 如果你要故障转移到 Azure 并且为云启用了数据加密，请在“加密密钥”中，选择你在 VMM 服务器上安装提供者期间启用数据加密时颁发的证书。
5. 当计划的故障转移开始时，第一步是关闭虚拟机以确保不会丢失数据。你可以在“作业”选项卡上跟踪故障转移进度。如果在故障转移中发生了错误（在虚拟机上或在恢复计划所包含的脚本中），则恢复计划的计划故障转移将停止。你可以再次启动故障转移。
6. 副本虚拟机在创建后处于待提交状态。单击“提交”以提交故障转移。
7. 复制完成后，虚拟机将在辅助位置启动。

##<a id="run-an-unplanned-failover"></a> 运行非计划的故障转移

本过程描述如何对恢复计划运行非计划的故障转移。或者，你也可以在“虚拟机”选项卡上对单个虚拟机或物理服务器运行故障转移。

1. 选择“恢复计划”>“recoveryplan\_name”。单击“故障转移”>“非计划的故障转移”。
2. 在“确认非计划的故障转移”页上，选择源和目标位置。请注意故障转移方向。

	- 如果先前的故障转移符合预期要求，并且所有虚拟机服务器要么位于源位置，要么位于目标位置，则故障转移方向详细信息仅供参考。
	- 如果虚拟机在源位置和目标位置上均处于活动状态，则将显示“更改方向”按钮。使用此按钮可以更改和指定故障转移应发生的方向。

3. 如果您要故障转移到 Azure 并且为云启用了数据加密，请在“加密密钥”中，选择您在 VMM 服务器上安装提供程序期间启用数据加密时系统颁发的证书。
4. 选择“关闭虚拟机并同步最新数据”，指定 Azure Site Recovery 应尝试关闭受保护的虚拟机并同步数据，以便对最新版的数据进行故障转移。如果你不选择此选项或尝试不成功，则系统将从虚拟机的最近恢复点开始故障转移。
5. 你可以在“作业”选项卡上跟踪故障转移进度。请注意，即使在未计划的故障转移期间发生错误，恢复计划也会运行直至完成。
6. 故障转移后，虚拟机处于“待提交”状态。单击“提交”以提交故障转移。
7. 如果将复制设置为使用多个恢复点，则在“更改恢复点”中，你可以选择使用不是最新的恢复点（默认情况下使用最新的恢复点）。提交后，将删除其他恢复点。
8. 复制完成后，虚拟机将在辅助位置启动并运行。但是，它们不受保护或进行复制。当主站点再次可用并采用相同的底层基础结构时，请单击“反向复制”开始反向复制。这确保所有数据都复制回主站点，并且虚拟机准备好再次进行故障转移。未计划的故障转移之后的反向复制会导致数据传输开销。传输将使用为云的初始复制设置配置的相同方法。

## 辅助位置故障回复到主要位置

 从主要位置故障转移到辅助位置后，复制的虚拟机不受 Site Recovery 的保护，辅助位置现在充当主要位置。请遵循以下过程故障回复到原始主站点。本过程描述如何对恢复计划运行计划的故障转移。或者，你也可以在“虚拟机”选项卡上对单个虚拟机运行故障转移。

1. 选择“恢复计划”>“recoveryplan\_name”。单击“故障转移”>“计划的故障转移”。
2. 在“确认计划的故障转移”页上，选择源和目标位置。请注意故障转移方向。如果从主要位置故障转移已按预期完成，并且所有虚拟机都位于辅助位置，则本部分仅供参考。
3. 如果您要从 Azure 故障回复，请在“数据同步”中选择设置：

	- **在故障转移之前同步数据（仅同步增量更改）** - 此选项可最大程度地减少虚拟机的停机时间，因为它可在不关闭虚拟机的情况下执行同步。此选项将执行以下操作：
		- 阶段 1：在 Azure 中生成虚拟机的快照，并将快照复制到本地 Hyper-V 主机。计算机将继续在 Azure 中运行。
		- 阶段 2：在 Azure 中关闭虚拟机，使其中不会发生任何新的更改。最终的更改集将传输到本地服务器，本地虚拟机将会启动。
	

	- **仅在故障转移期间同步数据（完整下载）** - 如果已在 Azure 上长时间运行，则使用此选项。此选项速度更快，因为我们预计磁盘的大部分已经更改，而且不想花时间进行校验和计算。此选项会执行磁盘的下载。如果已删除本地虚拟机，此选项也很有帮助。
	
	> [AZURE.NOTE] 如果已在 Azure 上运行了一段时间（一个月或以上）或已删除本地 VM，我们建议你使用此选项。此选项不会执行任何校验和计算。
	
4. 如果你要故障转移到 Azure 并且为云启用了数据加密，请在“加密密钥”中，选择你在 VMM 服务器上安装提供者期间启用数据加密时颁发的证书。
5. 默认情况下，将使用最后一个恢复点，但你可以在“更改恢复点”中指定不同的恢复点。
6. 单击复选标记开始故障回复。你可以在“作业”选项卡上跟踪故障转移进度。
7. 如果你选择了在故障转移之前同步数据的选项，请在完成初始数据同步且已准备好在 Azure 中关闭虚拟机后，单击“作业”> <计划故障转移工作名称>“完成故障转移”。这将关闭 Azure 计算机，将最新更改传输到本地虚拟机，然后启动虚拟机。
8. 现在，你可以登录到虚拟机，以验证是否可以按预期使用它。
9. 虚拟机处于待提交状态。单击“提交”以提交故障转移。
10. 现在，为了完成故障回复，请单击“反向复制”以开始保护主站点中的虚拟机。



## 故障回复到备用位置

如果您在 [Hyper-V 站点与 Azure](/documentation/articles/site-recovery-hyper-v-site-to-azure/) 之间部署了保护，则可以从 Azure 故障回复到备用本地位置。如果你需要设置新的本地硬件，此功能将十分有用。下面介绍了操作方法。

1. 如果你需要设置新的硬件，请在服务器上安装 Windows Server 2012 R2 和 Hyper-V 角色。
2. 创建与原始服务器上的名称相同的虚拟网络交换机。
3. 选择“受保护的项”->“保护组”->要故障转移的 <ProtectionGroupName> -> <VirtualMachineName>，然后选择“计划的故障转移”。
4. 在“确认计划的故障转移”中，选择“如果不存在本地虚拟机，则创建它”。
5. 在“主机名”中，选择你要在其上放置虚拟机的新 Hyper-V 主机服务器。
6. 在“数据同步”中，我们建议选择选项“在故障转移之前同步数据”。此选项可以最大程度地减少虚拟机的停机时间，因为它可以在不关闭虚拟机的情况下执行同步。此选项将执行以下操作：

	- 阶段 1：在 Azure 中生成虚拟机的快照，并将快照复制到本地 Hyper-V 主机。计算机将继续在 Azure 中运行。
	- 阶段 2：在 Azure 中关闭虚拟机，使其中不会发生任何新的更改。最终的更改集将传输到本地服务器，本地虚拟机将会启动。
	
7. 单击复选标记开始故障转移（故障回复）。
8. 在完成初始同步且已准备好在 Azure 中关闭虚拟机后，单击“作业”> <计划故障转移工作> >“完成故障转移”。这将关闭 Azure 计算机，将最新更改传输到本地虚拟机，然后启动虚拟机。
9. 你可以登录到本地虚拟机，以验证一切是否如你所愿。然后单击“提交”完成故障转移。
10. 单击“反向复制”开始保护本地虚拟机。

	>[AZURE.NOTE] 如果在“数据同步”步骤中取消故障回复作业，则本地 VM 将处于损坏状态。这是因为数据同步将 Azure VM 磁盘中的最新数据复制到本地数据磁盘上，在同步完成之前，磁盘数据并不处于一致状态。如果在取消数据同步后启动本地 VM，则可能无法启动。重新触发故障转移以完成数据同步。
 

<!---HONumber=Mooncake_1107_2016-->