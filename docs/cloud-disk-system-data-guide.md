---
title: 系统盘和数据盘怎么选：云盘类型、实际 IOPS 与释放后还扣不扣费
description: 阿里云 ECS、腾讯云 CVM 购买时系统盘和数据盘怎么选。云盘标称的最大 IOPS 要按容量公式折算，扩容不能缩小，Linux 数据盘要初始化，释放实例后未释放的云盘还会继续计费。
permalink: /cloud-disk-system-data-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md)

# 系统盘和数据盘怎么选

阿里云账号或腾讯云账号到手、创建第一台 ECS 或 CVM 时，购买页会让你选系统盘、数据盘和云盘类型。选错通常不是开不了机，而是两件事后才看见的事：几十 GiB 的盘并没有规格表上的最大 IOPS；实例删了，云盘仍按容量扣费。

关机之后云盘为什么还在计费，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。快照怎么设，见 [快照与备份](cloud-snapshot-backup-guide.md)。轻量应用服务器和 Lighthouse 的磁盘配额、不能卸载的系统盘，不是下面这套云盘，见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。购买页上的类型、容量和按钮会变，以控制台为准。

## 系统盘和数据盘各管什么

| 项目 | 系统盘 | 数据盘 |
| :--- | :--- | :--- |
| 放什么 | 操作系统和程序 | 业务数据、日志、数据库文件 |
| 怎么来 | 只能随实例一起创建 | 可以随实例创建，也可以以后单独买再挂上 |
| 阿里云 | 创建时自动初始化。默认随实例释放；关掉这个选项后，释放实例时它会被卸下，当作数据盘留下 | 单独创建的按量数据盘，默认不随实例释放；其他情况默认随实例释放。未挂载的盘不受实例释放影响 |
| 腾讯云 | 非弹性云盘，生命周期跟实例走，不能单独挂上或卸下 | 弹性云盘生命周期独立，可以在同一可用区的 CVM 之间卸载再挂载。一台 CVM 最多 20 块弹性云盘 |

依据：[阿里云块存储概述](https://www.alibabacloud.com/help/en/ecs/user-guide/elastic-block-storage-devices)（2026-05-15）、[创建和使用系统盘](https://www.alibabacloud.com/help/en/ecs/user-guide/create-and-use-a-system-disk)、[释放云盘](https://www.alibabacloud.com/help/en/ecs/user-guide/release-a-disk)（2026-10-22）、[腾讯云 CBS 概述](https://www.tencentcloud.com/document/product/362/2345)、[使用限制](https://www.tencentcloud.com/document/product/213/15379)。

一块从系统盘快照创建出来的阿里云盘，只能当数据盘用，不能再当系统盘。腾讯云的限制不一样：数据盘快照可以用来新建弹性云盘，系统盘快照只能用来做自定义镜像。

云盘不能直接改地域，也不能挂到另一个可用区的实例上。阿里云同可用区冗余的云盘只在本可用区可见；地域级 ESSD 可以挂到同地域任一可用区，写延迟通常高于同可用区盘。腾讯云要求云盘和 CVM 在同一可用区。换地域要走镜像或快照复制，见 [地域怎么选](cloud-region-selection-guide.md)。

## 规格表上的最大 IOPS，不是这块盘的 IOPS

两家都按容量给基线性能，容量越大，基线越高，直到该类型的上限。规格表第一行的「最大 IOPS」是上限，不是 40 GiB 或 50 GiB 系统盘的实际基线。

阿里云 ESSD 的公式（[ESSD](https://www.alibabacloud.com/help/en/ecs/user-guide/essds)，2026-09-01）是：

| 性能级别 | 可买容量 | 单盘 IOPS 上限 | 基线 IOPS |
| :--- | :--- | :--- | :--- |
| PL0 | 1–65,536 GiB | 10,000 | min{1,800 + 12 × 容量, 10,000} |
| PL1 | 20–65,536 GiB | 50,000 | min{1,800 + 50 × 容量, 50,000} |
| PL2 | 461–65,536 GiB | 100,000 | min{1,800 + 50 × 容量, 100,000} |
| PL3 | 1,261–65,536 GiB | 1,000,000 | min{1,800 + 50 × 容量, 1,000,000} |

按这个公式，40 GiB 的 PL0 是 1,800 + 12 × 40 = 2,280 IOPS，40 GiB 的 PL1 是 1,800 + 50 × 40 = 3,800 IOPS。PL0 要到 684 GiB 才触及 10,000（683 GiB 时是 9,996）。PL1 要到 964 GiB 才触及 50,000。吞吐同样随容量上升：40 GiB 的 PL1 是 120 + 0.5 × 40 = 140 MB/s，上限 350 MB/s 要到 460 GiB 才顶满。

同一页的举例写「20 GiB 的 PL1 最高可达 50,000 IOPS」。结合公式，这句里的 50,000 是 PL1 这一档的上限，不是 20 GiB 的基线。20 GiB 的 PL1 按公式是 2,800 IOPS。

系统盘还有另一道上限。阿里云限制页（2026-09-04）写：Linux 系统盘 20–2,048 GiB，Windows 40–2,048 GiB，FreeBSD 30–2,048 GiB。数据盘可以到该类型的最大容量，系统盘不能跟着数据盘的上限走。PL2 最低 461 GiB、PL3 最低 1,261 GiB，拿来做系统盘会把系统盘买得很大，第一台网站机器用不上。

实例规格还有自己的存储 IOPS 上限。多块盘的基线加起来超过它时，以实例规格为准。入门规格不要套用存储增强型规格页上的数字。

腾讯云五种云盘的单盘容量上限都是 32,000 GB，公式写在 [云盘类型](https://www.tencentcloud.com/document/product/362/31636)（2025-06-09）。表头容量单位是 GB，公式里是 GiB。计算时直接用购买页上的容量数字，不要自行把 1000 和 1024 换算一遍。以系统盘用量 FAQ 里的 50 GB 默认容量代入基线公式：

| 类型 | 50 GB 的基线 IOPS | 基线上限 | 基线顶满大约要 |
| :--- | :--- | :--- | :--- |
| 高性能云硬盘 | 1,800 + 8 × 50 = 2,200 | 6,000 | 525 GB |
| 均衡型 SSD | 1,800 + 15 × 50 = 2,550 | 10,000 | 547 GB |
| SSD 云硬盘 | 1,800 + 30 × 50 = 3,300 | 26,000 | 807 GB |

增强型 SSD、极速型 SSD 可以在基线之外再买额外性能，文档里的 10 万、100 万 IOPS 是叠加上限，不是默认基线。增强型 SSD 的性能只在 2020-08-01 之后创建的 S5、M5、SA2 以及更新代规格上得到保证。极速型 SSD 不能做系统盘，也不能单独购买，只能跟指定实例规格一起买，而且不能从别的云盘类型升级过来。这两种的可选可用区比高性能、均衡型少。

文档给出的单路随机读写延迟是厂商测试口径：高性能云硬盘 0.8–5 毫秒，均衡型 SSD 和 SSD 云硬盘 0.5–3 毫秒，增强型 0.3–1 毫秒，极速型 0.1–0.5 毫秒。这不是某一台国际站实例的实测。

## 第一台机器怎么选

- 网站、API、后台：阿里云用 ESSD PL0 或 PL1；腾讯云用高性能云硬盘或均衡型 SSD。只看「最大 IOPS」去买 PL3 或极速型 SSD，容量下限和实例限制会把账单抬高，小容量也吃不到那个上限。
- 数据库、上传文件、希望重装系统后还在的数据：另挂一块数据盘，和实例放在同一可用区。系统盘只放系统和程序。
- 业务只有一份数据时，不要放在本地盘或弹性临时盘上。本地盘在宿主机上，不能单独购买，费用含在实例规格里；腾讯云终止实例时，本地盘和非弹性云盘一起释放。弹性临时盘在阿里云文档里是临时高性能存储，按容量计费，不是数据盘的替代。
- 阿里云上一代的普通云盘、高效云盘、SSD 云盘，文档写明部分地域在停售。替代关系是：PL0 或 ESSD Entry 替代高效云盘和普通云盘，ESSD AutoPL 替代 SSD 云盘。ESSD Entry 只能挂到通用型 U 和经济型 e。购买页没有这些旧类型时，不要按旧教程去找。
- ESSD AutoPL 把容量和性能拆开，突发性能默认开启。每小时突发 I/O 不超过 10 万次时，这一小时不另收突发费；超过之后按突发规则计费，并有费用上限。这 10 万次不是磁盘的 IOPS 上限。预配置性能只有在你打开并填写 IOPS 之后才按量计费。第一台机器如果购买页给的是普通 ESSD，就不必为了「可突发」改成 AutoPL。见 [块存储计费](https://www.alibabacloud.com/help/en/ecs/block-storage-devices)（2026-09-21）。

单价按地域和类型变化。阿里云计费页用中国杭州地域举过 PL0、PL1 的例子，不能当成香港或新加坡的价格。腾讯云按量和包年包月的单价在 [价格总览](https://www.tencentcloud.com/document/product/362/2413)，额外性能是可选项。以下单页和当月账单为准。

## 买完之后，数据盘可能还不能用

阿里云写明：数据盘创建并挂上之后，要先初始化、挂载文件系统，才能使用。控制台可以用云助手做，云助手只支持 GPT 分区和 ext4。分区和创建文件系统会清空这块盘，盘上已经有数据时不要走初始化。命令行步骤以 [初始化数据盘（Linux）](https://www.alibabacloud.com/help/en/ecs/user-guide/initialize-a-data-disk-whose-size-does-not-exceed-2-tib-on-a-linux-instance)（2026-08-31）为准。

腾讯云新买的数据盘同样要初始化。整块盘只做一个文件系统时，初始化说明建议直接在裸设备上创建文件系统，不必先分区；要分成多块逻辑盘时再分区。小于 2 TB 可以用 MBR，2 TB 及以上要用 GPT。MBR 最大 2 TB，GPT 在云盘上的实际上限是云盘容量上限 32 TB。见 [初始化场景](https://www.tencentcloud.com/document/product/362/31596)。

系统盘不要用第三方工具再切一刀。腾讯云系统盘用量说明建议不要给系统盘分区，否则可能无法启动或丢数据。阿里云系统盘在创建实例时已经自动初始化。

登录之后用系统里的磁盘列表看容量，不要只看购买页。购买页显示 100 GiB、系统里只看到系统盘，多半是数据盘还没初始化，不是没买到。

## 只能扩大，不能缩小

两边都不支持把已有云盘缩小。腾讯云的建议是：新建一块更小的盘，挂到同一台实例，把数据拷过去，再释放旧盘。阿里云扩容文档同样写明不能缩小，扩容前要按这个约束估容量。

扩容只是把云盘容量变大。分区和文件系统不会自动跟着变大，不扩展的话，系统里仍是原来的可用空间。阿里云可以在线扩容，扩完不用重启；磁盘未挂载、实例已停止，或该规格只支持离线扩容时，用离线方式。有分区时只能扩展这块盘上的最后一个分区。正在创建快照的盘要等快照完成。还有一个小盘碰不到的条件：扩容前容量已经大于 16 TiB、又不是 8 GiB 的整数倍、文件系统是 ext4，并且内核落在受影响版本时，在线扩容可能写坏数据，要先按文档迁移系统或升级内核。见 [扩容 Linux 云盘](https://www.alibabacloud.com/help/en/ecs/user-guide/resize-linux-cloud-disks)（2026-09-17）。腾讯云在扩容流程里有扩展分区和文件系统这一步；Linux 系统盘如果没能在线扩展，而 `/etc/cloud/cloud.cfg` 里有 `growpart` 和 `resizefs`，重启后由 cloud-init 完成，否则要按操作系统文档手动扩展。

MBR 分区不要扩过 2 TiB。阿里云 ResizeDisk 说明写明，强行扩过可能导致数据丢失；要更大容量时，新建 GPT 数据盘再迁移数据。

腾讯云系统盘用量 FAQ（2024-01-06）写：新购默认 50 GB；镜像小于 50 GB 时，系统盘仍按最低 50 GB，不跟着镜像缩小。2022 年 12 月的更新日志曾写 Linux 最低可选容量从 50 GB 降到 20 GB。FAQ 更新更晚，仍写最低 50 GB。两处没有互相改掉，以下单页当时能选的最小值为准。云盘系统盘可以扩到 50 GB 以上；重装系统本身不会把云盘系统盘扩容。系统盘容量不能通过重装来缩小。

阿里云扩容按新增容量计费，扩容时不能顺便改计费方式。腾讯云包年包月和按量实例都可以扩容，扩容前云盘不能处于隔离状态。

## 实例删了，盘还在不在

云盘从创建开始按容量计费，和有没有挂载、里面有没有写入无关。卸载数据盘不会停止计费，要停止就得释放。释放会永久删除数据，先做快照。手动快照不随云盘释放，释放之后快照费还在；自动快照是否一起删，看「释放云盘时删除自动快照」这个开关。快照费见 [快照与备份](cloud-snapshot-backup-guide.md)。

阿里云「随实例释放」可以在创建时选，也可以之后在磁盘属性里改：

- 系统盘默认开启。开启时，即使这块系统盘后来被卸下、挂到别的实例当数据盘，释放源实例仍会把它一起释放。关闭后，释放实例时它变成一块留下的数据盘。
- 数据盘开启时，只释放当时挂在这台实例上的盘。已经卸载的盘不会被带走。
- 关闭时，包年包月数据盘会转成按量并保留，按量数据盘直接保留，保留下来的盘继续按量计费。
- 中国内地地域若要保留这些盘，账号需要完成实名认证。见 [阿里云身份验证](aliyun-international-account-verification.md)。
- 开启了多重挂载的盘不能设置随实例释放。
- 实例处于安全锁定时，DeleteInstance 文档写明：即便云盘的「不随实例释放」也会被忽略，盘仍随实例释放。

腾讯云有两套说法，释放前要对照控制台，不要只记其中一句。

[终止实例](https://www.tencentcloud.com/document/product/213/4930)（2025-12-31）写：本地盘和非弹性云盘随实例释放，数据丢失；弹性云盘走自己的生命周期。同一天更新的 [生命周期](https://www.tencentcloud.com/document/product/213/4856) 又写：购买时指定的系统盘和数据盘会释放，之后另外挂上的云盘不受影响。终止接口 `TerminateInstances` 的 `ReleasePrepaidDataDisks` 默认是 `false`，也就是默认不释放挂在实例上的包年包月数据盘。

[恢复实例](https://www.tencentcloud.com/document/product/213/4931) 写明：按量实例在回收站的保留期内，CPU 和内存不再计费，按量云盘继续计费。手动终止且未欠费的按量实例保留 2 小时，期满才释放；定时终止不进回收站，到点直接释放。保留期满、实例被释放时，弹性云盘会被卸载并进入云硬盘回收站。包年包月云盘手动退还后进入回收站 7 天，状态变为待回收就不再计费，7 天内续费可以恢复，也可以在回收站里永久删除。按量云盘手动终止立即生效，数据不能恢复。按量云盘自身还有一条欠费规则：余额为负超过 24 小时会被自动终止。实例欠费停机的时间线是另一套，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。两套时间对不上时，以控制台里这块盘的状态为准。

弹性公网 IP 不会因为释放实例就自动消失，空闲地址可能继续计费。那是网络资源，不是云盘，处理方式见计费方式那篇。

## 下单和释放时核对什么

1. 购买页写的是云盘，不是本地盘，也不是弹性临时盘。
2. 系统盘容量不低于该操作系统的下限，业务数据放到数据盘。容量一旦买下就不能缩小。
3. 云盘类型用基线公式心算一遍，确认自己买的是这块容量的 IOPS，不是规格表上的上限。
4. 数据盘和实例在同一可用区。看清「随实例释放」是开还是关。
5. 创建完成后，在系统里确认数据盘已经有文件系统并挂载。没有就按初始化文档做，不要对已有数据的盘格式化。
6. 释放实例前先做快照，再看系统盘、数据盘、快照、弹性公网 IP 分别会不会留下。留下的按量云盘和快照会继续出账。

## 常见问题

**40 GiB 的 PL1 为什么不是 5 万 IOPS？**
5 万是 PL1 的单盘上限。基线是 1,800 + 50 × 容量，40 GiB 时是 3,800。实例规格的存储上限更低时，以规格为准。

**关机之后云盘还扣费吗？**
扣。节省停机和关机不收费停的是计算资源，云盘继续计费。只有释放云盘才停止云盘容量费。

**把实例退掉，数据盘会不会一起没了？**
看它是不是随实例释放，以及它是不是弹性云盘。阿里云未挂载的数据盘不会被实例释放带走。腾讯云两份文档对「购买时一起买的数据盘」是否释放并不一致，以终止窗口里的选项和磁盘列表为准。没确认之前先做快照。

**云盘能改成另一种类型吗？**
阿里云可以变更云盘类型，也可以调整 ESSD 的性能级别，变更后按新类型计费，入口以控制台为准。腾讯云写明增强型 SSD 和极速型 SSD 不能从其他类型升级而来。

## 账号从哪里来

云盘是账号里的资源，选哪种盘不会新开一个云账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [Block storage overview](https://www.alibabacloud.com/help/en/ecs/user-guide/elastic-block-storage-devices)
- [ESSDs](https://www.alibabacloud.com/help/en/ecs/user-guide/essds)
- [Block storage devices（计费）](https://www.alibabacloud.com/help/en/ecs/block-storage-devices)
- [Release a cloud disk](https://www.alibabacloud.com/help/en/ecs/user-guide/release-a-disk)
- [ECS limits and quotas](https://www.alibabacloud.com/help/en/ecs/user-guide/limitations)
- [Initialize a data disk (Linux)](https://www.alibabacloud.com/help/en/ecs/user-guide/initialize-a-data-disk-whose-size-does-not-exceed-2-tib-on-a-linux-instance)
- [Resize a disk (Linux)](https://www.alibabacloud.com/help/en/ecs/user-guide/resize-linux-cloud-disks)
- [DeleteInstance](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-deleteinstance)
- [Cloud Disk Types](https://www.tencentcloud.com/document/product/362/31636)
- [Terminating/Returning an Instance](https://www.tencentcloud.com/document/product/213/4930)
- [Reclaiming/Restoring an Instance](https://www.tencentcloud.com/document/product/213/4931)
- [Life Cycle](https://www.tencentcloud.com/document/product/213/4856)
- [TerminateInstances](https://www.tencentcloud.com/document/product/213/33234)
- [Terminating Cloud Disks](https://www.tencentcloud.com/document/product/362/32399)
- [System Disk Usage](https://www.tencentcloud.com/document/product/213/17351)

## 一句话总结

系统盘放系统，业务数据放同一可用区的数据盘。规格表上的最大 IOPS 是大容量才能顶到的上限，小系统盘要用容量公式折算。云盘只能扩不能缩，从创建起就按容量计费；实例删了，没随实例释放的盘和手动快照还会继续扣费。

返回 [指南首页](index.md)
