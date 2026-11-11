---
title: 按量能转包年包月吗：公网跟不跟，腾讯云转不回去
description: 阿里云国际站 ECS 按量和包年包月怎么互转，系统盘、数据盘、固定公网 IP 和 EIP 哪些跟着变，转到按量要占每月退款额度。腾讯云国际站 CVM 只写了按量转包年包月。
permalink: /cloud-billing-method-switch-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md)

# 按量能转包年包月吗

测试机先用按量开着，跑顺了再想改成包年包月。我把阿里云国际站的两份转换说明、退款规则、计费 FAQ，和腾讯云国际站的计费模式、转换页、ModifyInstancesChargeType 对了一遍。阿里云两个方向都能转，公网带宽和 EIP 不跟着「按量转包年包月」走。腾讯云计费模式页只写了按量转包年包月。包年包月要换，只指向包销。

三种计费方式怎么选、关机之后还扣哪些费，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。轻量应用服务器和 Lighthouse 按套餐卖，没有下面这套互转，见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。按钮名称和可退金额以控制台为准。

## 两边各能往哪边转

| 现在的计费方式 | 阿里云 ECS | 腾讯云 CVM |
| :--- | :--- | :--- |
| 按量付费 | 可以转包年包月。实例、系统盘、镜像许可跟着转。数据盘看你勾不勾。公网带宽和 EIP 不在这次里 | 可以转包年包月。付清才生效。新周期从转换成功时起算 |
| 包年包月 | 可以转按量，退未用完的现金部分，并占用当月退款额度 | 计费模式页只写可以转包销。参数说明写接口只支持按量转包年包月 |
| 抢占式 | 不能转 | 不能转 |
| 预留实例、节省计划 | 抵扣账单的权益，不是一台能改计费方式的机器 | 预留实例不能转成别的计费方式。包销也不能再转 |

依据：[按量转包年包月](https://www.alibabacloud.com/help/en/ecs/change-the-billing-method-of-an-ecs-instance-from-pay-as-you-go-to-subscription-1)（2026-08-20）、[包年包月转按量](https://www.alibabacloud.com/help/en/ecs/change-the-billing-method-of-an-instance-from-subscription-to-pay-as-you-go-1)（2026-08-20）、[腾讯云计算费模式](https://www.tencentcloud.com/document/product/213/2180)（2026-05-19）、[按量转包年包月（CVM）](https://www.tencentcloud.com/document/product/213/59353)（2025-09-05）。

转的是这台已经在跑的实例。阿里云按量转包年包月的说明写，转换不影响运行状态。腾讯云那一页没写会不会重启。两家的新到期日，我只采用写明了的那一句：腾讯云从转换成功的时刻起按包年包月计。阿里云付款成功后，到实例详情里看计费方式和到期时间。

## 阿里云：按量转包年包月，公网留在按量

实例要在运行中或已停止。停在节省停机里的，先开机再转。抢占式不能转。退役规格不能转。有未支付订单，先付款或取消。设了自动释放时间的，先关掉自动释放。账号有欠费时，接口直接拒绝。

跟着变成包年包月的，是实例规格、系统盘、镜像许可。挂着的按量数据盘可以一起转，也可以留在按量。接口 `IncludeDataDisks` 默认是 false，不传就不转数据盘，而且是整台实例上的按量数据盘一起转，不能用这个参数只挑一块。控制台是一个勾选项。勾了什么，以付款页为准。

计费总览（2026-09-21）另有一句：按量实例上的系统盘和数据盘，必须跟实例一起改计费方式。转换页和接口写的是数据盘可以不勾。系统盘两处都是跟着实例走。数据盘以转换页的勾选为准。总览那句对得上的情况是：实例还停在按量时，不能单独把盘先改成包年包月。同一页还写，控制台没有对应菜单，就不能转。能不能转，阿里云按用量指标决定。

公网带宽留在按量。计费 FAQ（2026-08-28）写得更直：按量转包年包月只改实例和磁盘。要改带宽的按流量或按带宽，走升降配那条，见 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。EIP 是单独买的，这次也不跟着转。带宽或 EIP 还在按量扣的话，包年包月机器仍会因为余额不够停服务，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。按流量的出方向可以对照 [CDT 免费流量](aliyun-international-cdt-free-traffic.md)。

时长在接口里按月只列了 1 到 9 个月，以及 12 个月。控制台能选的档，以那一页为准。一次最多 20 台，同一批用同一个时长。转换页可以打开自动续费。这次付的是你选的这段时长。后面哪天再扣，见 [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md)。

限制页（2026-09-04）写，按量转包年包月在「实例台数或配额」上没有限制，退役规格不能转。同一张表又把 vCPU 配额按计费方式分开列。接口仍有 `QuotaExceed.ElasticQuota`，目标规格在该地域的配额用满时会报这个错。两处口径不完全一样。转换前看配额中心里，这台规格的包年包月还剩多少。

市场镜像在接口里有 `InvalidMarketImageChargeType.NotSupport`。分销商账号还会碰到另一句：市场里 ISV 的服务不能由分销商下单。机器用的是市场镜像时，先别转。

下单之后、付款之前，实例状态变了，这次转换失败。把实例恢复成下单时的状态，再去 Order Hub 付款。这中间如果升了配，订单金额和现配置对不上，这笔未支付订单作废。先取消，再按新配置重下。

## 阿里云：包年包月转回按量

想在到期前把包年包月实例放掉，计费总览写必须先转成按量。到期不续、等系统自己释放，是另一条路，见 [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md)。转完之后实例按量计费，余额要够，否则会停服。目标是停掉这台机器时，转完就释放。只转不释放，计算费会按秒接着走。释放时数据盘会不会留下，见 [系统盘与数据盘](cloud-disk-system-data-guide.md)。

这个方向会退差价，所以多几条限制。已经到期的不能转。公共镜像里的 Red Hat Enterprise Linux、SUSE Linux Enterprise Server 不能转。市场镜像不能转。计费总览写，市场里的订阅镜像只有包年包月实例能用，按量和抢占式不能用，和这条对得上。当月退款额度不够不能转。临时升级带宽的那段时间不能转，错误码是 `InvalidInstance.TempBandwidthUpgrade`。有未支付订单、刚改过配置，计费 FAQ 也把它们列成失败原因。

跟着改成按量的，是实例规格、系统盘、随这台实例一起买的包年包月数据盘，以及绑定的固定公网 IP。按带宽还是按流量，这一项不跟着改。EIP 仍走自己的计费方式。后来单独买的数据盘，转换说明没有写进「跟着实例一起转」。要改就用磁盘自己的入口，转换页的 FAQ 指到了那一页。

还没生效的续费订单、升配订单，转的时候按全额退。备案、故障或机房迁移送的时长作废，这段没有可退金额。活动折扣也作废。以后再转回包年包月，不一定还能用原来的折扣。

接口写，新的计费方式覆盖这台实例的整个生命周期，退款回到原支付方式，已核销的代金券不退。以后仍可以再转回包年包月，上一节的条件还要再满足一次。

实例如果是在停止状态下转成按量的，就算账号开了节省停机，它也不会自动进节省停机。要先开机，再关机，节省停机才会套上。节省停机会放掉哪些资源，见计费方式那篇。

中国内地地域做网站备案，[计费总览](https://www.alibabacloud.com/help/en/ecs/overview-of-billing-methods)（2026-09-21）要求包年包月至少三个月，并且买了公网带宽。转成按量之后还算不算满足备案条件，以备案页当时的校验为准。身份验证见 [阿里云身份验证](aliyun-international-account-verification.md)。

## 退款额度，和不满 30 天的 1.5 倍

可退金额是现金实付减去已消耗。现金不含代金券。这是退款规则页（2026-06-25）的算法，和降配不是同一页。降配公式见 [升降配](cloud-instance-type-change-guide.md)。转换窗口上的数字为准。

已用天数不满 30 天，ECS 的已消耗要再乘 1.5。满 30 天不再乘。不足一天按一天算。退款规则页的例子：1 月 1 日 12:00 买一年，1 月 10 日 14:00 转按量，已用天数算 10 天。同一天下午 14:00 就转，已用天数算 1 天。订单实际时长向下取整到整天。1 月 1 日 12:00 买一个月、订单结束时间是 2 月 2 日 00:00，实际购买时长算 31 天。这是那一页上的例子，不是报价。

新购、续费、降配订单的日单价，用原订单价格除以这段实际购买时长。升配订单还要再乘配置差价比例，比例里用的是目录价，不含折扣。同一页有完整式子。乘过 1.5 之后，已消耗可能不低于现金实付。页面显示 0，就按 0。

退款额度是另一套计数，不是美元。接口把每次占用写成：vCPU 数 ×（剩余天数 × 24 ± 浮动小时）。浮动小时没有给计算式。转换页上的剩余额度，才是这次能不能转的依据。用完了等到下个月 1 日，当月额度不结转。错误码写成 `QuotaExceed.RufundVcpu`，官网就少了一个 e。额度不能在限制页里申请提高。

一次也是最多 20 台。转成按量之后占的是按量配额，报错是 `QuotaExceed.AfterpayInstance`。RAM 子用户要有退款权限，否则是 `NoPermission.Refund`。子用户怎么授权，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

## 腾讯云：付清才算转上，转不回去

按量转包年包月会生成一笔订单。文档前面叫续费订单，改配置那段又叫新购订单。没付或付款失败，订单留在订单管理里。付清之前不能再转一次。付清之前如果改了配置、重装、调了带宽或磁盘，订单金额和实例对不上，这笔不能再付。先取消，再重新转。

付清之后立刻按包年包月计。新周期的起点是转换成功的时刻，不是当初按量创建的那天。转换页写，这种转出来的包年包月不支持七天无理由。购买页（2025-07-03）另写，包年包月的 CVM 不可退，付款前先核对配置。2024 年 4 月的更新日志写，到期前不支持提前退款。降配仍可能退差价，那是改规格，见升降配那篇。

控制台那句写的是，支持把实例和磁盘的计费方式一起转。接口 `ModifyPortableDataDisk` 默认 false，不传就不转弹性数据盘。窗口里有没有磁盘选项，以那一页为准。

网络只有两类会自动改成按带宽的包年包月：标准账号上、普通公网 IP 的按小时带宽；传统账号的按小时带宽。其余网络计费方式保持原样。标准账号和传统账号的差别见公网 IP 那篇。网络按带宽使用时长计费的，限制里写暂时不能转。

这些情况也不能转：可用区里剩下的包年包月配额，小于这次要转的台数；本来就不是按量；抢占式；市场镜像；批量计算的 BC1、BS1；已经有未完成的转换订单；设了定时销毁。定时销毁先取消再转。接口还写，关机不收费的实例不支持这次操作。促销期内买的机器，错误码 `FailedOperation.PromotionalPerioRestriction` 写不能改配置，也不能改计费方式。Red Hat 有单独的 `UnsupportedOperation.RedHatInstanceUnsupported`。商业镜像是 `UnsupportedOperation.CommercialImageChangeChargeType`。

批量转换时，每台的购买时长必须相同。接口一次最多 20 个实例 ID。时长错误码列出的月份是 1 到 12，以及 24、36。控制台能选的档以页面为准。这里也可以勾自动续费，扣款日仍按自动续费那篇。

计费模式页没有「包年包月转回按量」。`ModifyInstancesChargeType`（2026-05-27）的参数说明也写，只支持按量转包年包月。同一页的示例 1 却传了 `POSTPAID_BY_HOUR`。参数说明和计费模式页是同一套口径，示例和它们不一致。控制台没有转回按量的按钮时，我不以这个示例为准。

包年包月如果还要换，计费模式页只指向包销。包销标成内测。期结束前不能退、不能改配、不能再换计费方式。细节在计费方式那篇，按量实例的转换列里没有包销。

## 渠道账号上怎么对

转换发生在已经有的那个主账号上，不会新开一个号。按量转包年包月要先付清这段时长，余额要在付款前到账。已有账号按账号 ID 充值，见 [国际云充值](https://www.yundaxian.com/recharge)。付完到实例详情里看计费方式有没有变成包年包月。只看到余额少了、计费方式没变，这次还没转上。

控制台是下单后再付款。走 `ModifyInstanceChargeType` 时，自动支付默认开着，余额不够会生成只能取消的异常订单。把 `AutoPay` 设成 false，才会留下可以去控制台付的未支付订单。渠道账号不要为了支付去改付款设置、自己绑卡。没有默认付款方式或未被授权时，接口返回 `InvalidAccountStatus.PayAmountLimitExceeded`。

退订规则（2026-10-23）另写，分销客户不支持 ECS 退订。渠道结算如果落在这一类里，自助退订页可能没有入口。那是退订，不是上面的转换。转换页没有把分销客户写成一条禁止项。按钮不在，就停在这一步。

包年包月转按量的退款，接口写回到原支付方式。落到账号余额还是原支付渠道，以订单和账单为准。

## 常见问题

**转换完，公网还会扣余额吗？**
阿里云按量转包年包月时，公网带宽的计费方式不变，EIP 也不跟着转。带宽如果本来按流量或按量带宽计，余额仍要盖住它。腾讯云只有上面那两类按小时带宽会改成按带宽的包年包月，其余网络计费方式不变。

**节省计划和这次转换是一回事吗？**
计费 FAQ 把包年包月、预留实例、节省计划分成三种折扣。节省计划抵的是账单，不会把这台机器改成包年包月。预留实例也不是一台可以登录的机器。

**包年包月买错了，有没有五天无理由？**
退订规则页的总述提到五天无理由。产品表里 ECS 的「未使用全额退订」写成不支持，部分退订和未生效的续费订单写成支持。我没在 ECS 这一行看到单独的五天全额退。退订会释放资源，先备份。金额以退订页为准。腾讯云购买页写的是包年包月不可退。

**专有宿主机上的 ECS 能转吗？**
参数说明写，订阅时长不能超过宿主机剩下的时长。错误码 `InvalidOperation.NotSupport` 又写，宿主机上的实例不支持改计费方式。控制台没有入口，就不要改用接口硬转。

## 账号从哪里来

改计费方式的前提，是这台机器已经在你自己的国际版主账号下面。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。

## 官方参考

- [Change the billing method from pay-as-you-go to subscription](https://www.alibabacloud.com/help/en/ecs/change-the-billing-method-of-an-ecs-instance-from-pay-as-you-go-to-subscription-1)
- [Switch from subscription to pay-as-you-go](https://www.alibabacloud.com/help/en/ecs/change-the-billing-method-of-an-instance-from-subscription-to-pay-as-you-go-1)
- [Refund rules for switching from subscription to pay-as-you-go](https://www.alibabacloud.com/help/en/user-center/description-of-refund-rules-for-transfer-to-pay-as-you-go)
- [Billing FAQ](https://www.alibabacloud.com/help/en/ecs/billing-method-faqs)
- [ModifyInstanceChargeType](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-modifyinstancechargetype)
- [ECS limits](https://www.alibabacloud.com/help/en/ecs/user-guide/limitations)
- [Overview of billing methods](https://www.alibabacloud.com/help/en/ecs/overview-of-billing-methods)
- [Unsubscription rules](https://www.alibabacloud.com/help/en/user-center/cancel-subscription/)
- [Billing Modes](https://www.tencentcloud.com/document/product/213/2180)
- [Converting Pay-as-You-Go to Yearly/Monthly Subscription](https://www.tencentcloud.com/document/product/213/59353)
- [ModifyInstancesChargeType](https://www.tencentcloud.com/document/product/213/73503)
- [Purchase Methods](https://www.tencentcloud.com/document/product/213/506)
- [CVM release notes](https://www.tencentcloud.com/document/product/213/60176)

## 一句话总结

阿里云按量和包年包月可以互转。按量转包年包月时，系统盘跟着走，数据盘默认可以不转，公网带宽和 EIP 仍按原来的方式扣余额。转回按量要占每月退款额度，已用不满 30 天，已消耗按 1.5 倍计。腾讯云国际站写明的路径是按量转包年包月，付清才生效，新周期从转换成功时起算。包年包月转回按量，计费模式页和接口参数说明都没有放开。

返回 [指南首页](index.md)
