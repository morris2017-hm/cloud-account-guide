---
title: 公网 IP 和弹性公网 IP 怎么选：按流量还是按带宽，闲置还扣不扣费
description: 阿里云 ECS、腾讯云 CVM 的普通公网 IP 和弹性公网 IP 怎么选。按流量的峰值只是上限，按带宽过了 5 Mbps 单价会跳档；EIP 未绑定或绑错资源时，地址本身还会按小时或按天计费。
permalink: /cloud-public-ip-eip-bandwidth-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)

# 公网 IP 和弹性公网 IP 怎么选

阿里云账号或腾讯云账号到手、创建第一台 ECS 或 CVM 时，购买页会问要不要分配公网 IPv4，以及公网按流量还是按带宽计费。选错通常不是开不了机，而是两件事后才看见的事：按流量时写上去的带宽并不是保底速率；单独申请的弹性公网 IP（EIP）没绑上实例，地址本身仍在扣费。

节省停机、关机不收费会释放系统分配的公网 IP，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。阿里云按流量的 CDT 免费额度见 [CDT 免费流量](aliyun-international-cdt-free-traffic.md)。轻量应用服务器和 Lighthouse 的公网 IP 跟套餐走，不能套用下面这套 EIP，见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。本文只讨论 IPv4。单价和按钮会变，以购买页和控制台为准。

## 先分清两种地址

| 项目 | 阿里云静态公网 IP / 腾讯云普通公网 IP | 弹性公网 IP（EIP） |
| :--- | :--- | :--- |
| 怎么来 | 创建实例时分配。阿里云也可以在创建后把公网带宽调到大于 0 | 单独申请，或把已有公网 IP 转换成 EIP |
| 生命周期 | 跟这台实例走。实例释放时地址被收回 | 独立资源。实例释放或解绑之后，地址还在账号里，直到你释放它 |
| 能不能换绑 | 不能解下来绑到另一台 | 可以解绑，再绑到同地域的另一台实例、NAT 网关等 |
| 地址占用费 | 阿里云写明静态公网 IP 没有持有费，只为出方向公网带宽付费。腾讯云对比表写普通公网 IP 没有 IP 闲置费 | 满足下一节的条件时，地址本身按小时或按天计费 |

依据：[阿里云静态公网 IP](https://www.alibabacloud.com/help/en/ecs/user-guide/public-ip-address)（2026-05-15）、[腾讯云普通公网 IP 转 EIP](https://www.tencentcloud.com/document/product/1242/70630)、[腾讯云 IPv4 地址](https://www.tencentcloud.com/document/product/215/32382)。

一台机器、一个网站、域名可以随时改解析：购买时分配普通公网 IP 就够。域名备案、支付回调、客户防火墙白名单要长期用同一个地址，或者准备用节省停机、关机不收费：在关机前把地址转成 EIP。转换后 IP 字符串不变，但不能再转回普通公网 IP。

公网 IP 只解决「从互联网能不能找到这台机器」。端口通不通仍看安全组，见 [安全组怎么配](cloud-security-group-guide.md)。

## 按流量还是按带宽

两边的建议接近。腾讯云公网计费写：带宽利用率高于约 10% 时优先按带宽，低于约 10% 时优先按流量。阿里云自定义购买页同样用 10% 作分界：长时间占着带宽、利用率高于 10%，选按带宽；利用率不高于 10%、低谷很小而高峰波动大，选按流量。这是文档给出的经验分界。流量单价按地域不同，要比较时用购买页的单价乘自己的出流量。

计费方向也要分开：

- 出方向（服务器发往公网）计费。入方向（从公网进入服务器）不计流量费。
- 阿里云带宽说明（2026-09-09）对入方向有上限：所购出方向带宽小于 10 Mbit/s 时，入方向最高 10 Mbit/s；出方向大于 10 Mbit/s 时，入方向等于出方向。
- 控制台上的 Mbps 是 Mbit/s。同一页的换算：5 Mbps = 0.625 MB/s = 640 KB/s。
- 按流量时，购买页上的峰值是上限，资源紧张时实际速率可以更低。要保底速率，选按带宽。阿里云并建议按真实需要设置这个上限，避免程序错误或异常请求把出流量打满。
- 阿里云变更带宽的说明写明：中国香港的 BGP 精品（Multi-ISP Pro）不支持按流量。精品线路和普通 BGP 不是同一张价目，地域差异见 [地域怎么选](cloud-region-selection-guide.md)。

阿里云按流量的出流量有可能改由云数据传输（CDT）出账，免费额度和阶梯价见 CDT 那篇，这里不重复。两处文档的生效条件不一样，要对着账单看：[EIP 按量计费](https://www.alibabacloud.com/help/en/eip/pay-as-you-go/)（2026-10-26）写，2024-12-17 00:00（UTC+8）之后第一次创建按流量 EIP，流量费由 CDT 出账；在此之前创建的仍按 EIP 页的单价，除非你主动升级。ECS 自定义购买页写，CDT 不会自动套到尚未升级的 ECS，要在 CDT 控制台对该服务执行升级；未升级的按流量实例仍由原产品出账，按带宽不受这次升级影响。账单产品名是 Cloud Data Transfer，还是 ECS、Elastic IP Address，决定你正在用哪套单价。

按带宽要单独看 5 Mbps 这道阶梯。阿里云按带宽 EIP 的 BGP（多线）在香港、新加坡、弗吉尼亚、法兰克福等地，文档标价是 1–5 Mbps 每 Mbps 每天 0.14 美元，超过 5 Mbps 的部分每 Mbps 每天 0.50 美元。东京是 0.17 和 0.57。整天持有时：

- 5 Mbps：0.14 × 5 = 0.70 美元
- 10 Mbps：0.14 × 5 + 0.50 × (10 − 5) = 3.20 美元

从 5 Mbps 加到 10 Mbps，费用不是翻倍。当天如果改过峰值，按当天设过的最高值计；持有不足 1 小时按 1 小时，再除以 24 折成「天」。按流量则只看出方向 GB 数，改峰值不改变流量费。

腾讯云按小时带宽在香港、新加坡、东京、弗吉尼亚、法兰克福等地域，文档标价是不超过 5 Mbps 的部分每 Mbps 每小时 0.0058 美元，超出部分每 Mbps 每小时 0.0208 美元。同一小时内改过带宽，按这小时的最高配置计：

- 5 Mbps 一小时：0.0058 × 5 = 0.029 美元
- 10 Mbps 一小时：0.0058 × 5 + 0.0208 × 5 = 0.133 美元

上面的单价来自文档里的公有云价目，页面写明成交价以购买页为准。腾讯云按流量统计的是出方向字节，单位按 1024 进位，并写明因为 TCP 头部和重传，云侧统计可能比服务器上应用层看到的大约高 10%。文档列出的流量单价包括：香港 0.12 美元/GB，新加坡 0.081，弗吉尼亚 0.075，东京 0.13。同一页注明，2023-11-15 起中国内地以外地域的按小时流量有过最高约 35% 的价格调整说明，仍以官网成交价为准。

阿里云按量 EIP 的计费方式切换在次日 00:00（UTC+8）生效，生效前不能再改峰值；要先改峰值，就先取消尚未生效的切换订单。腾讯云按量 CVM 可以从按流量改成按小时带宽，同一小时内的流量部分按已用流量计、带宽部分按秒数占 3600 秒的比例计。包年包月的按月带宽，腾讯云写明在当前周期只能升、不能降，降配要等到续费周期。

## 地址本身什么时候单独扣费

普通公网 IP 跟实例在一起时，账单里主要是带宽或流量。EIP 多一项地址占用费。免不免，看它有没有绑定、绑到了什么、以及账号的 EIP 配额上限。配额上限是账号被允许持有的数量，不是当前已经申请了几个。

| 状态 | 阿里云 BGP EIP 的配置费（公网 IP 保留费） | 腾讯云 EIP 的 IP 资源费 |
| :--- | :--- | :--- |
| 刚申请、还没绑定 | 按量 EIP 从申请成功就开始计 | 按流量、以及传统账号的各种计费方式：未绑定就计。标准账号的按小时带宽、按月带宽：未绑定也不计 IP 资源费，但公网带宽费照计 |
| 直接绑在 VPC 里的 ECS / ECI 上，且账号 EIP 配额不超过 2000 | 免配置费 | 已绑定任意云资源且配额不超过 2000：免 IP 资源费，改收公网费用。腾讯云的免除不要求资源必须是 CVM |
| 绑在弹性网卡上（含主网卡）、CLB、NAT 网关上 | 仍收配置费。官方举例把 CLB 算进收费、把直接绑 ECS 算进免费 | 已绑定云资源后，按上一行的「已绑定」处理。传统账号的公网费用记在 CVM 等资源上，标准账号记在 IP 上 |
| 配额上限高于 2000 | 直接绑在 ECS 上也不再免除，全部计配置费 | 按流量和带宽包：绑定后仍计 IP 资源费，外加公网费用 |
| 美国（弗吉尼亚）Local ISP 线路 | 不论是否绑定都收配置费 | 文档写各线路的 IP 资源费单价相同 |

BGP 线路还有两种情况不收阿里云这项配置费：EIP 是从 IP 地址池划出来的，或者地址是你自带的（BYOIP）。地址池里的地址改按地址池自己的公网 IP 费用计。Local ISP 不在这个免除范围里。

阿里云按流量时，配置费按小时向上取整，单价是美元/小时/IP。文档标价里，香港 BGP（多线）和精品是 0.009，新加坡、雅加达、曼谷是 0.006，东京、弗吉尼亚、硅谷是 0.005。这项不免的时候，香港 0.009 × 24 = 0.216 美元/天；按 30 × 24 小时乘是 6.48 美元。这是小时单价的乘法，不是官方包月价。按带宽的配置费改用美元/天/IP，香港文档标价 0.211，新加坡 0.151，东京和弗吉尼亚 0.113，按天结算。订阅 EIP 的计费总览只列带宽费，不列这项保留费。

腾讯云 IP 资源费按秒计、按小时结算。文档标价：香港、新加坡、东京、首尔、硅谷、弗吉尼亚、法兰克福、曼谷、大阪每小时 0.04 美元；雅加达、柔佛和中国内地地域 0.031。官方举例：配额不超过 2000 时，广州地域闲置 2700 秒，费用是 0.031 × (2700 / 3600) = 0.02325 美元，在下一个小时结算。按 0.04 美元/小时连续闲置 24 小时是 0.96 美元，连续 30 × 24 小时是 28.80 美元，同样只是乘法。绑上云资源后这项暂停，解绑后继续计，释放后停止；同一小时内多次解绑，把未绑定的秒数加在一起。

所以两边「放着不用」的账单不一样。阿里云：直接绑在 ECS 上且配额不超过 2000，配置费可以为 0，按带宽的带宽费或按流量的出流量仍在。腾讯云标准账号、按流量：绑着就没有 IP 资源费；解绑后流量费停了，IP 资源费开始。腾讯云按小时带宽：解绑停不掉公网带宽费，要释放 EIP 才停。释放后的那一个计费周期仍可能出账，因为结算看的是上一小时或上一天。

腾讯云要先确认账号类型，否则会看错费用记在 IP 上还是记在 CVM 上。[账号类型说明](https://www.tencentcloud.com/document/product/1242/70613) 写：2020-06-17 00:00 之后注册的账号是标准账号（带宽和流量在 IP 或负载均衡上）；又写明部分渠道——包括国际站渠道商账号——曾经可能没有登记成标准账号，并写这个问题已经修复。代开账号不要用注册日期推断。打开公网 IP 控制台，列表顶部没有传统账号提示，就是标准账号；有提示，就是传统账号（带宽和流量在 CVM 上）。

关机文档和 EIP 文档说的「闲置」不是同一件事。[关机不收费](https://www.tencentcloud.com/document/product/213/19918)（2025-12-31）写：普通公网 IP 会被释放；事先转成 EIP 的地址仍绑在实例上，并写关机后闲置的 EIP 不计费。EIP 计费页的 IP 资源费，发生在未绑定的时候。关机后那枚 EIP 如果还挂在这台 CVM 上，就还是绑定状态。按小时带宽的公网带宽费，计费总览写明不论绑定与否都计，关机页没有单独豁免这一行。阿里云节省停机会释放静态公网 IP，EIP 留下来；按带宽计费的 EIP 带宽费继续。以账单里的「IP 资源费 / 配置费」和「公网带宽或流量」两行为准。

实例删掉之后，EIP 不会跟着消失。腾讯云写明 CVM 或 NAT 网关释放时会与 EIP 解绑，解绑后的按流量 EIP 进入 IP 资源费。释放实例的窗口里如果没有 EIP 这一项，释放完要到 EIP 列表里再释放一次。

## 地址要固定，或要换掉

阿里云静态公网 IP 不能指定具体地址，系统随机分配。创建后 6 小时内可以更换：先用标准停机停掉实例，再选更换公网 IP。选节省停机会把地址收回，更换就做不了。6 小时内更换超过 3 次，或者已经超过 6 小时，要走另一条：先转成 EIP，解绑，再把公网带宽设回大于 0 以取得一个新的静态公网 IP，最后释放那枚 EIP，避免配置费继续走。包年包月实例到期前 24 小时不能做这套操作；按带宽的包年包月要先改成按流量，才能把静态公网 IP 转成 EIP。转换不可逆，IP 字符串保持不变，之后按 EIP 单独出账。

腾讯云把普通公网 IP 转成 EIP 后，地址和原来的公网计费方式都保留，也不能转回去。标准账号不能直接把「按月带宽」的普通公网 IP 转成 EIP，要先改成按流量或按小时带宽；文档写明代金券和购买折扣不退，以后再改回按月带宽要按官网标价重新买。另一条路径是新申请一枚 EIP 再绑到 CVM 上：绑上时，这台 CVM 原来的普通公网 IP 会被释放。转换是留住原地址，新绑是换成新地址。

IPv4 地址说明里写每个账号每个地域可申请 20 个 EIP。配额以控制台为准。换地域一定换地址，迁移步骤见 [地域怎么选](cloud-region-selection-guide.md)。

换公网 IP 解决不了从中国内地访问海外地域的延迟。阿里云静态公网 IP 的 FAQ 写明，这段路径走的是运营商国际链路，换地址不会改道。要改善这段访问，用中国内地地域，或看全球加速一类产品，而不是反复更换 IP。

## 欠费后，EIP 的时钟和实例不是同一套

实例欠费后多久停机，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。EIP 有自己的表。

阿里云按量 EIP（2026-10-26 的计费页）：欠费后服务继续 15 天。仍未付款就进入停服，未加入共享带宽的 EIP、以及加入按量共享带宽的 EIP，带宽降到 1 Kbps；加入尚未到期的订阅共享带宽时，共享带宽本身不受影响。停服后再过 15 天仍未恢复，EIP 自动释放，配置删除，不能找回。停服后 15 天内补齐欠款，会结清并马上恢复。

腾讯云按流量、按小时带宽的 EIP，[欠费说明](https://www.tencentcloud.com/document/product/1242/71840)（2026-08-04）把账号欠费和「绑定的资源自己欠费」分成两节：

- 账号欠费：未绑定的 EIP，不满 2 小时仍可用并继续计费；满 2 小时后停服并停止计费；时长达到 2 小时再加 24 小时后释放。已绑定 CVM 的那一行，在到达这个终点时写的是解绑并释放。中间两格页面上没有单独的句子，不要自行补成和未绑定完全相同，以控制台状态为准。
- 绑定的云资源自己欠费：EIP 会被自动解绑，并进入闲置计费。CVM 进了欠费流程之后，这枚 EIP 可能已经在按 IP 资源费扣钱。不用就立刻释放。
- 账号里还有未过期、没用完的流量包时，标准账号下按流量的资源可以继续用；传统账号仍会按原流程停服、解绑、释放，流量包不改变这条生命周期。
- 按月带宽看的是到期日，账号欠费本身不影响这份预付。普通 BGP：到期前 7 天起发提醒；到期前没续费的，到期后再过 7 天进入 7 天隔离，隔离期内地址不可用，隔离期结束仍不续费就释放。专用 BGP 是到期后 1 天进入 7 天隔离。文档没有把「到期后、隔离开始前」写成仍可访问，以控制台为准。

腾讯云这套「2 小时 + 24 小时」短于 CVM 回收站里按量实例常见的保留天数。只给 CVM 续命、不看 EIP 列表，地址可能先没了。

## 下单和释放时核对什么

1. 第一台网站机器：购买时分配普通公网 IP，按流量，峰值按业务设上限。利用率长期高于约 10%，再改按带宽，并先算过 5 Mbps 以上那一档。
2. 域名或白名单要固定地址：在节省停机或关机不收费之前，把普通公网 IP 转成 EIP，确认转换后的地址和原来一致。
3. 单独申请 EIP 之后马上绑定。阿里云绑到网卡、CLB、NAT 上时，确认配置费是否仍在。腾讯云不用的 EIP 先解绑再释放；只解绑、不释放，按流量的地址会开始计 IP 资源费。
4. 释放实例后打开 EIP 列表。留下的未绑定地址继续出账。
5. 账单按产品拆开：阿里云看 Elastic IP Address 和 Cloud Data Transfer；腾讯云看 IP 资源费和公网费用是不是各有一行。
6. 腾讯云先在公网 IP 控制台确认是标准账号还是传统账号。渠道结算的账号保持交付时的付款方式，见 [阿里云代开](aliyun-agency-opening.md) 和 [腾讯云代开](tencent-cloud-agency-opening.md)。

## 常见问题

**关机之后公网 IP 变了，是不是故障？**
按量实例如果用了节省停机或关机不收费，系统分配的公网 IP 会被释放，下次启动可能是新地址。要留住地址，关机前先转成 EIP。在操作系统里面关机，两边都不会进入这种停计费模式。

**没有实例的 EIP 为什么还在扣钱？**
地址占用费从「持有」开始，不从「有流量」开始。阿里云按量 EIP 申请后就可能计配置费；腾讯云按流量的 EIP 在未绑定时计 IP 资源费。释放之后，上一小时或上一天的账单仍可能晚到。

**按流量填了 100 Mbps，是不是就有 100 Mbps 的保底？**
没有。这个数字是出方向上限。保底速率要选按带宽，并接受按所设带宽计费，而不是按实际跑满的比例计费。

**把 EIP 绑到弹性网卡上，阿里云还收配置费吗？**
收。免配置费的条件是直接绑定 VPC 中的 ECS 或 ECI，并且账号 EIP 配额不超过 2000。文档举例：即使选的是已经绑在主网卡上的弹性网卡，配置费仍在。

## 账号从哪里来

公网 IP 和 EIP 都是账号里的资源，换一种地址不会新开一个云账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [Assign and manage static public IP addresses](https://www.alibabacloud.com/help/en/ecs/user-guide/public-ip-address)
- [Network bandwidth](https://www.alibabacloud.com/help/en/ecs/user-guide/network-bandwidth/)
- [Public bandwidth](https://www.alibabacloud.com/help/en/ecs/public-bandwidth)
- [Convert a static public IP address to an EIP](https://www.alibabacloud.com/help/en/ecs/user-guide/convert-the-public-ip-address-of-an-instance-in-a-vpc-to-an-eip)
- [Pay-as-you-go EIP](https://www.alibabacloud.com/help/en/eip/pay-as-you-go/)
- [EIP billing overview](https://www.alibabacloud.com/help/en/eip/billing-overview)
- [EIP billing FAQ](https://www.alibabacloud.com/help/en/eip/billing-faq)
- [Public Network Billing（CVM）](https://www.tencentcloud.com/document/product/213/10578)
- [Public Network Fee](https://www.tencentcloud.com/document/product/213/39743)
- [EIP Billing Overview](https://www.tencentcloud.com/document/product/1242/70615)
- [IP Resource Fees](https://www.tencentcloud.com/document/product/1242/70617)
- [Public Network Fees（EIP）](https://www.tencentcloud.com/document/product/1242/70618)
- [Account Type Description](https://www.tencentcloud.com/document/product/1242/70613)
- [Switching a Common Public IP Address to an EIP](https://www.tencentcloud.com/document/product/1242/70630)
- [Payment Overdue（EIP）](https://www.tencentcloud.com/document/product/1242/71840)
- [Shutting Down Pay-as-You-Go Instances to Avoid Charges](https://www.tencentcloud.com/document/product/213/19918)

## 一句话总结

一台机器先用普通公网 IP，按流量并设好峰值上限；要跨关机、跨实例留住地址，再转成 EIP。按带宽过了 5 Mbps，超出部分按更高单价计。EIP 申请后就可能产生地址占用费，绑错资源或解绑后不释放，余额会在没有流量的情况下继续下降。

返回 [指南首页](index.md)
