<properties 
   pageTitle="关于虚拟网络的安全跨界连接 | Azure"
   description="了解虚拟网络的安全跨界连接类型，包括站点到站点连接、点到站点连接和 ExpressRoute 连接。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
	ms.service="vpn-gateway"
	ms.date="05/16/2016"
	wacn.date="07/25/2016"/>

# 关于虚拟网络的安全跨界连接

本文介绍用于将本地站点连接到 Azure 虚拟网络的不同方法。本文适用于 Resource Manager 与经典部署模型。如果想要查看 VPN 网关连接关系图，请参阅 [Azure VPN 网关连接拓扑](/documentation/articles/vpn-gateway-topology/)。

可以使用三个连接选项：站点到站点、点到站点和 ExpressRoute。选择的选项可能取决于不同的考虑因素，例如：


- 解决方案需要哪种吞吐量？
- 你是希望通信通过经由安全 VPN 的公共 Internet 进行，还是通过专用连接进行？
- 是否有可用的公共 IP 地址？
- 你是否打算使用 VPN 设备？ 如果是，它兼容吗？
- 是要连接几台计算机，还是想要站点持续连接？
- 想要创建的解决方案需要哪种类型的 VPN 网关？

下表可以帮助你为解决方案确定最佳的连接选项。

[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

## 站点到站点连接

通过站点到站点的 VPN，可以在本地站点和虚拟网络之间创建安全连接。若要创建站点到站点连接，请配置本地网络上的 VPN 设备，使其与 Azure VPN 网关创建安全连接。连接该创建后，本地网络上的资源可直接并安全地与位于虚拟网络中的资源进行通信。站点到站点连接不需要为本地网络上的每台客户端计算机都单独建立一个连接，即可访问虚拟网络中的资源。

**在下列情况下，请使用站点到站点连接：**

- 你想要创建混合解决方案。
- 你希望在本地位置和虚拟网络之间建立连接，而无需在客户端进行配置。
- 你希望建立持久的连接。

**要求**

- 本地 VPN 设备必须有面向 Internet 的 IPv4 IP 地址。该设备不能在 NAT 后面。
- 你必须有兼容的 VPN 设备。请参阅[关于 VPN 设备](/documentation/articles/vpn-gateway-about-vpn-devices/)。
- 使用的 VPN 设备必须与解决方案所需的网关类型兼容。请参阅[关于 VPN 网关](/documentation/articles/vpn-gateway-about-vpngateways/)。
- 网关 SKU 也会影响聚合吞吐量。有关详细信息，请参阅[网关 SKU](/documentation/articles/vpn-gateway-about-vpngateways/#gwsku)。

**S2S 可用的部署模型和方法**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]


## 点到站点连接

使用点到站点 VPN 也可以创建与虚拟网络的安全连接。在点到站点配置中，在每台要连接到虚拟网络的客户端计算机上单独配置连接。点到站点连接不需要 VPN 设备。此类连接使用安装在每台客户端计算机上的 VPN 客户端。通过手动从本地客户端计算机上启动连接，建立该 VPN。

点到站点和站点到站点配置可以并存，但与站点到站点连接不同的是，无法将点到站点连接配置为与 ExpressRoute 同时连接到同一虚拟网络。

**在下列情况下，请使用点到站点连接：**

- 只想配置少数几个客户端连接到虚拟网络站点。

- 你希望从远程位置连接到虚拟网络。例如，从咖啡厅或大会现场进行连接。

- 你拥有站点到站点连接，但有一些客户端需要从远程位置连接。

- 无权访问符合站点到站点连接的最低要求的 VPN 设备。

- VPN 设备没有面向 Internet 的 IPv4 IP 地址。

**P2S 可用的部署模型和方法**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## ExpressRoute 连接

使用 Azure ExpressRoute，可在 Azure 数据中心与你的本地环境或共同租用环境中的基础结构之间创建专用连接。ExpressRoute 连接不通过公共 Internet，与通过 Internet 的典型连接相比，提供更高的可靠性、更快的速度、更低的延迟和更高的安全性。

在某些情况下，使用 ExpressRoute 连接在本地和 Azure 之间传输数据还可以产生显著的成本效益。使用 ExpressRoute，你可以在 ExpressRoute 位置（Exchange 提供商设施）建立与 Azure 的连接，也可以直接从网络服务提供商提供的现有 WAN 网络（例如 MPLS VPN）连接到 Azure。

有关 ExpressRoute 的详细信息，请参阅 ExpressRoute[ 技术概述](/documentation/articles/expressroute-introduction/)。


## 后续步骤

- 有关 VPN 网关的详细信息，请参阅[关于 VPN 网关](/documentation/articles/vpn-gateway-about-vpngateways/)、VPN 网关[常见问题](/documentation/articles/vpn-gateway-vpn-faq/)和 [规划与设计](/documentation/articles/vpn-gateway-plan-design/)文章。

- 有关 ExpressRoute 的详细信息，请参阅 ExpressRoute [技术概述](/documentation/articles/expressroute-introduction/)、[常见问题](/documentation/articles/expressroute-faqs/)和[工作流](/documentation/articles/expressroute-workflows/)。





<!---HONumber=Mooncake_0718_2016-->