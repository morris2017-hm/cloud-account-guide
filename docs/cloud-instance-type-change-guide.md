---
title: 升降配会停机吗：同可用区改规格、跨可用区，以及降配退不退钱
description: 阿里云 ECS、腾讯云 CVM 变更实例规格要重启，vCPU 和内存一起改。同可用区不换地域。阿里云跨可用区会换私网 IP，固定公网 IP 只是尽量保留。包年包月降配算出来小于等于 0 就不退。
permalink: /cloud-instance-type-change-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md)

# 升降配会停机吗

机器买小了，或者买大了想省钱，控制台上就有升降配。我把阿里云国际站和腾讯云国际站现在的说明对了一遍。变更规格会重启，业务会中断。vCPU 和内存绑在同一档规格里，要一起换。地域留在原地。阿里云如果因为当前可用区没库存，改走跨可用区迁移，私网 IP 会换，固定公网 IP 只是尽量留。

下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。轻量应用服务器和 Lighthouse 换的是套餐，见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。抢占式不能按这篇升降配，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。按钮和可选规格会变，以控制台下拉列表为准。

## 改的是一整档规格

| 项目 | 阿里云同可用区变更规格 | 腾讯云调整机型规格 |
| :--- | :--- | :--- |
| 改什么 | vCPU 和内存一起换，不能只改其中一个 | 同样是换规格。系统盘和数据盘都得是云硬盘 |
| 系统和数据 | 变更说明写，动的是 vCPU 和内存，操作系统、程序和数据留下 | 配置调整 FAQ 写，升级后业务环境不用重配 |
| 什么时候生效 | 重启之后。包年包月付完款，控制台可能先显示新规格，启动或重启之后才真正换上 | 运行中的实例要先同意强制关机，重启后新规格才生效。FAQ 写升级大约 1 到 2 分钟 |
| 抢占式 | 按量变更的前提写明不包括抢占式 | 不支持跨机型调整 |
| 降配次数 | 包年包月降配有按历史消费算的月度额度，每月 1 日重置，数字在页面上 | 每台包年包月最多降 5 次，过期实例不能降。按量升降都不限次数 |
| 到期日 | 账单 FAQ（2026-04-27）写实时降配不改到期日 | 升配计费说明写不改到期时间 |

阿里云依据是配置变更总览（2026-06-23）、按量变更规格（2026-09-07）和包年包月变更规格（2026-08-19）。腾讯云依据是调整实例规格（2026-11-05）、配置调整 FAQ（2024-01-06）和调整计费说明（2025-08-14）。

包年包月这次变更只覆盖当前计费周期。临时升几天、再自动退回原规格，说明里没有这条路。已经约了下个周期降配续费时，这个周期不能再升，也不能再降，要等那次续费完成。到期前 15 天内，可以在续费订单里选更低的规格。新计费周期开始后 7 天内重启，这次降配才生效。另有带宽变更还没做完时，规格也改不了。

腾讯云要求系统盘和数据盘都是云硬盘，有一块是本地盘就不能调整规格。磁盘类型见 [系统盘与数据盘](cloud-disk-system-data-guide.md)。

## 下拉列表空的，多半是规格族或磁盘拦住了

阿里云变更前检查页更新于 2026-10-21。可选规格以这一页的对照表和 `DescribeResourcesModification` 的返回为准。控制台按钮是灰的，并带提示，就表示这台当前不能变。

几条会直接挡住：

安全增强、大数据、本地 SSD、持久内存这些规格族，默认不能变更。不支持列表里还有一批裸金属、gn5 和 vgn5i、FPGA 的 f1 和 f3，以及超算集群。Intel 和 AMD 默认不能互改。x86 和 ARM 的架构也要对上。六代及更早的某些规格升到八代及以后，可能因为指令集对不上而失败，先查对照表里有没有这条路径。

当前盘是 Ultra Disk、Standard SSD 或 ESSD Entry，目标又是七代及以后时，要先把盘换成目标规格支持的类型。Basic Disk 要提工单问。变更页如果提示同时改盘，改的是盘的类型或性能级别，数据还在，多出来的费用在订单上。包年包月不能在同一次操作里既把实例降配、又把磁盘升到更贵的类型。实例升配时如果要把盘换成更便宜的类型，先单独改盘，再改规格。

升到八代及以后，源实例要已经装好 NVMe 驱动，或者镜像支持安装。控制台里有 NVMe 驱动状态，没有装可以点安装。配置变更总览还写，跨代升级时 NVMe 磁盘设备名可能变，`fstab` 或开机脚本里写死了设备名，机器会起不来。

目标规格的核数和内存还得是当前系统认得的。镜像和目标规格不兼容时，官方写不能保证回滚。很多变更，尤其是老规格跨代，是单向的，不保证能改回去。重要机器先做系统盘快照，见 [快照与备份](cloud-snapshot-backup-guide.md)。

腾讯云允许跨机型调整，不用先迁数据，但目标规格必须是当前可用区有货的。下面几种会被拦住：抢占式不能跨机型；专有宿主机上的实例只能在宿主机剩余资源里调；GPU、FPGA 这类异构实例不能作为跨机型的源或目标；经典网络不能调到只支持 VPC 的规格；目标规格不支持当前的云盘类型、镜像、弹性网卡数量或公网带宽上限时，也不能调。

少数腾讯云实例在调整后私网 IP 会变。变的话，调整页会写出来。页面没写，就没有变。关机说明（2024-01-08）写，普通关机不改公网 IP 和私网 IP。调整规格比普通关机多这一条私网 IP 的提示，以调整页为准。

## 停机方式选错，可能起不来

阿里云按量实例在运行中变更时，控制台会先关机。停机模式要选标准停机。选了节省停机，计算资源会被收回，目标规格没库存时可能启动失败。已经处于节省停机的，先按标准方式启动，再变更。两种停机各计什么费，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。

包年包月实例在运行中可以预约重启时间，入口在事件里的配置变更重启。有的规格页面会提示不能预约，只能先关机再改。

腾讯云正在「关机不收费」时，除了启动，调整配置、调整磁盘、重装、重置密码这些都做不了，要先启动。调整配置、调整磁盘、重装这几类运维自己触发的关机，也不按关机不收费来停 CPU 和内存的费用。关机不收费页更新于 2025-12-31。

按量变更接口还要求账号没有欠费。欠费后的停机时间是另一套，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。

## 阿里云跨可用区，IP 按另一套走

当前可用区没有目标规格时，预检查 FAQ 写可以换到同规格族的另一个可用区。这是单独的「跨可用区迁移」，页面更新于 2026-10-09，菜单名是 Migrate Instance Across Zones。迁移时只能选同一规格族里的规格。想换规格族，先迁可用区，再在新可用区里变更规格。

换地域不走这一步。跨地域用迁移功能，或做自定义镜像再复制到目标地域，见 [地域怎么选](cloud-region-selection-guide.md)。

跨可用区会停机再启动。从停掉源实例到目标实例启动，大约 15 分钟。实例显示运行，只说明计算和网络迁完了，云盘数据还在继续迁。大约每 100 GiB 要 4 小时。这段时间磁盘 I/O 可能变差，也不能做快照或云盘相关操作。

私网 IP 会变。目标交换机从自己的网段里重新分配一个，迁完后还可以再改。写死了内网地址的程序要一起改。固定公网 IP 是尽量保留，目标可用区容量不够时会分配一个新的。EIP、实例 ID、MAC、安全组、云盘 ID、用户名和登录密码保持不变。地址要跨可用区还留着，先把固定公网 IP 转成 EIP，见 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。实例如果在负载均衡的后端服务器组里，迁完不会自动加回去，要自己加。

软件许可证的机器码可能变。系统盘至少留出 500 MiB，否则虚拟化驱动可能装不上，实例起不来。实例要在 VPC 里。配了 IPv6、多块弹性网卡、反向代理、高可用虚拟 IP、全球加速或路由表时，说明写不能做这次迁移。Safedog、Huweishen、Yunsuo 这几款要先关掉，迁完再开，否则虚拟化驱动可能装失败。已经有一次规格变更还没通过重启生效时，先重启，再开始跨可用区。迁移任务跑着的时候，不要再关机、重启或释放。

抢占式不能跨可用区。异构计算、裸金属、超算集群、u2a、大数据、本地 SSD、安全增强也不支持。突发性能 e 只有 e-c1m1、e-c1m2、e-c1m4 可以。八代点名可以的是 hfg8i、hfc8i、hfr8i、g8i、c8i、r8i、g8a、c8a、r8a、g8ae、c8ae、r8ae、g8y、c8y、r8y。九代点名可以的是 g9i、c9i、r9i。控制台一次最多迁 5 台。只迁可用区、规格不变，不另外收费。同时改了规格，就按升配或降配的费用走。

## 升配补差价，降配不一定退得到

阿里云包年包月升配，补的是剩余时长的差价，订单页有金额。长期活动价买的实例，变更后可能失去原来的续费折扣。升配订单生效之后不能取消。要回到原规格，再做一次降配，降配按降配规则计费。按量实例在变更生效后按新规格单价计。

阿里云降配的现行退款在费用中心说明里，页面更新于 2026-10-08。退款等于线上可退金额乘以新旧配置的差价比例。线上可退金额是现金实付减去已消耗，现金不含代金券抵扣。ECS 已使用不满 30 天时，已消耗按日单价、使用天数和适用折扣算出后再乘 1.5；满 30 天不再乘 1.5。不足一天按一天算。差价比例大于 1 时按 1 计。结果小于等于 0 就不退。活动价或代金券买的，已消耗按官网目录价算，很容易把可退金额吃成 0。同一页写，另一套公式（原配置剩余价值减去新配置购买费用）还在计划中，国际站什么时候灰度另通知。账号没进那次灰度时，仍用现在这套乘法。

ECS 账单 FAQ 更新于 2026-04-27，美元支付写成剩余价值减新配置费用，本地币种写成按原订单汇率的比例退。它还写，实时降配不改到期日；首购和后来升配如果不是同一种币，就不支持实时降配。两页算法不一样。下单时以降配订单上的金额为准。费用中心这页写，退款通常退回原支付方式；退款金额大于当前可开票金额时，会改退到账号余额。

腾讯云包年包月升配按天折算。费用是新旧月单价的差，乘以升配月数，再乘当时的折扣。升配月数用剩余天数除以 365/12。到期日不变。代金券和赠送额度可以付这笔钱。计费说明里的美元演算标明不是国际站实价，金额以订单为准。

腾讯云降配的退款，是实例可退金额减去新规格费用。大于 0，按原支付方式退。小于等于 0，降配仍然做，只是不退。折扣和代金券不退。可退金额会先扣掉已消耗的主机；网络如果按流量计，已使用的网络费按 0 计，按带宽计则按使用时长折算。FAQ 把预估退款为 0 的一种原因写成：购买时打过折，降配却按新规格的原价去减。

按量实例的调整页要确认新规格的冻结金额，并写明调整之后按量价格从第一档计。第一档对应哪一档时长，以计费总览和这一页为准。

渠道结算的账号，升配和降配都在原来的账号里完成，付款设置保持交付时的样子。余额不够付升配差价时，按账号 ID 充值，见 [国际云充值](https://www.yundaxian.com/recharge)。退款落在账号余额还是原支付渠道，以订单和账单为准。

## 常见问题

**只加内存、CPU 不动，能选到吗？**
选不到。两边都是换一整档规格，核数和内存一起变。

**同可用区升配，公网 IP 会变吗？**
阿里云同可用区变更写的是 vCPU 和内存，操作系统、程序和数据留下。腾讯云调整页只对可能变化的私网 IP 做提示，没提示就没变。要换可用区时，再看上一节的 IP 规则。

**降配是不是按差价退现金？**
两家都可能算出 0。活动价、代金券和「已消耗按目录价算」都会把退款压没。订单页显示 0，降配仍可能继续。

**抢占式和轻量能用这篇的入口吗？**
抢占式不能。阿里云轻量可以换更高套餐，也可以做镜像迁到 ECS。腾讯云轻量只能升、不能降。

## 账号从哪里来

升降配改的是已经有的那台 ECS 或 CVM，不会因此新开一个云账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [ECS instance configuration change overview](https://www.alibabacloud.com/help/en/ecs/user-guide/overview-of-instance-configuration-changes)
- [Change the instance type of a pay-as-you-go instance](https://www.alibabacloud.com/help/en/ecs/user-guide/change-the-instance-type-of-a-pay-as-you-go-instance)
- [Change the instance type of a subscription instance](https://www.alibabacloud.com/help/en/ecs/user-guide/change-the-instance-types-of-subscription-instances)
- [Pre-check instance type changes](https://www.alibabacloud.com/help/en/ecs/user-guide/instance-families-that-support-instance-type-changes)
- [Change an instance type across zones](https://www.alibabacloud.com/help/en/ecs/user-guide/change-instance-types-across-zones)
- [Refund rules for downgrades](https://www.alibabacloud.com/help/en/user-center/description-of-downgrade-refund-rules)
- [FAQ about overdue payments, release, unsubscription, and renewal](https://www.alibabacloud.com/help/en/ecs/billing-and-subscription-management-faqs)
- [Configuring the Instance Specifications](https://www.tencentcloud.com/document/product/213/2178)
- [Configuration Adjustment Billing Guide](https://www.tencentcloud.com/document/product/213/59345)
- [Instance Configuration Adjustment](https://www.tencentcloud.com/document/product/213/17349)
- [Shutting Down Pay-as-You-Go Instances to Avoid Charges](https://www.tencentcloud.com/document/product/213/19918)
- [Shutting Down an Instance](https://www.tencentcloud.com/document/product/213/4929)

## 一句话总结

同可用区升降配会重启，vCPU 和内存一起换，系统和云盘上的数据留下。阿里云跨可用区会更换私网 IP，固定公网 IP 只是尽量保留，EIP 不变。包年包月降配按差价计算，结果小于等于 0 就不退，金额以订单页为准。

返回 [指南首页](index.md)
