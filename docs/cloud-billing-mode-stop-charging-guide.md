---
title: 包年包月、按量付费还是抢占式：关机之后还扣哪些费
description: 阿里云 ECS、腾讯云 CVM 的包年包月、按量付费和抢占式怎么选；节省停机与关机不收费会停掉哪些费用，云盘、镜像和公网 IP 为什么还会出账。
permalink: /cloud-billing-mode-stop-charging-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)

# 包年包月、按量付费还是抢占式

阿里云账号购买或腾讯云账号购买完成之后，创建 ECS、CVM 的页面会先问计费方式。选错的常见结果有两种：测试机关机之后余额继续下降，或者一台需要一直在线的网站被放进了可能被系统回收的抢占式实例。欠费之后多久停机，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。这篇只处理购买当时就能决定的事：三种计费方式各适合什么，以及你主动关机之后，计算、云盘、镜像和公网 IP 分别还记不记账。

轻量应用服务器和 Lighthouse 是套餐，基础套餐按包年包月卖，没有下面这套「节省停机 / 关机不收费」。套餐和云服务器的差别见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。单价、库存和按钮名称会变，以购买页和控制台为准。

## 三种方式各自在计什么

| 对比项 | 包年包月 | 按量付费 | 抢占式 |
| :--- | :--- | :--- | :--- |
| 阿里云 ECS | Subscription，预付一个月或更长 | Pay-as-you-go，按秒计量，从创建记到释放 | Spot，按秒，价格随市场变动，官方写明最高可比按量价格低 90% |
| 腾讯云 CVM | Yearly/Monthly，至少预付一个月 | 按秒计量、按小时结算（UTC+8） | 按秒计量、按小时结算。计费模式页和抢占式 FAQ 写常见成交价约为同规格按量的 3%–20%；配额表写成 “3%–20% off”。两种写法不同，以下单页显示的价格为准 |
| 适合 | 长时间开着的网站、接口、数据库 | 天数不确定的测试、临时扩容 | 中断后能从检查点或外部数据重跑的批处理 |
| 主动关机 | 已付周期继续走，到期日不变 | 默认继续收计算费，满足条件才能停掉计算费 | 阿里云可进节省停机；腾讯云两份文档对「关机不收费」是否适用说法不一致，见下文 |
| 系统回收 | 到期后按包年包月规则处理 | 由你释放；欠费会另走停机流程 | 库存或价格条件触发时，系统可以回收 |

依据：[阿里云计费方式总览](https://www.alibabacloud.com/help/en/ecs/overview-of-billing-methods)（2026-09-21）、[实例规格计费](https://www.alibabacloud.com/help/en/ecs/instance-types)、[腾讯云计算费模式](https://www.tencentcloud.com/document/product/213/2180)（2026-05-19）、[腾讯云抢占式实例](https://www.tencentcloud.com/document/product/213/17817)（2026-10-27）。

腾讯云按量实例在创建时会先冻结一小时的 CPU、内存和数据盘费用，每小时整点（UTC+8）结算上一小时的实际秒数；实例释放后，冻结的费用解冻。阿里云按量实例按秒计，最后一个计费周期有最短时长，具体秒数以计费说明为准。两边的预留实例都是用来抵扣符合条件的按量账单的权益，不是一台可以登录的机器。腾讯云另有包销模式，文档标明为内测：最短使用时间一年，购买时预付一个月并按月自动续费，包销期结束前不能退还、不能改配、不能换成其他计费方式。第一台机器用包年包月或按量即可，承诺类产品等用量稳定后再看购买页。

## 第一台机器怎么选

- 网站或 API 需要一直可访问：选包年包月。阿里云写明，订阅时长按自然月、自然年计算。2 月某日买一个月，到期日是 3 月对应的那一天，按 30 天去推会推错。见 [Subscription](https://www.alibabacloud.com/help/en/ecs/subscription)。
- 只想开几天看看地域和延迟：选按量付费。测完在控制台释放实例。只关机、不释放，标准停机模式下计算费还在走。
- 工作日使用、晚上和周末停机，又希望系统盘留着：选按量付费，再用下一节的节省停机或关机不收费。接受重启时可能没有库存。
- 渲染、转码、可重跑的分析任务：才考虑抢占式。业务数据另有副本。

包年包月实例上仍可能出现按量账单。按流量计费的公网带宽、按量数据盘、弹性公网 IP、手动快照，都会在预付周期之外单独扣余额。阿里云订阅说明把这些列成常见原因。流量计费可结合 [CDT 免费流量](aliyun-international-cdt-free-traffic.md) 看，快照费用见 [快照与备份](cloud-snapshot-backup-guide.md)。

还有一条和内地网站直接相关的限制：阿里云计费总览写明，中国内地地域的网站备案只接受订阅时长至少三个月、并且购买了公网带宽的包年包月 ECS。按量付费和抢占式不能用来申请这项备案。要买内地地域，还要先完成账号身份验证，见 [阿里云身份验证](aliyun-international-account-verification.md) 和 [地域怎么选](cloud-region-selection-guide.md)。

## 阿里云：标准停机和节省停机

按量实例（含抢占式）在 VPC 里，并且没有本地盘、没有持久内存时，可以在控制台关机时把 Stop Mode 选成 Economical Mode（节省停机，旧称「停机不收费」）。带本地盘的大数据型、本地 SSD 型，以及带持久内存的规格，不支持。经典网络实例也不支持。

节省停机释放的是 vCPU、内存、GPU、FPGA 和系统分配的公网 IP，私网 IP 保留。系统盘、数据盘和相关快照保留。计算资源的计费停止。云盘继续计费。按带宽计费的弹性公网 IP，其带宽费继续计费；EIP 不会因为这次关机被释放。配置费、绑定费在部分条件下减免，条件以节省停机页所链的 EIP 计费说明为准。

镜像许可证在文档里有一处对不上。[Stop an instance](https://www.alibabacloud.com/help/en/ecs/user-guide/stop-an-instance)（2026-04-28）写的是镜像停止计费。[Economical mode](https://www.alibabacloud.com/help/en/ecs/user-guide/economical-mode)（2026-07-20）开头也写镜像许可证停止计费，同一页的对照表又把 image licenses 放进继续计费的项目。以当月账单里是否还有镜像条目为准，核对方法见 [账单与用量](cloud-international-billing-usage-guide.md)。

操作时注意四点：

1. 在 ECS 控制台对运行中的实例执行 Stop，并把停机模式选成 Economical Mode。停稳后状态为 Stopped，并带 Economical Mode 标记。在操作系统里执行 `shutdown`、`poweroff` 或 `halt`，不会进入节省停机。
2. 调用 StopInstance 时把 `StoppedMode` 设为 `StopCharging`。实例不满足条件时，这个调用不会报错，实例会按标准停机停下，计算费继续。要用 DescribeInstances 或控制台上的标记确认。已经处于标准停机时，不能直接改成节省停机，需要先启动，再按节省停机停一次。
3. 系统分配的公网 IP 会被释放，下次启动可能换成新地址。需要固定地址时，先把系统公网 IP 转成 EIP，再节省停机。EIP 保留，并可能继续产生费用。
4. t5、t6 这类突发性能实例上积累的 CPU 积分会在进入节省停机时清零。启动后的突发能力从零开始积。

启动失败是预期中的风险。计算资源已经交回去，热门地域、高峰时段可能返回 `OperationDenied.NoStock`。可以稍后重试，或更换实例规格。账号欠费时同样可能起不来。欠费自动停机也会进入节省停机，时间线见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。

标准停机则保留全部资源，计算、磁盘、镜像和公网带宽都继续计费，重启不依赖当时的库存。短时间停机维护、必须马上开回来的机器，用标准停机。

控制台每次关机默认选中标准停机。可以在 ECS 概览的 Custom Settings 里把节省停机设成默认选项，下次仍会弹出选择。官方给的例子是工作日白天使用的开发测试机，用运维编排在固定时间节省停机，生产环境要先接受「到点可能开不回来」。

## 腾讯云：关机不收费

按量 CVM 在系统盘和数据盘都是云硬盘时，关机窗口里可以选择 CVM No Charge When Shutdown（关机不收费）。生效后 CPU 和内存被释放，这两项停止计费；云盘和镜像继续计费。公网 IP 被释放。若要保留地址，先把公网 IP 转成 EIP。文档写明：关机之后，处于闲置状态的 EIP 不收费，绑定的 EIP 在关机和再次启动后仍然保留。见 [Shutting Down Pay-as-You-Go Instances to Avoid Charges](https://www.tencentcloud.com/document/product/213/19918)。

下面这些情况，同一页写明不适用：

- 登录到实例内部再关机。
- 挂了本地盘。
- 抢占式实例。
- 因为欠费被关机。欠费停机走的是另一套回收流程。
- 调整配置、调整磁盘、重装系统等运维操作导致的关机。这段时间不套用关机不收费，CPU 和内存继续计费。

关机不收费期间，阶梯价所累计的使用时长暂停，启动后继续累计，不会把已有时长清零。此期间除了启动，文档列出的调整配置、调整磁盘、调整网络、重装系统、重置密码、续费、重命名都不能做，要先启动。

API 的 `StoppedMode` 默认是 `KEEP_CHARGING`（关机后继续计费）。要停计算费，必须显式传 `STOP_CHARGING`。一批实例里如果混有不符合条件的机器，符合条件的停 CPU 和内存费，不符合的照常计费。

重启可能因为库存不足失败，可以稍后重试或更换规格。

抢占式实例要单独看。上面的限制页把抢占式列在不适用里；[抢占式实例 FAQ](https://www.tencentcloud.com/document/product/213/17817)（2026-10-27）对「是否支持关机不收费」的回答是 Supported。两页冲突。以关机窗口里是否出现该选项为准。选项不出现时，按继续计费来做余额准备。

## 两边对照

| 项目 | 阿里云节省停机 | 腾讯云关机不收费 |
| :--- | :--- | :--- |
| 适用 | VPC 中的按量实例，含抢占式；无本地盘、无持久内存 | 系统盘和数据盘都是云硬盘的按量实例 |
| 停掉的费用 | vCPU、内存、GPU、FPGA，以及系统公网 IP 的按带宽费用 | CPU、内存 |
| 继续的费用 | 系统盘、数据盘、快照；按带宽计费的 EIP。镜像见上一节的文档差异 | 云盘和镜像 |
| 公网 IP | 系统分配的地址释放，启动后可能变化；EIP 保留 | 普通公网 IP 释放；EIP 保留，文档写闲置 EIP 不收费 |
| 私网 IP | 保留 | 该功能页未写私网 IP 的变化，以控制台为准 |
| 系统内关机 | 不进入节省停机 | 不进入关机不收费 |
| 欠费导致的关机 | 官方 FAQ 写明会进入节省停机 | 明确不适用本功能 |
| 抢占式 | 可以主动节省停机；被系统中断时是否停进节省停机，取决于创建时的中断行为 | 限制页写不适用，FAQ 写支持 |

## 抢占式实例被收回时数据在哪里

阿里云抢占式实例在创建时选定保护期和中断行为，创建后不能改。保护期默认 1 小时；只有明确选 None 才没有保护期。保护期内不因市场价格或库存被中断。保护期结束后，市场价高于出价，或该规格库存不足，就会触发中断。

中断行为有两个：

- Release：计算资源、系统公网 IP、固定带宽和云盘，按「什么是抢占式实例」的说明会释放。实例先进入待释放，大约 5 分钟后释放，这个状态不能撤销。节省停机 FAQ 又写，云盘是否留下取决于创建时的「随实例释放」。创建页上如果可以取消随实例释放，以该选项为准，并另外做快照。
- Stop in Economical Mode：中断后 5 分钟内进入节省停机，云盘、EIP 和快照保留并继续计费，静态公网 IP 回收。市场价或库存不满足时，可能启动失败。

你在控制台主动做一次节省停机，不会改变中断行为。中断行为仍是 Release 时，系统回收仍可能把实例释放掉。

腾讯云按市场价计费，你填写的最高价是出价上限。库存不足导致的回收，不能靠把出价抬高来避免。系统回收前大约 2 分钟，可以通过实例元数据看到终止时间。计费模式页写明：系统主动回收时不保留实例数据。抢占式不能升降配，也不能转成包年包月。

## 包年包月关机，到期日不动

[Stop an instance](https://www.alibabacloud.com/help/en/ecs/user-guide/stop-an-instance) 写明，包年包月实例关机后计费继续，到期日不受影响。腾讯云的关机不收费页只覆盖按量实例。包年包月在计费模式页里是预付：按所选资源配置扣费，到期后自动关机。该页没有把「有效期内主动关机」写成可以暂停这段预付费用。

阿里云订阅页（2026-08-04）对到期之后的描述，页面内部有两套说法，引用时需要一起看：

- 未开启自动续费：服务在到期时停止。自动续费扣款失败：停服时间落在到期后第 15 天 00:00 到第 16 天 00:00 之间。
- 同一页的资源表又写：到期后 15 天内计算资源仍保留且实例可用；满 15 天后停服但资源仍在；满 30 天后释放计算资源和订阅云盘，数据不能恢复。到期后、释放前，控制台会出现备份入口，可以做自定义镜像或快照。

两段不一致。安排下线日期时，以控制台里这台实例的到期时间和状态为准，并在到期前做快照。腾讯云包年包月到期后的隔离和回收，沿用 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) 里已经引用的官方说明，这里不另写一套时间表。

提前不再使用阿里云包年包月实例时，计费总览的路径是先把计费方式改成按量，再释放。控制台是否给出变更入口，由官方按该实例的使用情况决定。没有按钮，就表示当前不能改。变更后可能退回一部分预付费用，金额以变更页为准。改成按量之后，余额要盖住后续按量账单。

腾讯云计费模式页写明：按量可以转包年包月；包年包月可以转包销；抢占式、预留实例和包销不能再转成其他模式。

阿里云包年包月和按量可以互转，抢占式不能转，也不能改规格、改带宽。云市场里按订阅售卖的镜像，只给出包年包月实例使用。

## 账号到手后建议核对的顺序

1. 在购买页看清计费方式，以及公网是按带宽还是按流量。按流量的出方向费用可能在 CDT 或 EIP 账单里，不一定挂在 ECS 行上。
2. 测试机用完后释放。还要保留系统盘时，再使用节省停机或关机不收费，并在关机前处理公网 IP。
3. 关机后看状态标记或关机选项，再等下一张按量明细。明细里仍有实例规格费用，说明这次停机没有停掉计算费。
4. 给余额设提醒。按量实例在腾讯云侧欠费后的缓冲很短，见欠费指南。
5. 渠道结算的账号，付款方式保持交付时的状态。开通按量或包年包月，都不需要自己去绑一张国际卡。验收见 [阿里云代开](aliyun-agency-opening.md) 和 [腾讯云代开](tencent-cloud-agency-opening.md)。

## 常见问题

**关机后余额还在降，是不是没关干净？**
先看是不是在系统内部关机，以及是不是标准停机。云盘在两种「停计算费」模式下都继续计费。阿里云的快照和按带宽 EIP 也会继续。把这些行从账单里拆出来，才能判断计算费有没有停。

**包年包月关掉几天，到期日会不会往后推？**
不会。阿里云停机说明写的是到期日不受影响。

**抢占式能当正式网站的唯一服务器吗？**
两边都允许系统在库存或价格条件满足时回收。腾讯云回收时不保留实例数据。阿里云只有在创建时把中断行为设为节省停机，云盘才会按该模式保留，而且之后可能启动失败。

## 账号从哪里来

计费方式是账号里的资源属性，换一种计费方式不会新开一个云账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [Overview of billing methods](https://www.alibabacloud.com/help/en/ecs/overview-of-billing-methods)
- [Subscription](https://www.alibabacloud.com/help/en/ecs/subscription)
- [Billing methods for ECS instance types](https://www.alibabacloud.com/help/en/ecs/instance-types)
- [Economical mode](https://www.alibabacloud.com/help/en/ecs/user-guide/economical-mode)
- [Stop an instance](https://www.alibabacloud.com/help/en/ecs/user-guide/stop-an-instance)
- [What is a spot instance?](https://www.alibabacloud.com/help/en/ecs/user-guide/what-is-a-spot-instance)
- [Billing Modes](https://www.tencentcloud.com/document/product/213/2180)
- [Shutting Down Pay-as-You-Go Instances to Avoid Charges](https://www.tencentcloud.com/document/product/213/19918)
- [Spot Instances](https://www.tencentcloud.com/document/product/213/17817)

## 一句话总结

一直在线用包年包月，临时测试用按量并在用完后释放，可中断任务才用抢占式。按量实例只有在控制台或 API 里明确选择节省停机、关机不收费，计算费才会停；云盘仍然计费，系统分配的公网 IP 会被释放。在操作系统里点关机，两边都不会触发这种停计费。

返回 [指南首页](index.md)
