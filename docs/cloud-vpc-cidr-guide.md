---
title: 代开的第一台机器，网段选错了改不了：主网段、默认 VPC，以及辅助网段
description: 阿里云国际站控制台不能改 VPC 主网段，ModifyVpcAttribute 写可在原网段内扩缩，默认 VPC 另写不能改。腾讯云主网段创建后不能改，产品页和创建接口对 10 段范围不是同一句，对等连接不认辅助网段。
permalink: /cloud-vpc-cidr-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md) · [验证器丢了](cloud-mfa-lost-login-guide.md) · [登录邮箱怎么换](cloud-logon-email-change-guide.md) · [释放保护](cloud-release-protection-guide.md) · [私网网段](cloud-vpc-cidr-guide.md)

# 代开的第一台机器，网段选错了改不了

第一台 ECS 或 CVM 的私网地址，是创建 VPC 和交换机时定下来的。我把阿里云国际站的 VPC 说明、创建接口、修改接口、默认 VPC 接口，还有腾讯云国际站的创建页、限制页、网络规划和创建接口，按页面日期对了一遍。交换机和子网，两边都写创建之后不能改。主网段没有收成同一句。阿里云控制台写不能改。修改接口写可以在原网段里扩或缩。默认 VPC 的接口又写创建后不能改网段。腾讯云产品页写主网段不能改，地址不够就加辅助网段。创建接口对 10 段的范围，和产品页不是同一句。

下文只写 ECS、CVM 用的 VPC。轻量应用服务器和 Lighthouse 不放进来。地域怎么选见 [地域怎么选](cloud-region-selection-guide.md)。进得去进不去，还要看 [安全组怎么配](cloud-security-group-guide.md)。

## 哪一层能改

| 这一层 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 主网段，控制台 | 不能改 | 列表写不能改。操作列可以编辑，编辑页只让加辅助网段 |
| 主网段，接口 | `ModifyVpcAttribute` 写在原网段内扩或缩。`CreateVpc` 没写「之内」 | 创建之后不能改 |
| 默认 VPC | 两条创建路径，网段写法不同。接口写创建后不能改 | 随实例创建，不占配额。页面没给网段 |
| 交换机 / 子网 | 创建后不能改 | 创建后不能改 |
| 辅助网段 | 默认最多 5 段 IPv4、5 段 IPv6，不能和主网段重叠 | 每 VPC 5 段。对等连接不支持 |
| 系统保留地址 | 每个交换机头 1 个、尾 3 个 | 每个子网头 2 个、尾 1 个 |

## 阿里云：控制台不能改，接口写能缩

VPC 说明页（2026-08-13）写，创建时填的 IPv4 是主网段。控制台不能改。可以调 `ModifyVpcAttribute`，用 `CidrBlock` 扩大或缩小。缩小之后，新网段必须包住已经在用的地址。同一页写，VPC 的 IPv6，以及交换机的 IPv4、IPv6，都不能改。

`ModifyVpcAttribute`（2026-08-28）把范围收紧了。新网段要在原来的 IPv4 网段之内，建议掩码 16 到 28 位。示例值是 `192.168.0.0/24`。页面没有给一个换成另一段私网的例子。已经启用的 IPv6 不能改地址，也不能改线路类型。子网落在新网段外面时，错误码是 `IllegalCidrBlock`，原句是 Some subnet cidrBlock is not in the cidrBlock。和新的辅助网段冲突时，错误码是 `OperationFailed.ConflictWithSecondaryCidr`。

`CreateVpc`（2026-07-17）又是一句。创建之后可以扩大或缩小原来的 IPv4 网段。已经在用、又落在目标网段外面的地址，会导致修改失败。这页没写「必须留在原网段之内」。参数说明写掩码 16 到 28 位，例子是 `10.0.0.0/16`、`172.16.0.0/16`、`192.168.0.0/16`。同一行的示例值是 `172.16.0.0/12`。IPAM 的 `Ipv4CidrMask` 示例是 12。三处我都留着，不收成一个掩码范围。

FAQ（2026-10-09）和修改接口站在一边：控制台不支持改主网段，`ModifyVpcAttribute` 只能在主网段内部放大或缩小。想把 `192.168.0.0/16` 换成 `10.0.0.0/16`，这三页都没有写成这个接口能做的事。地址不够，走辅助网段。辅助网段和主网段同时生效，可以用来建交换机、放 ECS。

用户网段是另一个参数。FAQ 写 `UserCidr` 只能在创建 VPC 的接口里传，控制台设不了，设上之后不能改。它管的是「这段地址按路由表走，还是被当成公网」。最多 3 段。我不用它去补私网地址。

## 默认 VPC 的网段，两处写法不一样

说明页写，一个地域只能有一个默认 VPC，一个可用区只能有一个默认交换机。默认的不占配额。默认和非默认不能互相转换。删可以删。

买 ECS、CLB 或 RDS 时，如果这个地域还没有 VPC，可以让系统建一个默认 VPC。这样建出来的，网段固定是 `172.16.0.0/12`。同一页写，调用 `CreateDefaultVpc` 建出来的，网段是 `172.xx.0.0/16`。xx 没有展开成具体数字。

`CreateDefaultVpc`（2026-10-26）写，默认 VPC 创建之后不能改网段，可以加辅助 IPv4 网段。这页没有再写 `172.xx.0.0/16`。每个默认 VPC 最多 30 万个私网地址，这个配额不能提高。`CreateVpc` 对普通 VPC 也写了同一个 30 万，同样不能提高。

FAQ 自己解释过，`172.16.0.0/12` 包含 `172.17.0.0/16`。同一份 FAQ 又建议，要用 Docker 或 Kubernetes 时避开 `172.17.0.0/16`。随 ECS 创建的那条默认网段，和这句建议是叠上的。我要跑容器，就另建 VPC，不在这块默认网段上拆。

腾讯云产品页把 172 段写成 `172.16.0.0` 到 `172.31.255.255`。对等连接要求两边网段不重叠。阿里云这边已经是 `172.16.0.0/12`、FAQ 又写它包含 `172.17.0.0/16` 时，对端再选进这段 172，两块会叠上。

## 交换机删了才能换，保留地址两边不对称

`CreateVSwitch`（2026-09-04）写，交换机网段创建后不能改。掩码 16 到 29 位，必须是所属 VPC 的子集。不能和路由条目的目标网段相同，可以是它的子集。每个 VPC 最多 150 个交换机。一台云产品实例只能属于一个交换机。说明写 `CidrBlock` 和 `CidrMask` 至少填一个。错误码 `ParamExclusive.CidrBlockAndCidrMask` 写这两个参数互斥。我按当次返回看，不把「至少填一个」和「不能同时填」收成一句。

系统保留地址，说明页和这个接口是同一套。IPv4 留出第一个和最后三个。`192.168.1.0/24` 的例子是 `.0`、`.253`、`.254`、`.255`。

FAQ 写，要换交换机网段，只能删掉再建。删之前先释放或迁走里面的 ECS、SLB、RDS。实例刚释放，弹性网卡还要再清 1 到 2 分钟，马上删交换机会失败。辅助网段也一样。`UnassociateVpcCidrBlock`（2026-08-28）写，先删掉用这段建的交换机，再删辅助网段。

## 腾讯云：主网段不能改，10 段两页不是同一句

创建页、限制页、网络规划、查看页都是 2024-10-22。主网段和子网网段创建后不能改。查看页的 IPv4 那一列写不能修改。操作列又写可以 Edit IPv4 CIDR block。编辑页（同一天）把「不能改」限定成主网段。Add 是加辅助网段，不是改主网段。

允许的范围，产品页写成三段。`10.0.0.0` 到 `10.255.255.255`，掩码 12 到 28。`172.16.0.0` 到 `172.31.255.255`，掩码 12 到 28。`192.168.0.0` 到 `192.168.255.255`，掩码 16 到 28。

`CreateVpc`（2026-05-27）比这窄。参数说明写，网段必须落在 `10.0.0.0/12`、`172.16.0.0/12`、`192.168.0.0/16` 里面。同一页写，10 和 172 最大到掩码 12，192.168 最大到掩码 16，最小都是掩码 28。请求示例是 `10.8.0.0/16`，它落在 `10.0.0.0/12` 里。`10.16.0.0/16` 在产品页的 10 段里，不在这句 `10.0.0.0/12` 里。我按下单时的报错看。

网络规划还多了一句经典网络。要和经典网络互通，VPC 得用 `10.[0~47].0.0/16` 及其子集，别的范围不行。`10.16.0.0/16` 到 `10.47.0.0/16` 在这句里，也在产品页的 10 段里，不在创建接口那句 `10.0.0.0/12` 里。限制页另写，经典网络的 CVM 不能和辅助网段里的资源互通。三句我都留着。

子网也有两句。`CreateSubnet`（2026-05-27）写，子网创建后不能改。只有一个子网时，可以和 VPC 一样大。最小掩码 28，16 个地址。最大掩码 16，65536 个地址。VPC 那边 10 和 172 可以到掩码 12。掩码 12 的 VPC，和「子网最大只到掩码 16」放不到同一个大小上。创建页的例子是 VPC 为 `10.0.0.0/16` 时，子网可以等于它，也可以是 `10.0.0.0/24`。我看创建窗口给的范围。

限制页的保留地址和阿里云不同。`172.16.0.0/24` 留出 `.0`、`.1`、`.255`。头两个，尾一个。

单台机器的地址是另一件事。IP 页写，CVM 主网卡的主私网 IP 可以改。辅助网卡的主私网 IP 不能改。CLB 的私网 IP 不能改。子网网段仍然不能改。阿里云 FAQ 把「给已有 ECS 改主私网 IPv4」指到另一页，交换机那一问的答案仍是不能改。

## 辅助网段补的是地址，对等连接不认

阿里云说明页写，辅助 IPv4 不能用 `100.64.0.0/10`、`224.0.0.0/4`、`127.0.0.0/8`、`169.254.0.0/16`，也不能和主网段重叠。默认最多 5 段 IPv4、5 段 IPv6。控制台在 VPC 基本信息的 CIDR Block Management 里加。快捷项是 `10.0.0.0/16`、`172.16.0.0/16`、`192.168.0.0/16`，也可以自定义。FAQ 写，主网段和辅助网段里的 ECS 属于同一个 VPC，安全组和网络 ACL 放行就可以互通。ClassicLink 和辅助网段不兼容。开了路由学习的转发路由器，会学习辅助网段上交换机的系统路由。

腾讯云编辑页写，同一个 VPC 里的子网默认互通，不管它挂在主网段还是辅助网段。辅助网段可以和自定义路由的目标重叠。辅助网段走本地路由，优先级高于自定义的子网路由。我加之前先看路由表里有没有一段会被盖住。

对等连接不支持辅助网段。经典网络的 CVM 也不能和辅助网段互通。云联网、VPN 网关、标准专线网关支持。专线还有三句限制：金融云地域不可用，最多传播 10 段辅助网段，NAT 型专线网关不可用。限制页的配额是每个地域 20 个 VPC、每个 VPC 100 个子网、5 段辅助网段，要提高就提工单。创建页写默认 VPC 和默认子网不占该地域配额。

地址不够、又要对等连接时，我先看缺的是哪一种。腾讯云编辑页写对等连接不认辅助网段。IP 页写，两个 VPC 网段重叠，对等连接建不起来。子网本身不重叠时，可以改看云联网，云联网把限制收到子网。两条路我分开对，不把「加一段辅助网段」当成对等连接已经通了。

中国站和国际站也通不了。阿里云 FAQ 写，基于跨站合规，对等连接不支持中国站账号和国际站账号的 VPC 互通。说明页写，跨账号授权同样不支持中国站和国际站。国际站账号上的机器，不能靠对等连接直接接到中国站的 VPC。跨账号搬机器见 [机器能过户吗](cloud-cross-account-image-migration-guide.md)，那篇走的是镜像和快照，不是改这段网段。

IPv6 也改不了规划。阿里云启用之后，IPv6 网段不能再改。腾讯云网络规划写，每个 VPC 分到一个 `/56`，每个子网一个 `/64`，不能按私网那样自己划。查看页写 IPv6 还在 beta，要用得提工单。

## 接手时我看的几处

这个地域如果已经有默认 VPC，我先看它的网段，再决定机器放不放进去。阿里云随资源创建的那条是 `172.16.0.0/12`。腾讯云创建页没写默认网段，我以列表里的 IPv4 CIDR Block 为准。空的地域，我自己建一条不重叠的。掩码我按产品页和创建接口各自的句子填，接口报错就改一档，不拿 2024 年的产品页去覆盖 2026 年的参数说明。

交换机只划当下要用的一段，剩下的留给以后。阿里云一个交换机绑死一个可用区，腾讯云一个子网也绑死一个可用区。同 VPC 里换可用区，是再加一个交换机，不是把原网段改到另一个区。

网段实在要用另一段，腾讯云 IP 页写同账号的 CVM、腾讯云 MySQL 可以迁到另一个 VPC。阿里云要换的是交换机，FAQ 写先迁走或释放资源再删。删 VPC 之前，腾讯云查看页写里面不能还有资源。阿里云说明页还要先拆掉 CEN 这类连接。

## 常见问题

**控制台改不了，调接口就能把 192.168 换成 10 吗？**
阿里云修改接口和 FAQ 都写，调整要留在原来的主网段里面。示例是把一段缩成 `/24`。换成另一个私网段，这几页没有写成 `ModifyVpcAttribute` 能做的事。腾讯云主网段创建后不能改。要另一段地址，加辅助网段，或另建 VPC。

**已经在默认 VPC 里开了机器，网段还能换吗？**
阿里云 `CreateDefaultVpc` 写默认 VPC 的网段不能改，可以加辅助 IPv4。默认和非默认不能互转。腾讯云主网段同样不能改。机器可以继续用，新的网段得加辅助段，或迁到新建的 VPC。

**加了辅助网段，对端 VPC 就能互通吗？**
同一个 VPC 里面，阿里云 FAQ 写安全组和网络 ACL 放行即可。腾讯云写子网默认互通。腾讯云对等连接不支持辅助网段。两边网段重叠时，对等连接建不起来。中国站和国际站的对等连接，阿里云 FAQ 写不支持。

**子网掩码选长了，删掉重建就行吗？**
网段本身不能改，只能删了再建。阿里云删之前要先清掉交换机里的实例，实例释放后再等 1 到 2 分钟。腾讯云子网创建后也不能改。系统保留地址还占掉头尾几个，掩码越长，能分给机器的越少。

## 账号从哪里来

网段是 VPC 上的参数，改它不会新开一个云账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。交付后自己建 VPC，别让第一台机器落在没看过的默认网段上。渠道账号不要为了改网络去绑卡。云大仙渠道开的账号，续费走原来的渠道。别处注册的国际站账号，不能按老号在这边充。

## 官方参考

- [VPCs and vSwitches](https://www.alibabacloud.com/help/en/vpc/vpc-and-vswitch)
- [CreateVpc](https://www.alibabacloud.com/help/en/vpc/developer-reference/api-vpc-2016-04-28-createvpc)
- [ModifyVpcAttribute](https://www.alibabacloud.com/help/en/vpc/developer-reference/api-vpc-2016-04-28-modifyvpcattribute)
- [CreateDefaultVpc](https://www.alibabacloud.com/help/en/vpc/developer-reference/api-vpc-2016-04-28-createdefaultvpc)
- [CreateVSwitch](https://www.alibabacloud.com/help/en/vpc/developer-reference/api-vpc-2016-04-28-createvswitch)
- [UnassociateVpcCidrBlock](https://www.alibabacloud.com/help/en/vpc/developer-reference/api-vpc-2016-04-28-unassociatevpccidrblock)
- [VPC FAQ](https://www.alibabacloud.com/help/en/vpc/frequently-asked-questions)
- [Creating VPC](https://www.tencentcloud.com/document/product/215/31805)
- [Limits](https://www.tencentcloud.com/document/product/215/31804)
- [Network Planning](https://www.tencentcloud.com/document/product/215/31795)
- [Viewing VPCs](https://www.tencentcloud.com/document/product/215/40069)
- [Editing IPv4 CIDR Blocks](https://www.tencentcloud.com/document/product/215/40070)
- [IP and IP Ranges](https://www.tencentcloud.com/document/product/215/35529)
- [CreateVpc](https://www.tencentcloud.com/document/product/215/15774)
- [CreateSubnet](https://www.tencentcloud.com/document/product/215/15782)

## 一句话总结

交换机和子网的网段，两边都是创建后不能改。阿里云控制台不能改主网段，修改接口只写在原网段里扩缩，默认 VPC 的接口写网段不能改。随 ECS 创建的默认网段是 `172.16.0.0/12`，和 FAQ 让你避开的 `172.17.0.0/16` 叠在一起。腾讯云主网段不能改。产品页的 10 段写到 `10.255.255.255`，创建接口写必须落在 `10.0.0.0/12` 里。辅助网段能补地址，腾讯云的对等连接不认它。

返回 [指南首页](index.md)
