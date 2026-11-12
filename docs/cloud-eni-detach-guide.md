---
title: 代开的机器，网卡解不掉：主网卡、辅助私网 IP，以及公网跟不跟
description: 阿里云国际站主网卡在实例还在时解不掉。保留后一页写它变成辅助网卡，又写可以设成新实例的主网卡，主网卡保留只列出部分地域。腾讯云主网卡随 CVM 删除。改主私网 IP，一页写可能重启，一页写自动重启大约 30 秒。公网地址只在换交换机和换 VPC 两页写成不变。
permalink: /cloud-eni-detach-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md) · [验证器丢了](cloud-mfa-lost-login-guide.md) · [登录邮箱怎么换](cloud-logon-email-change-guide.md) · [释放保护](cloud-release-protection-guide.md) · [私网网段](cloud-vpc-cidr-guide.md) · [网卡解不掉](cloud-eni-detach-guide.md)

# 代开的机器，网卡解不掉

第一台 ECS 或 CVM 上线之后，私网地址挂在网卡上。我把阿里云国际站的弹性网卡说明、保留页、改主私网 IP、换交换机、换 VPC，还有腾讯云国际站的产品页、概念页、使用限制和两份改 IP 说明，按页面日期对了一遍。实例还在的时候，两边的主网卡都解不下来。阿里云多了一条释放时保留。保留页先写留下来的主网卡变成辅助网卡，下一句又写可以把它设成新实例的主网卡。腾讯云改主私网 IP，2024 年的页写可能重启，2026 年的页写会自动重启大约 30 秒。公网地址跟不跟，阿里云只在换交换机和换 VPC 两页写成不变。

下文只写 ECS、CVM 的弹性网卡。轻量应用服务器和 Lighthouse 不放进来。网段本身能不能改，见 [私网网段](cloud-vpc-cidr-guide.md)。固定公网 IP 和 EIP 怎么计费，见 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。进得去进不去，还要看 [安全组怎么配](cloud-security-group-guide.md)。

## 哪一层能动

| 这一层 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 主网卡，实例还在 | 不能解绑，也不支持热插拔 | 不支持绑定和解绑 |
| 主网卡，实例释放之后 | 默认随实例释放。关掉之后，留下的主网卡被写成辅助网卡 | 创建与管理页写，主网卡在 CVM 删除后一起删 |
| 辅助网卡 | 可以热插拔。列出的几档规格必须先关机 | 可以绑定和解绑，只能绑同可用区的 CVM |
| 主网卡的主私网 IP | 可以改。要先关机，改完不会自己开机 | 可以改。两页对重启的写法不同 |
| 辅助网卡的主私网 IP | 创建后不能改 | 创建后不能改 |
| 辅助私网 IP | 控制台能分配。系统里要再配一次才会认 | 控制台能分配。退掉时，绑在这个地址上的 EIP 一起解绑 |
| 多一块网卡 | 说明页写不另外收费，带宽不因此翻倍 | 使用限制按 vCPU 给张数。IP 张数不等于 EIP 配额 |

## 阿里云：主网卡拔不下来，保留两句对着

弹性网卡说明（2026-10-30）写，每台 ECS 只有一块主网卡。主网卡不能解绑，也不支持热插拔。辅助网卡可以在 Running 状态下绑上或解下，不用重启。同一页写，网卡可以在同一 VPC、同一可用区的实例之间迁移。这句迁的是能解下来的那块。主网卡在实例还在的时候不在这句里。

热插拔还有一张例外表。s6、e、t6、t5，以及上一代的 xn4、n4、mn4、e4，表里点了名的规格不能热插拔。这些规格创建实例时不能顺便绑辅助网卡。实例建好之后可以绑，绑和解绑都要先关机。表外的规格，我仍以当次控制台能不能在运行中操作看。

多绑几块，带宽不加。说明页写，块数变多，实例的网络带宽不因此乘上去。限额页（2026-09-04）另写，同一台机器上两块以上网卡如果来自同一个交换机，可能出现不对称路由。它建议用辅助私网 IP 去分地址，少用同一交换机上的多块网卡。网卡和实例必须同一 VPC、同一可用区。交换机可以不同。VPC 创建之后不能改。

限额页没有印出「一个地域能建多少块网卡」的数字，只给了配额 ID `q_elastic-network-interfaces`。单台机器能绑几块，写的是看实例规格，不按腾讯云那种 vCPU 分档。

## 释放时想留地址，先看地域表

说明页写，Release with Instance 默认开着。开着，网卡随实例释放。`ModifyNetworkInterfaceAttribute`（2026-08-27）的 `DeleteOnRelease` 示例值是 `true`，参数说明把 `true` 写成不保留，`false` 写成保留。和「默认随实例释放」是同一方向。

保留页（2026-08-13）把释放之后的去向拆开。随实例释放时，私网 IPv4、IPv4 前缀、IPv6 都释放。EIP 只是解绑，EIP 本身要自己再释放或改绑。安全组关联拆掉，规则还在。指向这块网卡的 VPC 路由删掉。关掉随实例释放时，这些项留下，EIP 的绑定也留下。

同一页有两句我没有收成一句。留下来的主网卡会变成辅助网卡。下一句写，可以把它设成新实例的主网卡。再挂的时候，目标实例要和这块网卡同一 VPC、同一可用区。

主网卡能不能留，还看地域。辅助网卡，页面写所有地域都能留。主网卡只写了表里的地域。国际站这边我在表里看到香港、新加坡、吉隆坡、雅加达、马尼拉、曼谷、东京、首尔、硅谷、法兰克福、伦敦、弗吉尼亚，另外还有页面列出的中国内地地域。表里没有的地域，我不当成也能留主网卡。

这张表没有固定公网 IP 那一行。固定公网 IP 会不会在实例释放时被收回，我按 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) 那篇对，不拿「网卡留下了」去推断固定公网 IP 还在。EIP 解绑之后可能继续收地址占用费，也在那篇。

## 改主私网 IP：要先关机，公网两页才写不变

改主私网 IP（2026-10-30）开头写，改地址要重启，会断网。往下写，实例必须是 Stopped，不能是锁定。改完不会自己开机，要自己启动。我按「先关机，改完自己点开机」来做。开头那句「重启」，我不当成改完会自动起来。

主网卡上如果还有辅助私网 IPv4 或 IPv6，要先卸掉。系统里如果写死了静态地址，要先改成 DHCP，不然新地址和系统里的旧配置对不上，远程会断。写死在程序、白名单、`/etc/hosts` 里的旧地址也要改。

辅助网卡的主私网 IP，创建之后不能改。随实例一起创建的，地址还不能指定。单独创建的，可以在交换机里挑一个空地址。要换，只能解绑、删掉、再建一块。

公网地址，三页不是同一句。换交换机（2026-08-04）写，公网地址保持不变，私网换成目标交换机里的地址，实例同样不会自动开机。换 VPC（2026-10-30）也写公网地址保持不变。换 VPC 之前要先卸掉全部辅助网卡，并清掉主网卡上的辅助地址。目标安全组要 1 到 5 个，而且类型相同，全是基础安全组或全是企业安全组。改主私网 IP 那页把换交换机放进可选步骤，影响列表里没有公网地址。我只在写明了的那两页上，把公网地址当成不动。

换 VPC、换可用区，都不是把主网卡拔下来插到另一边。[升降配](cloud-instance-type-change-guide.md) 记过，阿里云跨可用区迁移的说明把多块弹性网卡列进做不了的条件。网卡说明要求实例和网卡同一可用区。

## 辅助地址分了，系统里可以还是没有

分配辅助私网 IP（2026-10-28）写，主网卡和辅助网卡都能加。控制台分完，还要在操作系统里再配一次，否则实例认不到。同一页写，未绑定的网卡最多分 10 个私网地址。已经绑上实例之后，上限改看实例规格。规格允许的个数比 10 小，我按规格看，不拿「没绑定时能到 10」去套已经绑上的网卡。

从主网卡上取消分配，实例要在 Running 或 Stopped。这一节只写地址从列表里消失。它没有写 EIP 会不会一起解绑。腾讯云那边有这句，我不用那句去补阿里云。

同一页还写了 cloud-init。公共镜像默认带它，开机时会生成网络配置。手改过 cloud-init 生成的配置，或者改用 NetworkManager 管网络，重启时可能被盖回去。页面给出的做法是在 `/etc/cloud/cloud.cfg.d/` 里关掉它的网络配置。我改静态地址之前先看这句，避免重启之后又变回旧的。

基础安全组的地址配额，和网卡张数是两套。限额页写，一个基础安全组在一个账号、一个地域里最多 6000 个私网地址，主私网 IPv4、辅助 IPv4、IPv6 和前缀都算进去。企业安全组的 65536 计的是关联网卡的块数，不计地址个数。一块网卡最多加入 10 个安全组。`ModifyNetworkInterfaceAttribute` 的参数说明写，安全组只能改辅助网卡。改实例上的安全组，说明页写的是在改主网卡。两套入口我分开对。安全组规则本身见 [安全组怎么配](cloud-security-group-guide.md)。

## 腾讯云：开头写能迁移，主网卡又写不能解

产品页（2026-04-29）第一句写，弹性网卡可以在多台 CVM 之间迁移。概念节接着写，创建 CVM 时生成的是主网卡，用户另建的是辅助网卡。主网卡不支持绑定和解绑，辅助网卡支持。概念页（2026-05-09）是同一套。能迁走的是辅助网卡。主网卡不在第一句的「可以迁移」里。

创建与管理页（2026-05-09）写，辅助网卡的安全组要单独配，不跟主网卡自动走。删网卡时，私网地址、EIP、安全组都会解绑。只有没绑资源的网卡才能删。主网卡在 CVM 删除之后一起删。我要留地址，留的是先解下来的辅助网卡。主网卡没有阿里云那种「关掉随实例释放」的开关页。

绑定仍然要求同一可用区。创建页的手动填地址例子是，子网 `10.0.0.0/24` 时，可用范围从 `10.0.0.2` 到 `10.0.0.254`。阿里云交换机保留的是头 1 个、尾 3 个，两边不是同一套，见 [私网网段](cloud-vpc-cidr-guide.md)。

## 改主私网 IP，重启两页不是同一句

CVM 控制台那页（2024-07-10）写，改主网卡的主 IP 可能导致 CVM 重启，填的地址必须在当前子网里，重启之后才生效。弹性网卡控制台那页（2026-05-09）写，只能改主网卡的主私网 IP，辅助网卡的主 IP 不能改。它写实例会自动重启大约 30 秒，重启后服务恢复。两页我都留着。我不等 2024 年的「可能」去覆盖 2026 年的「自动」，也不把「大约 30 秒」当成所有规格的固定时长。

退辅助私网 IP（2026-03-30）写，主 IP 不能退。私网地址解绑之后，绑在这个地址上的 EIP 会自动解绑。`UnassignPrivateIpAddresses`（2026-05-27）写的是同一句，一次最多 10 个地址。EIP 解绑之后还占不占地址费，仍看 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。这两页没有写公网地址会不会因为改了主私网 IP 而换成另一个。阿里云那两句「公网不变」，我不抄到腾讯云上。

## 配额表和查询接口不是同一列

使用限制（2026-03-30）按 vCPU 给了一张总表。同一 VPC 里，已经绑到 CVM 上的弹性网卡最多 1000 块。表上的 IP 个数是单块网卡的上限，页面写明它不保证你有同样多的 EIP。

| vCPU | 网卡张数 | 每块 IPv4 | 每块 IPv6 |
| :--- | :--- | :--- | :--- |
| 1 | 2 | 6 | 6 |
| 2 到 4 | 4 | 10 | 10 |
| 6 到 8 | 6 | 20 | 20 |
| 12 | 8 | 30 | 30 |
| 14 到 64 以下 | 8 | 40 | 40 |
| 64 及以上 | 16 | 40 | 40 |

同一页的注写，部分 Cloud Physical Machine 2.0 机型支持弹性网卡，举例有标准型 BMS5、内存型 BMM5c。这些机型每台 32 块，每块 20 个辅助私网 IP。产品页（2026-04-29）的限额表末尾把 Cloud Physical Machine 2.0 写成不支持。更新更晚的是产品页。两句我都留着，下单时看这台规格的控制台，以及 `DescribeNetworkInterfaceLimit` 的返回。

这个查询接口把配额拆成三套。`EniQuantity` 是标准挂载，`ExtendEniQuantity` 是扩展挂载，`SubEniQuantity` 是中继网卡。使用限制那张表只有一列网卡张数。我以当次返回的三套数字看，不把它们加总后填进那一列。

## 系统里配了，报文仍可能从主网卡出去

给 Linux 配弹性网卡（2026-03-30）分成工具和手写。工具那一节写，绑上网卡或重启时，它会写配置并下发路由。适用列表写的是 CentOS 8.0、7.8、7.6、7.5、7.4、7.2。我不用这份列表去套 Ubuntu。

手写那一节的同一子网例子里，辅助网卡的默认网关被注释掉，页面写是免得和主网卡抢。配完之后，Linux 默认仍从主网卡把报文发出去。要让从辅助地址进来的流量仍从辅助网卡出去，页面写的是策略路由。临时策略路由在网络服务重启后消失，要写进配置才留得住。

阿里云说明页是同一类现象。辅助网卡默认路由的优先级低于主网卡，报文优先从主网卡走。希望某个辅助地址的流量从它自己的网卡出去，要另做策略路由。控制台显示已绑定，只说明云平台这边挂上了。

## 接手时我看的几处

我先分主网卡和后来加的辅助网卡。实例还在，两边的主网卡都解不下来。阿里云要留私网地址，先看 Release with Instance 是不是关着，再对保留页的地域表。默认是随实例释放。腾讯云主网卡随 CVM 删除，要留的是已经解绑的辅助网卡。

辅助私网 IP 在控制台有了，我进系统看接口上有没有这个地址。没有，就是还没配进系统。阿里云这边我同时看 cloud-init 会不会在下次重启时把配置盖掉。腾讯云退辅助地址之前，我先看这个地址上有没有 EIP。页面写退地址时 EIP 会自动解绑。

改主私网 IP 之前，阿里云我先关机，并清掉辅助地址。改完页面写不会自己开机。腾讯云我按 2026 年那页准备它自己重启。公网是固定公网 IP 还是 EIP，我分开记。阿里云只有换交换机和换 VPC 写了公网不变。

多买一块网卡之前，我先看是要多一个地址，还是要多一块网卡。同一交换机上的第二块网卡，阿里云写可能不对称路由。带宽也不会因为多一块就翻倍。

## 常见问题

**主网卡能拔下来，插到同一可用区的另一台吗？**
实例还在的时候，两边都写不能。阿里云要等实例释放，并且事先关掉随实例释放。保留页写留下来的主网卡变成辅助网卡，下一句又写可以设成新实例的主网卡。主网卡保留只支持页面列出的地域。腾讯云创建与管理页写，主网卡在 CVM 删除后一起删。

**加一块网卡，公网带宽会翻倍吗？**
阿里云说明页写不会。网卡本身那页写不另外收费。EIP、固定公网 IP 和带宽仍按公网那篇计。腾讯云使用限制写，网卡上的 IP 个数不保证你有同样多的 EIP。

**控制台已经分了辅助私网 IP，系统里为什么没有？**
阿里云分配页写，控制台分完还要在系统里再配。腾讯云手写配置那页写，配完之后报文默认仍从主网卡出去。临时策略路由重启网络服务后会丢。

**只改主私网 IP，公网地址变不变？**
阿里云换交换机、换 VPC 两页写公网地址保持不变。只改主私网、影响列表里没写公网的那页，我不替它补这句。腾讯云两份改 IP 说明都没写公网地址。固定公网 IP 和 EIP 的生命周期见公网那篇。

**退掉机器，绑在网卡上的 EIP 还在吗？**
阿里云随实例释放时，保留页写 EIP 被解绑，EIP 要自己释放或改绑。关掉随实例释放时，EIP 的绑定留下。腾讯云删掉一块已经解绑的网卡时，EIP 会解绑。主网卡是随 CVM 一起删。解绑之后的空闲 EIP 可能继续计费。

## 账号从哪里来

网卡是实例上的配置，改它不会新开一个云账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。交付后自己看主网卡和随实例释放，别把主网卡当成能热插拔的地址。渠道账号不要为了改网卡去绑卡。云大仙渠道开的账号，续费走原来的渠道。别处注册的国际站账号，不能按老号在这边充。

## 官方参考

- [ENI overview](https://www.alibabacloud.com/help/en/ecs/user-guide/eni-overview)
- [Retain and reuse an ENI](https://www.alibabacloud.com/help/en/ecs/user-guide/eni-retention-and-reuse)
- [Manage the primary private IP address](https://www.alibabacloud.com/help/en/ecs/user-guide/modify-a-private-ip-address)
- [Assign a secondary private IP](https://www.alibabacloud.com/help/en/ecs/user-guide/assign-secondary-private-ip-addresses)
- [Change the vSwitch of an ECS instance](https://www.alibabacloud.com/help/en/ecs/user-guide/replace-the-switch-of-an-ecs-instance)
- [Change the VPC of an ECS instance](https://www.alibabacloud.com/help/en/ecs/user-guide/change-the-vpc-of-an-ecs-instance)
- [ModifyNetworkInterfaceAttribute](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-modifynetworkinterfaceattribute)
- [ECS limits](https://www.alibabacloud.com/help/en/ecs/user-guide/limitations)
- [Elastic Network Interface](https://www.tencentcloud.com/document/product/213/6514)
- [Key Concepts](https://www.tencentcloud.com/document/product/213/78703)
- [Creating and Managing an ENI](https://www.tencentcloud.com/document/product/213/78705)
- [Modifying a Private IP Address](https://www.tencentcloud.com/document/product/213/16561)
- [Modifying the Primary Private IP Address](https://www.tencentcloud.com/document/product/213/78710)
- [Use Limits](https://www.tencentcloud.com/document/product/215/78456)
- [Releasing a Secondary Private IP Address](https://www.tencentcloud.com/document/product/215/78466)
- [UnassignPrivateIpAddresses](https://www.tencentcloud.com/document/product/215/15814)
- [Configuring an ENI for a Linux CVM](https://www.tencentcloud.com/document/product/215/78461)
- [DescribeNetworkInterfaceLimit](https://www.tencentcloud.com/document/product/215/34768)

## 一句话总结

实例还在，两边的主网卡都解不掉。阿里云默认随实例释放。关掉之后，保留页写主网卡变成辅助网卡，又写可以设成新实例的主网卡，而且只列出部分地域。辅助私网 IP 在控制台分了，系统里还要再配，报文仍可能从主网卡出去。腾讯云主网卡随 CVM 删除。改主私网 IP，2024 年的页写可能重启，2026 年的页写自动重启大约 30 秒。公网地址只在阿里云换交换机和换 VPC 两页写成不变。

返回 [指南首页](index.md)
