---
title: 自动续费哪天扣钱：开了不会马上续上
description: 阿里云国际站 ECS、轻量和腾讯云国际站 CVM 的包年包月自动续费在哪几天扣余额、失败后怎么重试、手动续费会不会再扣一笔，以及「不续费」和关掉自动续费的差别。
permalink: /cloud-subscription-auto-renewal-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md)

# 自动续费哪天扣钱

包年包月的机器有自己的到期日。余额里有钱，只说明扣款日当天有机会成功。我把阿里云国际站费用中心、ECS 自动续费页，和腾讯云国际站续费管理、CVM 续费接口对了一遍。阿里云国际站从到期前 3 天才开始扣，失败了才换下一个日子，成功就停。腾讯云写的是到期日当天扣，当天不够就在停服前按天再看余额。两边都写明：打开自动续费，只是把周期设上，不会当场续一笔。

按量付费按使用扣余额，续费名单里没有它。三种计费方式怎么选，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。已经到期或欠费之后多久停、多久释放，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。阿里云轻量应用服务器在续费说明里单列了一行。腾讯云包年包月套餐以续费管理里是否有这一条为准。时长和金额以续费页为准。

## 两边的扣款日

| 项目 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 谁要续 | 包年包月资源。按量实例、已释放的实例不在名单里 | 包年包月资源。按量实例按小时结算，页面写不用续费 |
| 扣款日 | 到期前 3 天、前 1 天、到期当天、到期后第 6 天、到期后第 14 天。上一次成功，后面的不再扣 | 到期日当天扣。当天余额不够，停服前按天检查，余额够了再续。具体时刻可能晚于到期时间 |
| 打开之后 | 只保存周期，次日生效。资源明天就到期的，这一轮自动扣款不跑，要手动续 | 快到期或即将回收的资源，续费管理要求先别改自动续费设置 |
| 每次续多久 | 默认跟当初购买的时长一样，之后可以改。设成 3 个月，就按 3 个月续 | CVM 的续费标记接口写明，自动续费是每次到期后续 1 个月 |
| 代金券 | 自动续费可以使用优惠券。费用中心写余额、credits 或 vouchers 要够；ECS 页写的是余额和代金券。credits 页面没有再解释，以订单实际扣的资金为准 | 符合条件的代金券会自动抵。一次批量续费选中超过 100 个资源时，代金券用不了 |
| 设成不续费 | 到期提醒收成一次。ECS 接口写只在到期前第 3 天发 | 到期前不再发通知 |

依据：[续费管理](https://www.alibabacloud.com/help/en/user-center/renewal-guide-1)（2026-10-08）、[国际站自动续费](https://www.alibabacloud.com/help/en/user-center/configure-auto-renewal)（2026-06-02）、[ECS 自动续费](https://www.alibabacloud.com/help/en/ecs/enable-auto-renewal-for-an-instance-1)（2026-09-21）、[腾讯云续费管理](https://www.tencentcloud.com/document/product/555/7454)（2026-03-13）、[ModifyInstancesRenewFlag](https://www.tencentcloud.com/document/product/213/68324)（2026-05-27）。

同一份阿里云自动续费说明里，前面有一句「多数产品提前 9 天开始扣」。紧跟着的国际站说明把扣款日改成上面这五个点。ECS 自动续费页和续费管理、续费 FAQ 用的也是这五个点。ECS 那一页把第四个点写成 “7 days after expiration (T+6)”。T+6 就是到期后第 6 天，和费用中心一致；多出来的 “7 days” 和它自己的 T+6 对不上。我按费用中心的五个点记。续费页上如果写出了下一次扣款时间，以那一行为准。

## 阿里云：失败才换下一天

第一次尝试在到期前 3 天。余额不够，才在前 1 天、到期当天、到期后第 6 天、到期后第 14 天再试。扣成功就停，不会五个日子各扣一笔。到期后第 14 天仍没扣到，自动续费不再尝试，剩下的时间要自己到续费页手动续。扣款失败会发到注册邮箱。

开关可以从创建时一直开到到期后 14 天。设置本身次日才生效。所以「明天到期，今晚把自动续费打开」这一轮赶不上，得手动续。新买的实例同步到续费系统有延迟，续费 FAQ（2026-09-23）写可以刷新，或等到第二天再看。

续费时长在购买时默认等于购买时长。买一个月，自动续费也是一个月。买三个月，就按三个月续。创建之后可以在实例的 Configure Auto-renewal 里改时长，也可以关掉。费用中心的路径是 Expenses、Renewal Management。新版控制台写的是 Order、Resource Renewal，手动续费、自动续费、到期不续费分成几个标签。RAM 子用户要续费，主账号得先给 `AliyunBSSFullAccess` 或 `AliyunBSSRenewFullAccess`。子用户怎么分权，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

手动续费在资源释放前都可以做。到期前续，新周期从上一笔订单的结束时间往后接。已经关机之后再续，新周期从续费成功时起算，关机那段不收费。一台实例同时只能有一笔待生效的续费订单，这笔还没生效，就不能再下一笔。

已经开了自动续费，又在扣款日之前手动续了一笔：付完款后，系统不再做这一轮自动扣款，避免两笔叠在一起。自动续费开关还在，下一次按新的到期日重算。续费 FAQ 的例子把到期日从 3 月 30 日改到 4 月 30 日，扣款时间却写成 3 月 27 日 08:00，这个钟点对的是旧到期日。我不以这个例子里的钟点为准，只采用「按新到期日重算」这一句。

续一个月，按自然月接到下个月的对应日 00:00，不是固定加 30 天。1 月 31 日到期，续一个月到 2 月 29 日或 2 月 28 日，以后每次都落在这一天。5 月 31 日到期，续一个月到 6 月 30 日，以后落在 30 日，直到某一次落到 1 月 30 日，再续才会进 2 月的月底。按年自动续费一年只出一笔续费订单，其余月份的账单是空的，那是按年出账，不是漏扣。

到期提醒由站内信和邮件发出，时点是到期前 7 天、3 天、1 天、当天，以及释放前 1 天。低余额通知在消息中心的 Account Messages 里，站内信和邮件默认开着。导出续费清单只是报价，不会把机器续上。

## 开没开自动续费，到期后差在缓冲

续费管理（2026-10-08）按续费状态拆开写：

| 资源 | 到期后还能用多久 | 之后 |
| :--- | :--- | :--- |
| 开了自动续费的 ECS | 有 15 天缓冲 | 到期满 15 天进入关机，数据还在。关机后再过 15 天释放，数据找不回 |
| 手动续费的 ECS | 没有这 15 天缓冲 | 到期就关机，数据仍留着。关机后再过 15 天释放 |
| 轻量应用服务器 | 前 15 天仍可访问，和开没开自动续费无关 | 到期满 15 天关机，数据保留，这期间续费可以再启动。关机后再过 15 天释放 |

ECS 订阅页（2026-08-04）的正文另有一套说法：没开自动续费，到期就停服；开了自动续费但扣款失败，停服时间落在到期后第 15 天 00:00 到第 16 天 00:00 之间。同一页的资源表又写，到期后 15 天内计算资源仍可用。这两段对不上的地方，[计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) 里已经标过。下线日期以控制台里这台实例的状态为准，到期前先做快照。

资源释放之后，续费 FAQ 写流量费也不再收。数据在释放时就没了。

## 不续费，和关掉自动续费

「到期不续费」主要是少收提醒。续费管理写，一般会在关机前通知三次，设成不续费就收成一次。ECS 的 ModifyInstanceAutoRenewAttribute 写得更具体：不续费状态下，只在到期前第 3 天发一次提醒。设回去之后，可以再手动续，或重新打开自动续费。

腾讯云的三个标记在 ModifyInstancesRenewFlag 里。`NOTIFY_AND_AUTO_RENEW` 是到期通知并自动续费，余额够就按月续一个月。`NOTIFY_AND_MANUAL_RENEW` 是到期通知，不自动扣。`DISABLE_NOTIFY_AND_MANUAL_RENEW` 是不通知，也不自动扣。续费管理里的 Non-renewal 标签对应最后这种，到期前不发通知。

取消自动续费之后，腾讯云会恢复到期前提醒。从「不续费」改回手动或自动，提醒也会回来。包销实例的续费标记只能是自动续费，接口不让改成另外两个。包销本身是什么，见计费方式那篇。

## 续错了一笔

续费订单还没生效时，退订页有 Unsubscribe Renewal That Does Not Take Effect 这一栏，可以申请把现金部分全额退回，优惠券不在退款里。已经生效的订单在 Unsubscribe Resource 里，按非全额规则退剩余费用，金额以退订页为准。退款显示 0 美元时，续费 FAQ 给了两种情况：这笔续费是购买时附赠的，或者优惠券已经把现金部分抵成 0。

活动价不一定能带到续费。改过配置、在续费周期里升过配、新客价或限时价已经过期，续费都可能回到标价。付款页上的金额为准。

## 腾讯云：到期日当天，然后按天看余额

续费管理写，自动续费的资源在到期日当天续，具体时刻可能晚于到期时间。当天余额不够，停服之前每天查一次，余额够了就把设成自动续费的资源续上。已经停服的资源不会自动续，也不会出现在续费管理里。恢复停服资源的步骤见欠费那篇。CVM 欠费说明（同一套国际站文档）另写：到期前 7 天发续费提醒；开了自动续费且余额足够时，在到期日执行。费用中心把提醒窗口做成可调，从到期或自动续费日前 7 天到 3 个月，渠道是消息中心、短信、邮件。一个渠道都不选，就不发。这个设置只对当前登录账号生效。

CVM 创建和续费用的是 InstanceChargePrepaid。数据类型页（2026-09-21）把这项的默认续费标记写成 `NOTIFY_AND_MANUAL_RENEW`，也就是通知、但不自动续。同页的 ChargePrepaid 是给专有宿主机 AllocateHosts 用的，默认才是自动续费。买 CVM 时以购买页的勾选和实例上的续费标记为准，不要把宿主机的默认套过来。

自动续费的时长，标记接口写的是一个月。手动续费接口 RenewInstances（2026-06-11）用月做单位，错误码里列出的时长是 1 到 12 个月，以及 24、36 个月。同一份数据类型又把 48、60 个月写进 InstanceChargePrepaid。下单时以续费页能选的时长为准。这个手动续费接口默认把挂载的弹性数据盘一起续，`RenewPortableDataDisk` 设成 false 才只续实例。自动续费标记那份接口没有单独写磁盘。订单里磁盘是单独一行时，以那一行为准。余额不足、账号还有未支付订单，手动续费会被拒绝。

统一到期日在续费管理里设置，只能选每月 1 日到 28 日，续费时长要大于一个月。费用按续费天数乘以单日价格算，折扣还可能叠加上去。文档里的演算是：8 月 6 日到期的资源，一个月 13 美元，折成每天 0.43 美元，调到每月 1 日并续到 10 月 1 日，应付 24.18 美元，算式是 13 + (56 − 30) × 0.43。这是 2026-03-13 那一页上的例子，不是现行报价。阿里云续费页也有把 ECS 到期日对齐到每月同一天的入口，新周期从续费当天起算，天数和金额以那一页为准。

## 渠道账号上怎么对

扣款用的是账号里当时能用的余额和代金券。费用中心把 credits 和 vouchers 写在一起，订单页会显示这笔到底扣了哪一种。渠道结算的账号，付款方式保持交付时的样子。阿里云在到期前 3 天就会第一次尝试，腾讯云卡在到期日当天，余额要在这两个时点之前到账。已有账号按账号 ID 充值，见 [国际云充值](https://www.yundaxian.com/recharge)。充进去之后，到续费页看到期日有没有往后移。只看到余额变了、到期日没变，这一轮还没续上。

## 常见问题

**导出续费清单，是不是已经续上了？**
没有。续费 FAQ 写，导出只是把资源、时长和费用拉出来报价。要续，得在页面上点续费并付完款。

**关掉免密支付，已经买的机器会不会停？**
续费 FAQ 的原问是：打开自动续费之后，取消订阅或关掉免密支付，会不会影响已经买的配额和资源。回答是不影响已经买下的使用权利，只让后面的自动续费不再执行。渠道账号的付款方式保持交付时的样子，余额不够就按账号 ID 充。

**按年自动续费，为什么有的月份账单是空的？**
一年只产生一笔续费订单。出账的那个月能看到年费，其余月份为空。周期到 Resource Renewal 里对自动续费配置和下一次续费时间。

**续费页找不到这台机器？**
按量实例不在这份名单里。已经释放的也不能续。资源包在 Resource Plans 页单独续。域名、企业邮箱等要走页面右上角的产品入口。还是没有，就核对当前登录的是不是购买时的那个账号。

## 账号从哪里来

续费发生在已经有的那个主账号上，换续费方式不会新开一个号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。

## 官方参考

- [Renew expiring resources](https://www.alibabacloud.com/help/en/user-center/renewal-guide-1)
- [Configure auto-renewal](https://www.alibabacloud.com/help/en/user-center/configure-auto-renewal)
- [Resource Renewal FAQ](https://www.alibabacloud.com/help/en/user-center/support/renewal-guide-q-a)
- [Enable auto-renewal for an ECS instance](https://www.alibabacloud.com/help/en/ecs/enable-auto-renewal-for-an-instance-1)
- [Subscription](https://www.alibabacloud.com/help/en/ecs/subscription)
- [Renewal Management](https://www.tencentcloud.com/document/product/555/7454)
- [ModifyInstancesRenewFlag](https://www.tencentcloud.com/document/product/213/68324)
- [RenewInstances](https://www.tencentcloud.com/document/product/213/72769)
- [Data Types](https://www.tencentcloud.com/document/product/213/15753)
- [Overdue Payments](https://www.tencentcloud.com/document/product/213/2181)

## 一句话总结

阿里云国际站在到期前 3 天、前 1 天、当天、到期后第 6 天和第 14 天依次尝试，成功就停。设置次日生效，明天到期的要手动续。腾讯云在到期日当天扣，当天不够就在停服前按天再看余额；CVM 的自动续费接口按月续一个月。打开开关不会当场扣款，手动续过的那一轮也不会再自动扣一笔。

返回 [指南首页](index.md)
