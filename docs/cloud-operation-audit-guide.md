---
title: 代开账号到手后，操作记录能查多久：登录、改密，以及谁关了机器
description: 阿里云国际站操作审计默认留 90 天，登录和改密要到新加坡地域查，控制台不能按来源 IP 筛选。腾讯云国际站操作记录一次跨度 30 天，同一页写可以落在过去 90 天里。跟踪只投递创建之后的新事件。
permalink: /cloud-operation-audit-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md)

# 代开账号到手后，操作记录能查多久

账号刚交过来，我先想知道交付前谁登过控制台，谁改过密码，谁把机器关了。我把阿里云国际站的 ActionTrail，和腾讯云国际站的 CloudAudit，按查询、计费、回补这几页对了一遍。两边都写默认就开，不用先建跟踪。阿里云管理事件默认留 90 天。登录、改密、改邮箱、绑 MFA、RAM，2022-12-22 起只能在新加坡地域查。腾讯云操作记录页写，一次筛选跨度 30 天，但这 30 天可以落在过去 90 天里。总览和 FAQ 仍写最近 30 天。控制中心方案页又写 180 天。

控制台里的产品名是 ActionTrail 和 CloudAudit。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。改密和 MFA 本身怎么做，见 [防封清单](cloud-account-risk-control.md)。子用户怎么拆，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

## 默认能往回翻多久

| 项目 | 阿里云 ActionTrail | 腾讯云 CloudAudit |
| :--- | :--- | :--- |
| 不建跟踪时能查多久 | 管理事件 90 天。账号中心 FAQ（2026-07-09）写这段查询不收费 | 操作记录页（2026-07-23）写：过去 90 天内，一次筛选跨度 30 天。总览（2024-12-03）和 FAQ（2024-01-24）仍写最近 30 天 |
| 登录记录在哪 | 全球事件。全球服务页（2026-07-17）写，2022-12-22 00:00:00 起只能在新加坡查 | 这几页没有写必须换某个地域。事件名是 `ConsoleLogin` |
| 新建跟踪会不会带上旧记录 | 不会。创建跟踪页（2026-10-15）写只投递启用之后的新事件 | 创建页可以勾选，把过去 90 天、全部资源类型的事件送进这个跟踪集 |
| 更长的另一种写法 | 保留说明（2026-06-02）写，不建跟踪就查不到 90 天以前 | 控制中心方案页（2024-12-11）写每个账号的操作日志保留 180 天，并可送到 COS、CLS |

阿里云产品介绍（2026-06-15）把事件分成两类。管理事件是创建、修改、删除资源，外加控制台登录 `ConsoleSignin`。数据事件是读 OSS 对象、查 Tablestore 这种，默认不采集，也不存。所以默认这 90 天里，看不到谁读了桶里的对象。要记数据事件，得另建跟踪，再送到日志服务（SLS）或 OSS，存储按那两个产品计费。

腾讯云三套天数我没有合成一个。操作记录页更新更晚，写的是过去 90 天里每次最多框 30 天。总览和 FAQ 还停在「最近 30 天」。控制中心那份是多账号方案，不是云审计产品页，它写 180 天。我查单账号时，以操作记录页当时能选的起止日期为准。接口 `DescribeEvents`（2026-05-27）又是一套英文：`StartTime` 写成不能是当前时间再往后 90 天，`EndTime` 写成查询跨度要小于 30 天，一次最多返回 50 条。这句「往后 90 天」和操作记录页的「过去 90 天」方向对不上。接口解释不清时，我按控制台能选的日期查。

腾讯云总览写，创建账号时就会启用。阿里云写默认开启，自动记下最近 90 天的操作。我没在这些页里看到渠道交付会把交付前的事件清掉。还落在上面那个窗口里的登录和改资源，按下一节的地域去翻。窗口外面的，页面上写查不到，除非当时已经投到你自己的存储里。

限额页（2026-04-09）写，事件要 10 分钟后才能在控制台查，送到 OSS 也是 10 分钟。腾讯云 FAQ 写的是另一件事：送到指定 COS 桶，一般在 API 调用后 5 到 10 分钟。刚在控制台点的，阿里云先等满这 10 分钟再查。

## 登录不在机器那一页

阿里云把不绑地域的服务叫全球服务。全球服务页把账号登录服务（AasCustomer）列在里面，事件包括 `ConsoleSignin`、`ConsoleSignout`、`PasswordModify`、`PasswordReset`、`EmailModify`、`MobileModify`、`MFABind`、`MFAUnbind`。RAM 用户登录是另一项（AasSub），里面也有一条 `ConsoleSignin`，还有 `ChangePassword`、`BindMFADevice`。RAM 本体的 `CreateUser`、`CreateAccessKey`、`DeleteAccessKey` 同样在这份全球事件里。这些都只能在新加坡查。

账号中心（2026-07-09）的点法是：事件查询，顶栏选新加坡，服务名搜 Cloud Account Sign-in Service。全球服务页的英文是 Cloud account login service。我把这两个名字当成同一类主账号登录。子用户要换 RAM user logon service，不要只筛主账号那一项。

ECS 不在这份全球服务名单里。查询页（2026-09-08）写要选目标地域，例子是按资源所在地域查。关一台香港的 ECS、换它的盘、重置实例登录密码，我选香港，不在新加坡里找。账号密码是 `PasswordReset`，那一条仍在新加坡。登录在新加坡，改这台机器在机器所在地域。两处都要看，才对得上同一次登录之后动了哪台。

腾讯云账号事件表（2024-01-24）里，登录是 `ConsoleLogin`。交付后我还会看 `ModifyMail`、`ModifyPhoneNum`、`ChangeMailPassword`、`BindToken`、`UnbindToken`、`ApplyAccountDeactivation`，以及 `QueryKeyBySecretId`（查看 API 密钥明文）。场景页（2024-12-03）写，登录时间、来源 IP、有没有用多因素认证，都会留下。CVM 被关掉时，同一页的例子是按操作时间和来源 IP 去对那次关机。

操作记录列表里，操作者分三种。主账号显示 root。子用户显示子用户名，人已经删了就改显示子用户 ID。角色显示角色名，角色删了就显示角色 ID。交付前如果有人用过子用户再删掉，列表里可能只剩一串 ID。

## 一条记录里到底有什么

阿里云管理事件结构（2026-08-21）里，`eventTime` 写明是 UTC。北京时间晚上的登录，会显示成 UTC 的上午或下午，对钟点时先换时区。`userIdentity.type` 为 `root-account` 时，`userName` 是 `root`，`principalId` 是账号 ID。`ram-user` 会写下这个 RAM 用户的名字。`assumed-role` 写成 `RoleName:RoleSessionName`。

`sourceIpAddress` 多数时候是客户端 IP，IPv4 和 IPv6 都有。请求如果是云服务自己发的，这里会变成服务名，例子是 `ecs.aliyuncs.com`。来源落在 VPC 或阿里云内网、又认不出是你的 VPC 时，记成 `Internal`。认出来了，另外有 `vpcId`。`Internal` 不是空记录。`sessionContext` 在控制台登录或使用临时令牌时出现，里面的 `mfaAuthenticated` 表示控制台登录有没有用 MFA。`eventType` 里，`ConsoleSignin` 是登录，`ConsoleSignout` 是登出，`ApiCall` 是 API，`ConsoleOperation` 是控制台或购买页上的管理操作，`AliyunServiceEvent` 是平台自己对资源做的操作。

控制台登录时，结构页写 `accessKeyId` 不显示。用 SDK 调用会记下 AccessKey ID。用临时安全令牌时，记下临时的 AccessKey ID。角色扮演也会走令牌。只看到一串 AccessKey，分不清是 SDK 还是角色。主账号在浏览器里登录，按这页的写法，这个字段不出现。

查询页写明，事件查询不能用来源 IP 当筛选条件。来源 IP 只是结果里的一列。要对着某个 IP 把事件捞全，得先把管理事件投到日志服务，再查 `event.sourceIpAddress`。同一时段可以按操作者、资源名、事件名翻，再在结果列里对 IP。

账号中心写，`ConsoleSignin` 会记下这次登录是否成功。不熟的 IP 出现时，那一页的建议是改密码、绑 MFA，再把同一时段的其他操作翻出来。改密和 MFA 见防封清单。按 IP 捞全量，受上面这条筛选限制。

腾讯云详情页（2026-07-23）里，`eventType` 为 `ConsoleCall` 是控制台，`ApiCall` 是直接调 API，`MiniProgramCall` 是助手小程序。`sensitiveAction` 为 1 表示平台把它标成敏感操作，为 0 则不是。哪些算敏感由平台定，想把别的操作也标进去，页内写要提工单。`errorCode` 不是 0，是 CAM 鉴权异常。`apiErrorCode` 不是 0，或者 `apiErrorMessage` 不为空，是 API 执行失败。失败的调用也会出现在列表里。`sessionContext.mfa` 为 0 时，说明写的是这次没有做校验、操作不要求通过 MFA。为 1 表示做了检测。详情里有 `sourceIPAddress`，`sessionContext` 里还有一个 `userIp`。两个字段都在时，我两个都看。字段说明给了时间例子，没有写时区。对钟点时以页面上显示的时间为准。

阿里云写的记录范围是控制台、OpenAPI 和开发者工具。腾讯云写的是控制台、API 和命令行工具。系统里 SSH 敲了什么命令，两边列出来的事件里都没有。进机器之后的会话，还是看系统自己的日志。私钥和 VNC 是另一篇，见 [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md)。

## 跟踪只带走新建之后的事件

想把记录留过默认窗口，阿里云要建跟踪。快速创建会跟踪全部管理事件，读和写都在内，送到日志服务。这一档不含数据事件，也不含 Insights，也不能改送到 OSS 或 MaxCompute。自定义那一页如果只勾写事件，读事件就丢掉了。创建页写读事件体积大，但仍建议带上，这样 AccessKey 的使用和资源读取才完整。每个地域最多 5 条单账号跟踪。限额表里，单账号跟踪通过控制台或 LookupEvents 查历史事件的频率写成 2，单位那一格是空的，以配额中心为准。

跟踪的 home region 是你创建时所在的地域。创建页的 FAQ 写，这个地域不决定日志存在哪。送到日志服务时，存在目标 Project 的地域。全球服务页写，全球事件送到 OSS 时，和 home region 的事件放在同一目录。

回补是另一件事。回补页（2026-07-17）写，要先提工单或找商务把账号加进白名单。只能送到日志服务，一个账号同时只能跑一个回补任务，只补管理事件，地域必须和跟踪相同。时间窗是从当前往前 90 天，到跟踪生效后再过 5 分钟。页内例子：跟踪是 40 天前建的，回补只送出跟踪创建之前的那 50 天。账号中心那页写跟踪建好后日志会自动永久保存，没有提白名单，也没有提「只送新事件」。创建跟踪页更新更晚，我以创建页和回补页为准。

腾讯云功能页（2026-06-18）仍写，公测期间最多 5 个跟踪集。创建页（2026-07-08）可以选择 COS、日志服务 CLS 或 CKafka。回补选项的英文是 Backfill Events from the Past Three Months (90 Days)。勾上之后，说明写的是全部资源类型的全部事件都会送进这个跟踪集。这页没写要先加白名单。第一批大约 10 分钟后送出，之后有增量就每 5 到 10 分钟一批。功能页仍写每 5 分钟送到 COS，并且一小时一个文件。投递说明更新更晚。间隔以投递说明为准。文件是不是按小时切开，看桶里的对象。

FAQ 写，关掉跟踪集的日志功能，或删掉跟踪集，会停止往它指定的 COS 送日志。这句没有写控制台里还能查的那一段会被抹掉。默认那段还在不在，以操作记录页为准。

## 查询不收费，存下来才另计

阿里云账号中心 FAQ 写，最近 90 天在控制台查询不收费。跟踪送到日志服务或 OSS，只付那两边的存储。计费页（2026-09-10）写，送到日志服务时主要是存储、写入流量，以及从公网读日志的流量。内网读不收这最后一项。快速创建时，页面上的费用估算按你过去 30 天的事件量来算，实际看账单。同一页写，如果以后对某些功能收费，会提前一个月通知。

数据事件的跟踪删掉之后，日志服务里那个日志库不会跟着删，存储费还在。计费页写项目名通常以 `actiontrail` 开头，要自己去删对应日志库。删了里面的数据就没了。这句针对的是数据事件。管理事件的日志库是不是同一套残留，页内没有写，删之前先看日志服务里的列表。

计费页还有一句：完成实名、状态正常的账号可以使用 ActionTrail。控制台打得开，就按上面查。打不开，再对这句，以及 [阿里云身份验证](aliyun-international-account-verification.md)。创建跟踪时如果在中国内地新建 OSS 桶，创建页写要先完成实名。

腾讯云购买页（2024-01-24）写 CloudAudit 免费。FAQ 写查看和搜索事件历史不收费。跟踪集要选 COS、CLS 或 CKafka，购买页没有写这三样是否另计。以那三个产品自己的账单为准。

## 接手时我怎么对

先别建跟踪。阿里云打开事件查询，地域选新加坡，把主账号登录和 RAM 用户登录分开筛，看交付日前到今天的 `ConsoleSignin`、改密、改邮箱、解绑 MFA、创建 AccessKey。再把地域换成机器所在地域，按实例 ID 看开关机、重置密码、更换系统盘。来源 IP 在结果列里读。出现 `Internal` 或 `ecs.aliyuncs.com` 这种服务名，不要当成没有来源。

腾讯云打开操作记录，在过去 90 天里按 30 天一段往前翻。看 `ConsoleLogin`、改邮箱、改手机、绑令牌和解绑、申请注销、查看密钥明文。操作者是 root 就是主账号。只剩一串 ID 时，对一下 CAM 里这个子用户还在不在。详情里的 `mfa` 和两个 IP 字段一起看。

窗口快到边沿、又想留下交付前那段，再考虑跟踪。阿里云回补要白名单，而且补不了数据事件，也只覆盖最近 90 天里、跟踪创建前的管理事件。腾讯云创建页的勾选写的是过去 90 天全部事件，我没在那页看到白名单。已经滑出这 90 天的，回补页没有写还能再捞回来。180 天那句在控制中心方案页，和操作记录页不是同一套口径。

不熟的 IP、不是你的改密、不是你的解绑 MFA，账号中心的建议是立刻改密码并绑上 MFA，再翻同一时段还改了什么资源。日常不要用主账号，见子用户那篇。

## 常见问题

**香港的机器被关了，去新加坡能看到吗？**
登录在新加坡。关这台 ECS 的管理事件，查询页写要选目标地域，我在香港查。两处的时间都用 UTC 对。

**控制台能按 IP 把登录筛出来吗？**
阿里云事件查询不能。来源 IP 只在结果列里。腾讯云操作记录的筛选项写了时间、事件名、用户名、读写类型、敏感操作、资源标签、资源名、密钥 ID、请求 ID、API 错误码。没把来源 IP 列成筛选条件。详情里再看 `sourceIPAddress`。

**跟踪一建好，交付前的登录会进日志服务吗？**
阿里云不会。只送启用之后的新事件。更早的管理事件要另做回补，还要白名单。腾讯云要看创建时有没有勾上 Backfill Events from the Past Three Months。没勾，创建页没有写会把过去 90 天送进去。大约 10 分钟后送出第一批，这句是投递时效。

**把跟踪删了，存储费停吗？**
阿里云计费页写，数据事件那个日志库不会随跟踪删除，存储继续，要自己删日志库。腾讯云 FAQ 写，关掉或删除跟踪集会停止往 COS 投递。已经落在桶里的文件，购买页没有写会自动删。

**能看到 SSH 进系统之后执行了什么吗？**
阿里云写的是控制台、OpenAPI 和开发者工具。腾讯云写的是控制台、API 和命令行工具。系统里的命令不在这些事件里。

## 账号从哪里来

操作记录查的是已经在你名下的国际版账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。

## 官方参考

- [What is ActionTrail?](https://www.alibabacloud.com/help/en/actiontrail/product-overview/what-is-actiontrail)
- [How long are events retained?](https://www.alibabacloud.com/help/en/actiontrail/support/what-is-the-maximum-number-of-days-that-actiontrail-events-are-retained-for-query)
- [Supported global services](https://www.alibabacloud.com/help/en/actiontrail/product-overview/global-services-that-work-with-actiontrail)
- [Quotas and limits](https://www.alibabacloud.com/help/en/actiontrail/product-overview/limits)
- [Query events in the last 90 days](https://www.alibabacloud.com/help/en/actiontrail/quick-query-events)
- [Management event structure](https://www.alibabacloud.com/help/en/actiontrail/user-guide/management-event-structure)
- [Use ActionTrail to query account operations](https://www.alibabacloud.com/help/en/account/record-account-operations-with-actiontrail)
- [Billing](https://www.alibabacloud.com/help/en/actiontrail/product-overview/billing-1)
- [Create a single-account trail](https://www.alibabacloud.com/help/en/actiontrail/user-guide/create-a-single-account-trail)
- [Create a data backfill task](https://www.alibabacloud.com/help/en/actiontrail/user-guide/create-a-historical-event-delivery-task)
- [CloudAudit overview](https://www.tencentcloud.com/document/product/1021/34384)
- [CloudAudit FAQs](https://www.tencentcloud.com/document/product/1021/30339)
- [CloudAudit features](https://www.tencentcloud.com/document/product/1021/30333)
- [Purchase guide](https://www.tencentcloud.com/document/product/1021/30337)
- [Shipping logs with a tracking set](https://www.tencentcloud.com/document/product/1021/42145)
- [Viewing event details](https://www.tencentcloud.com/document/product/1021/40499)
- [DescribeEvents](https://www.tencentcloud.com/document/product/1021/42703)
- [Account operations recorded by CloudAudit](https://www.tencentcloud.com/document/product/1021/37658)
- [Scenarios](https://www.tencentcloud.com/document/product/1021/30334)
- [Multi-account log collection](https://www.tencentcloud.com/document/product/1220/57831)

## 一句话总结

阿里云默认能查 90 天的管理事件，登录和改密要到新加坡，香港机器的开关机在香港查，控制台不能按来源 IP 筛选。腾讯云操作记录页是过去 90 天里一次框 30 天，总览仍写 30 天，方案页写 180 天，以操作记录页能选的日期为准。阿里云的跟踪只送创建之后的新事件，更早的管理事件要先加白名单再回补。腾讯云不勾回补时，创建页没写会送过去 90 天。数据事件默认没有。

返回 [指南首页](index.md)
