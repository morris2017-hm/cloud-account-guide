---
title: 代开的机器手滑能删掉吗：释放保护默认关着，欠费和定时释放照样走
description: 阿里云国际站按量实例的释放保护默认关闭，只拦手动释放。欠费、定时释放、安全停机和伸缩组缩容照样放机器。腾讯云终止保护的参数说明只写 API，功能页写控制台也拦。
permalink: /cloud-release-protection-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md) · [验证器丢了](cloud-mfa-lost-login-guide.md) · [登录邮箱怎么换](cloud-logon-email-change-guide.md) · [释放保护](cloud-release-protection-guide.md)

# 代开的机器手滑能删掉吗

按量机器我先当成能被一下删掉。我把阿里云国际站的释放保护、释放实例、客户端，还有腾讯云国际站的终止保护、终止实例、恢复实例，按页面日期对了一遍。两边默认都是关的。阿里云这开关只写给按量实例。控制台、API、CLI 的手动释放会被拒绝。欠费、定时释放到点、安全或合规停机、伸缩组缩容，保护开着也会把机器放掉。

腾讯云功能页写，开了之后控制台和 API 都不能终止。创建参数叫 `DisableApiTermination`，说明里只写「不允许通过 API 删除」。系统级的例子只给了按量欠费。定时终止和立即释放，保护页没有点名。

下文只对 ECS 和 CVM。轻量应用服务器和 Lighthouse 的退订页，这次没放进对照。盘和公网地址跟不跟实例走，见 [系统盘与数据盘](cloud-disk-system-data-guide.md)、[公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。谁在哪天点过释放，见 [操作记录能查多久](cloud-operation-audit-guide.md)。

## 默认关着，挡住的是手点的那一下

| 这一下 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 买来默认 | 关。创建时要自己打开 | 关。购买时要自己勾上 |
| 计费方式 | 保护页的前提写成按量 | 保护页没写计费方式。终止页写包年包月不支持提前销毁 |
| 控制台手动释放 | 开着就跳过，归到不可释放 | 功能页写控制台也不能终止 |
| 单台 API | `DeleteInstance` 返回 `InvalidOperation.DeletionProtection` | `TerminateInstances` 返回 `UnsupportedOperation.InstancesProtected` |
| 欠费 | 保护页写超过 15 天会自动释放。释放页只写「因欠费」，没写天数 | 功能页的系统级例子就是按量欠费终止 |
| 定时 | 两页都写到点照样释放 | 保护页没写。终止页写到点直接释放，不进回收站 |
| 伸缩组缩容 | 挡不住。要在伸缩组里另设 Protected | 保护页没写 |
| 安全或合规 | 保护页写会强制释放 | 保护页没写，例子只有欠费 |

## 阿里云三个入口，名字对不上

保护页（2026-09-21）写，进实例详情，点 More，再搜索 Enable Release Protection 或 Disable Release Protection。状态在 Other Information 的 Release Protection Enabled。Yes 是开，No 是关。

释放页（2026-09-16）写在另一处。操作列里是 Instance Settings，再点 Enable Instance Release Protection。关掉时这页写成 Disable Release Protection，少了 Instance 这个词。真正去释放，又在 Instance Status 下面的 Release，和保护不在同一个菜单。

客户端页（2026-10-19）更新更晚。开保护的路径是 Change Instance Status，再点 Add Instance Protection。释放是同一列里的 Release Instance。这页只写按量、防手滑。它没写默认关着，也没写欠费和定时。释放那一节没写会不会先看这个开关。我在客户端点释放之前，先回网页控制台看 Release Protection Enabled。

控制台批量手动释放时，保护页写开着的实例会被跳过，放进 Non-releasable Instances。悬停是 Release protection is enabled for this instance。单台 `DeleteInstance` 的错误码是 `InvalidOperation.DeletionProtection`。释放页还提到批量接口 `DeleteInstances`，没有给这个接口单独的错误码。批量被拒时我看当次返回，不拿单台的码去套。

接口名和页面用词也不一样。创建和修改都是参数 `DeletionProtection`。`ModifyInstanceAttribute` 改已有实例，`RunInstances` 或 `CreateInstance` 在创建时带上。

## 定时释放：一页说挡不住，备注让你先关掉

保护页的使用说明把「已经设了自动释放时间」列进保护不管的情况。同一页的备注又写，先关掉保护，再去定时，或再调 `ModifyInstanceAutoReleaseTime`。两句都留着。我先打开详情里的自动释放时间。有时间，就按释放页的取消入口处理，不把「再开一次保护」当成取消。

`ModifyInstanceAutoReleaseTime`（2026-09-02）写给按量和抢占式。时间是 UTC，格式 `yyyy-MM-ddTHH:mm:ssZ`。秒不是 00，会改到这一分钟的开头。最早是 30 分钟之后，最晚是 3 年之内。不传 `AutoReleaseTime` 就是取消。这页的错误码有 `ChargeTypeViolation`、`InstanceLockedForSecurity`，没有 `DeletionProtection`。调用时保护是开是关，我看返回，不替这张表补一行。

抢占式出现在这个定时接口里。保护页的前提只写按量。抢占式被收走的规则见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。我没把抢占式写进释放保护的适用行。

释放页的取消入口在实例详情，Basic Information 的 Auto Release Time，旁边是 Cancel。

## 欠费天数，三处各写各的

保护页写，欠费超过 15 天会自动释放，释放保护不管。释放页只写因欠费释放，没有天数。ECS 欠费页把停机和释放拆成两段，见 [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)。三处我都不收成同一个数字。机器还在不在，看实例列表。

伸缩组 FAQ（2026-09-21）另写一条。问题里的用词是欠费后立刻释放。回答是健康检查把非 Running 看成不健康。按量和抢占式欠费后先被停掉，接着被移出并释放。实例 FAQ（2026-10-28）写的是另一句：伸缩组自动创建出来的实例，事后在实例页或用 `ModifyInstanceAttribute` 打开释放保护，伸缩组仍会把它放掉。要留，得到伸缩组里把实例设成 Protected。普通按量机器若在伸缩组里，我两个开关都看。

## 能关保护的人，等于能删机器

释放页的生产建议把两个权限拆开。`ecs:DeleteInstance` 和 `ecs:DeleteInstances` 给可以释放的人。`ecs:ModifyInstanceAttribute` 能把 `DeletionProtection` 改成 false，这一项交给另一个角色。同一个人两样都有，保护只是多一次确认。子用户怎么拆，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

安全或合规原因的强制释放，保护页写明不看这个开关。实例处于安全锁定时，云盘「不随实例释放」也会被忽略，那是磁盘那篇里的 `DeleteInstance` 说明。两边都发生时，我按当次错误和磁盘列表看，不假设保护还在生效。

## 腾讯云：功能页写控制台，参数说明只写 API

终止保护页（2025-09-30）写默认关闭。开启后，控制台和 API 都不能终止，随时可以关掉。系统级不生效，例子是按量欠费即将被终止。这页没有「包括但不限于」那种收口，也没有点名定时终止、立即释放、伸缩组或安全违规。

已有实例从 More 或 More actions 进 Instance settings，再点 Instance termination protection。弹窗里选 Enable 或 Close。新购走 Custom Configuration，在第二步 Complete Configuration 勾选。勾选说明链到创建页，创建页没有把这勾选限定成按量。

`RunInstances`（2026-08-26）的参数名是 `DisableApiTermination`。true 写成不允许通过 API 删除，false 是允许，默认 false。`ModifyInstancesAttribute` 用同一个参数改已有实例。说明里没有「控制台」三个字。

`TerminateInstances`（2026-05-27）的错误码比参数说明更宽。`UnsupportedOperation.InstancesProtected` 的句子是：实例处于终止保护中，不能终止，先关掉保护再试。它没有写成「仅 API」。控制台被拦时，我按弹窗。API 被拦时，我按这个错误码。参数说明那句只写 API，我留着，不把它改写成和功能页完全同一句。

## 手动退还有「两小时」和「马上没」

终止页（2025-12-31 18:31）的总述写，退还之后进回收站。表里的手动终止，未欠费的按量机器进回收站，放满 2 小时还不恢复就释放。定时终止精确到秒，到点直接释放，不进回收站，到点之前可以取消。包年包月写着 Early Destruction Not Supported，定时终止那一格是 Scheduled Termination Not Involved。到期后进回收站，最多 15 天。

恢复页（同一天 18:32）多了一个按钮。手动删除时若选 Release now，或选 Scheduled termination，机器马上终止，不进回收站，资源按该页释放，数据不能恢复。选 release after 2 hours，才进回收站。终止页那张表没有 Release now 这一行。保护页比这两页更早，两个按钮都没出现。

开保护之前，我先看这台有没有定时。`ActionTimer` 在数据类型页（2026-09-21），动作只有 `TerminateInstances`，时间是 UTC，要比当前时间晚 5 分钟。终止页写精确到秒，又写到点前随时取消。最早能设到什么时候，我看当次窗口，不把 5 分钟和「精确到秒」收成一条。

保护页没有写它拦不拦这两个按钮。功能页的总句是控制台和 API 都不能终止，系统级例子只有欠费。按钮级的结果以弹窗为准。盘、弹性公网 IP 在回收站里还扣不扣，磁盘那篇已经对过终止页和恢复页，这里不重复。

包年包月不符合退还政策时，错误码是 `InvalidInstanceNotSupportedPrepaidInstance`。企业系统镜像 Red Hat 是 `UnsupportedOperation.RedHatInstanceTerminateUnsupported`。这两个都不是 `InstancesProtected`。计费方式转不回去的限制见 [按量转包年包月](cloud-billing-method-switch-guide.md)，那里也写了设了定时销毁要先取消。

## 接手时我看的几处

按量的正式机器，我先在详情里看保护是不是 No。是 No，就按当天菜单打开。打开之后再看自动释放时间是空的。机器在伸缩组里，再去伸缩组看 Protected，只开 ECS 上的释放保护不算完。

包年包月我不当成这个开关的主场。阿里云未到期时，释放页写释放按钮是灰的，要先转成按量，或等到到期再释放。到期的包年包月走释放时，接口要带 `TerminateSubscription=true`。腾讯云终止页写不支持提前销毁。保护开着，也不会多出一个提前销毁。到期进回收站，我按终止页和 [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) 看。

真要放掉一台之前，先做快照，见 [快照与备份](cloud-snapshot-backup-guide.md)。再看系统盘、数据盘、弹性公网 IP 哪些会留下并继续扣费。保护只拦实例上的手动释放。单独删一块盘、单独释放一个地址，释放页没有写这个开关会跟着拦。

渠道结算的账号，我不为了怕欠费去自己绑卡。付款设置保持交付时的样子。余额怎么见底、见底后多久停机，仍看欠费那篇。

## 常见问题

**开了保护，欠费还会被放掉吗？**
会。阿里云保护页把欠费列进自动释放。腾讯云保护页的系统级例子就是按量欠费终止。天数以欠费页和实例列表为准，我不用保护页上的「超过 15 天」去覆盖 ECS 欠费页的两段时间。

**定时已经设上了，再开保护来得及吗？**
阿里云两页都把到点释放写成保护不管的事。已经设上的时间，我到 Auto Release Time 里取消。腾讯云保护页没写定时终止。终止页写到点直接释放、不进回收站。开保护之前先看有没有定时。

**包年包月开这个开关有用吗？**
阿里云保护页的前提是按量。未到期的包年包月，释放按钮按释放页是灰的。腾讯云终止页写包年包月不支持提前销毁。到期进回收站是终止页和欠费页的流程。我不等这开关去挡到期。

**客户端能绕过网页上的保护吗？**
客户端页没写绕过，也没写会检查。我先在网页控制台确认 Release Protection Enabled，再在客户端点 Release Instance。

## 账号从哪里来

释放保护是实例上的开关，开它不会新开一个云账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。交付后自己打开释放保护。渠道账号不要为了怕欠费去绑卡。云大仙渠道开的账号，续费走原来的渠道。别处注册的国际站账号，不能按老号在这边充。

## 官方参考

- [Manage instance release protection](https://www.alibabacloud.com/help/en/ecs/user-guide/enable-or-disable-release-protection-for-ecs-instances)
- [Release an instance](https://www.alibabacloud.com/help/en/ecs/user-guide/release-an-instance)
- [Manage ECS instances with Alibaba Cloud Client](https://www.alibabacloud.com/help/en/ecs/user-guide/use-alibaba-cloud-client-to-manage-ecs-instances)
- [ModifyInstanceAutoReleaseTime](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-modifyinstanceautoreleasetime)
- [Auto Scaling FAQ](https://www.alibabacloud.com/help/en/auto-scaling/support/faq)
- [Instance management FAQ](https://www.alibabacloud.com/help/en/ecs/instance-management-and-operations-faqs)
- [Enabling Instance Termination Protection](https://www.tencentcloud.com/document/product/213/47383)
- [Terminating/Returning an Instance](https://www.tencentcloud.com/document/product/213/4930)
- [Reclaiming/Restoring an Instance](https://www.tencentcloud.com/document/product/213/4931)
- [TerminateInstances](https://www.tencentcloud.com/document/product/213/33234)
- [RunInstances](https://www.tencentcloud.com/document/api/213/33237)
- [ModifyInstancesAttribute](https://www.tencentcloud.com/document/api/213/33246)
- [Data Types](https://www.tencentcloud.com/document/product/213/15753)

## 一句话总结

阿里云按量实例的释放保护默认关着，只拦手动释放。欠费、定时到点、安全停机、伸缩组缩容照样放机器。三个入口的菜单用词不一样。腾讯云功能页写控制台和 API 都拦，参数说明只写 API。终止页和恢复页对「手动终止进不进回收站」不是同一句。开保护之前先看有没有定时，伸缩组里再看另一个 Protected。

返回 [指南首页](index.md)
