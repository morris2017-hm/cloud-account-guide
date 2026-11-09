---
title: 轻量应用服务器还是云服务器：阿里云 ECS、腾讯云 CVM 怎么选
description: 阿里云轻量应用服务器和 ECS、腾讯云 Lighthouse 和 CVM 怎么选：套餐与单品的差别、流量超额和欠费停机、磁盘快照与公网 IP 的限制，以及轻量迁到 ECS 时要改什么。
permalink: /lightweight-server-vs-ecs-cvm-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)

# 轻量应用服务器还是云服务器

阿里云账号购买或腾讯云账号购买完成之后，控制台里通常并排着两类机器：套餐制的轻量应用服务器，以及可以分开选规格、磁盘、网络和计费方式的云服务器。阿里云国际版把前者叫 Simple Application Server（轻量应用服务器），后者叫 ECS；腾讯云国际版把前者叫 Lighthouse，后者叫 CVM。名字不同，限制也不同。这篇按两家国际站的产品对比、资源限制和计费说明，整理第一台机器该开哪一种。地域本身怎么选见 [地域怎么选](cloud-region-selection-guide.md)。配额、套餐流量和单价都会调整，以官方当前页面和控制台显示为准。

## 套餐和单品差在哪里

轻量把计算、系统盘、公网和流量捆成一份包年包月套餐，网络由系统建好。云服务器把这些拆开卖，网络、安全组和计费都要自己定。

| 对比项 | 阿里云轻量应用服务器 | 阿里云 ECS | 腾讯云 Lighthouse | 腾讯云 CVM |
| :--- | :--- | :--- | :--- | :--- |
| 更适合 | 个人和低流量应用：建站、论坛、小型电商、开发学习 | 要自定规格、网络和计费的业务 | 中小团队和个人的中低负载网站、小程序、学习环境 | 高并发、集群、视频、大数据和异构计算 |
| 怎么卖 | 套餐和数据盘都是包年包月；超出套餐流量的部分另按量计 | 包年包月、按量付费、抢占式，并可搭配节省计划等 | 基础套餐只按包年包月卖，内含流量包 | 计算、存储、网络分开计，固定带宽或按流量 |
| 公网 | 创建时分配一个公网 IP，不能更换，到期后不保留 | 可用 EIP、NAT 网关自行设计出网 | 带宽和流量写在套餐里 | 可用 EIP，并用安全组控制访问 |
| 访问控制 | 实例自带防火墙，每台最多 50 条规则 | 安全组，还可加网络 ACL | 实例防火墙，每台最多 100 条规则 | 安全组 |
| 磁盘 | 1 块系统盘，数据盘最多再挂 1 块，且不能卸载 | 可挂多块云盘 | 云盘只能挂在 Lighthouse 上 | 弹性云盘挂到同可用区的 CVM |
| 规格 | 在套餐之间升级 | 通用型、计算型、GPU、裸金属等 | 按套餐整体升级，不能降配；不能指定 CPU 型号 | 内存型、高 I/O、大数据、裸金属、GPU/FPGA 等 |

同规格下，腾讯云写明 Lighthouse 的 CPU 和内存表现与 CVM 相当。差别主要在网络、磁盘、计费和能不能降配，不在「轻量的 CPU 一定更弱」。

## 什么时候用轻量就够

下面几件事同时成立时，轻量套餐通常够用：

- 一台机器就能跑的网站、博客、论坛、小型网店、图床，或开发测试、上课用的环境。
- 希望月费跟着套餐走，暂时不想分别核算 CPU、磁盘和带宽。
- 能接受套餐给出的峰值带宽，业务不要求公网质量承诺。
- 还用不到负载均衡、自动伸缩、多块数据盘、IPv6，也不需要自己规划 VPC。

阿里云官方把轻量的典型用途写成建站、知识管理、云端学习、电商、论坛和开发环境，并可以用应用镜像或系统镜像较快装好环境。腾讯云官方写成企业站、博客、小程序、移动端 H5、电商、云存储和图床，以及云上的开发测试和学习环境；应用镜像里会预置软件栈。创建 Lighthouse 时不能挑选底层物理机的 CPU 型号，由平台分配一台满足该套餐的 CPU。

## 什么时候改开 ECS 或 CVM

官方建议在需求超出套餐时改用云服务器：

- 需要通用型、计算型、大数据、GPU、FPGA、NPU 或裸金属。轻量没有这些机型。腾讯云对高并发网站、视频编解码、大型游戏和复杂分布式集群，直接建议用 CVM。
- 要自己划 VPC、交换机、安全组、网络 ACL，或用 EIP、NAT 网关出网。
- 要自动伸缩、部署集、资源编排或资源组。阿里云轻量不提供这些能力。
- 计费要混用包年包月、按量和节省计划。两家的轻量基础套餐都是预付费，阿里云轻量的数据盘同样只按包年包月卖。
- 需要 IPv6。阿里云轻量不支持配置 IPv6。
- 公网要稳定质量。阿里云写明轻量带宽是峰值，高峰期可能限速或丢包，并建议这类业务改买按固定带宽计费的 ECS。

腾讯云还有一条方向性限制：Lighthouse 只能按套餐整体升配，不能降配。锐驰型（Razor Speed）只能升到更高规格的锐驰型；入门型（Starter）和通用型（General）可以升级到另一类套餐，成功后改按目标类型占用配额。业务规模可能变小的，一开始就用可以调整配置的 CVM，比先买一个只能往上升的套餐更合适。

## 流量超额和欠费停机

轻量套餐的停机条件和按量付费的 ECS、CVM 不是同一张时间表。ECS、CVM 的宽限期见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。阿里云 ECS 的 CDT 每月免费流量也不要直接套到轻量套餐上，那是另一套产品，见 [CDT 免费流量](aliyun-international-cdt-free-traffic.md)。

**阿里云轻量**

- 购买页把公网流量标成不限流量时，出入向流量都含在套餐价格里，不再另收流量费。
- 带每月流量额度的套餐，只对超出额度的出方向公网流量按量收费，入向不计这一项。额度在每个自然月 1 日重置。
- 实例概览的监控里如果有流量指标，说明这份套餐有月额度；没有这项指标，通常表示流量已含在套餐内。
- 流量还在额度内时，账号欠费不会因此停掉这台轻量。已经产生超额按量流量，同时账号欠费，实例会自动停机。

**腾讯云 Lighthouse**

- 流量包只统计出方向公网流量，按实例的月计费周期重置，重置点和套餐计费周期一致。当月没用完的流量不退。
- 超出后按使用量计费，按小时结算。锐驰型套餐不对公网流量设用量上限。
- 套餐是预付费。账号欠费时，仍在当月流量包内的实例不受这条欠费规则影响。已经超出当月流量包、同时账号欠费的实例，约 2 小时后停服，控制台显示为待回收。余额补正后会自动恢复；购买了多个月的实例，也可以等到该月流量包重置后再用。再次超额仍会再次停服。
- 套餐到期会进入待回收，15 天内续费可以恢复。到期不续则释放，实例里的数据清空，不能恢复。

每份套餐含多少流量、超出后每个地域每 GB 多少钱，以购买页和官方定价页为准。

带宽峰值也要分开看：

- 阿里云轻量的入向和出向共用套餐里的同一个峰值。套餐写 200 Mbps 时，两个方向各自的上限都是 200 Mbps，但两个方向的流量加在一起不能同时打满这 200 Mbps。该数值是峰值，不是质量承诺。
- Lighthouse 套餐上的带宽默认指出方向上限。购买带宽高于 10 Mbps 时，入向按所购带宽分配；低于 10 Mbps 时，入向按 10 Mbps 分配。锐驰型的入向和出向上限都是 200 Mbps。

## 磁盘、快照和公网 IP

阿里云轻量每台只有一块系统盘。数据盘最多再挂一块，挂载点是 `/dev/vdb`，容量范围在限制页写为 0 到 16,380 GiB。这块数据盘和实例绑在一起，到期日相同，不能卸载，也不能单独续费，要续就和实例一起续。

快照只能手动创建，不支持定时策略。每台最多 3 个；账号总额度是实例数的 3 倍，上限 15 个。重置系统或更换镜像之前做的快照会保留，但不能再用来回滚磁盘。实例释放时，它的快照和由它创建的自定义镜像一起释放。快照不能下载或导出。因此 [快照与备份](cloud-snapshot-backup-guide.md) 里写的 ECS、CVM 自动快照策略，不能直接套到轻量应用服务器上。

公网 IP 在创建时分配，之后不能更换，实例到期后也不保留。域名如果指着这个地址，到期、释放或迁到 ECS 之后都要改解析。

腾讯云这边，Lighthouse 的云盘和 CVM 的云盘相互独立，轻量云盘不能挂到 CVM 上。每个地域的免费快照数是已创建实例数的 2 倍，并且不超过 10。自定义镜像每个地域最多 20 个；其中每个地域有 5 个免费额度，超出部分按小时计费，不足 1 小时按 1 小时计。账号欠费后不能再创建自定义镜像，已有镜像（含免费额度内的）会进入待回收，7 天内未把余额补正会被释放。

开机器前再核对三条配额，避免下单被拒：

- 阿里云轻量默认每个账号、每个地域 50 台，可以按官方说明申请上调。
- Lighthouse 在中国港澳台及其他国家地域：锐驰型每个地域 5 台，入门型和通用型各 20 台。待回收的实例也占用配额。入门型和通用型配额不足时，除中国香港以外的地域可以提交申请，由腾讯云评估是否提高。
- 两家都不支持在这类实例上安装虚拟化软件、使用声卡，或外接加密狗、U 盘和移动硬盘。阿里云的嵌套虚拟化只出现在 ECS 裸金属和超级计算集群上。

部分应用镜像或系统镜像会在 `/home/staragent/`、`/usr/local` 等目录预装监控和安全组件。官方要求不要改动这些文件。

## 轻量以后能迁到云服务器吗

阿里云可以迁。官方路径是在轻量控制台把实例做成自定义镜像，再用该镜像创建同一账号下的 ECS，目标地域可以是原地域，也可以是另一个地域。创建页会按镜像带出地域、镜像、系统盘，以及原实例有数据盘时的数据盘配置。公网 IPv4 要重新分配，所以域名解析要改到新地址；安全组入站规则按原来的防火墙端口重开，否则迁完网站或 SSH 会不通。安全组的配法见 [安全组怎么配](cloud-security-group-guide.md)。

镜像分享还有几条边界：同一地域内，一张自定义镜像最多分享给 10 个主账号，不能分享给 RAM 用户，也不能在中国站账号和国际站账号之间分享。带数据盘的实例做成的共享镜像不包含数据盘。自定义镜像若来自应用镜像，再用它创建包年包月 ECS 时可能另收镜像费；创建按量付费 ECS 则不收镜像费。订单页的显示为准。轻量侧创建快照和自定义镜像本身，在额度内不另收费。

腾讯云的官方对比写明，Lighthouse 云盘不能挂到 CVM。迁移时不要计划把磁盘拆下来直接挂到新的 CVM。是否提供自定义镜像或迁移向导，以当时的 Lighthouse 控制台和文档为准。

升配方面，阿里云轻量可以在控制台换成更高规格的套餐，也可以按上面的镜像方式迁到 ECS。腾讯云轻量只能升、不能降。

## 和账号、防火墙的关系

轻量服务器开在已经验收过的国际版主账号下面，不会因此多出一个账号。中国站和国际站仍然不互通，轻量自定义镜像也不能跨这两个站点分享。要购买中国内地地域时，阿里云国际版的身份验证要求见 [身份验证](aliyun-international-account-verification.md)，腾讯云国际版未完成身份验证就不能购买或使用中国内地地域服务，见 [腾讯云账号购买](tencent-cloud-international-account-purchase.md)。

防火墙也不是安全组。阿里云轻量每个实例最多 50 条防火墙规则，出方向 25 端口被封，发信要改用 465。同一账号、同一地域的轻量实例默认可以内网互通，但默认访问不到同地域 VPC 里的 ECS 或数据库，需要另外建立 VPC 连接。Lighthouse 每个实例最多 100 条防火墙规则，每个账号在每个地域最多 10 个 SSH 密钥对，和 VPC、CVM 的内网互通另有限制，见其地域与网络连通性说明。

## 账号从哪里来

还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理国际版账号。账号到手后先改密码、绑定 MFA，再开第一台机器。

## 官方参考

- [Comparison with ECS](https://www.alibabacloud.com/help/en/simple-application-server/product-overview/comparison-between-simple-application-server-and-ecs)
- [Simple Application Server limitations](https://www.alibabacloud.com/help/en/simple-application-server/product-overview/limits)
- [Billable items](https://www.alibabacloud.com/help/en/simple-application-server/product-overview/billable-items)
- [Billing FAQ](https://www.alibabacloud.com/help/en/simple-application-server/product-overview/billing-faq)
- [Migrate a simple application server to an ECS instance](https://www.alibabacloud.com/help/en/simple-application-server/use-cases/migrate-data-from-lightweight-application-servers-to-ecs-instances-through)
- [Comparing Lighthouse with CVM](https://www.tencentcloud.com/document/product/1103/41521)
- [Lighthouse Use Limits](https://www.tencentcloud.com/document/product/1103/41265)
- [Lighthouse Instance Package](https://www.tencentcloud.com/document/product/1103/41264)
- [Lighthouse Billing Overview](https://www.tencentcloud.com/document/product/1103/41403)
- [Lighthouse Overdue Payments](https://www.tencentcloud.com/document/product/1103/41405)

## 一句话总结

一台低流量网站、希望月费跟着套餐走，用轻量应用服务器；要多块磁盘、要更换公网 IP、要安全组和 VPC、要按量计费或降配、要 GPU 和自动伸缩，直接开 ECS 或 CVM。轻量的流量超额、欠费停机和快照额度都按套餐产品单独计算，买之前先看清是不限流量还是每月额度。

返回 [指南首页](index.md)
