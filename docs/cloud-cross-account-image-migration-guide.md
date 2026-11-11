---
title: 机器能过户到新账号吗：共享镜像、数据盘，以及 IP 会变
description: 阿里云国际站和腾讯云国际站都不能把 ECS/CVM 改挂到另一个账号。同地域共享自定义镜像，数据盘另走快照。中国站和国际站不能互享。取消共享后，阿里云已创建实例的系统盘不能再初始化。
permalink: /cloud-cross-account-image-migration-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md)

# 机器能过户到新账号吗

新开了一个主账号，旧机器还挂在原来的账号下面。我把阿里云国际站的迁移总览、镜像共享、快照共享、CopyImage，和腾讯云国际站的镜像共享、复制、创建镜像页对了一遍。这些页都没有把原实例的账号改掉。阿里云是共享镜像让对方新建，或者用 SMC 在对方账号里做出一张镜像，再由对方创建；SMC 也可以迁进对方已经有的一台实例。腾讯云这几页写的是先复制再共享，由对方新建。公网 IP 会变。余额也不会跟着走。

同账号只换地域，见 [地域怎么选](cloud-region-selection-guide.md)。同账号换可用区，见 [升降配](cloud-instance-type-change-guide.md)。下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。按钮名称以控制台为准。

## 什么跟着走，什么留在原账号

| 项目 | 会不会到新账号 |
| :--- | :--- |
| 实例本身、实例 ID | 不会。目标账号里是一台新实例 |
| 系统盘里做镜像时的文件 | 会，装进自定义镜像，再由对方创建实例 |
| 数据盘 | 不一定在镜像里。不在的话要另共享快照 |
| 公网 IP、弹性公网 IP | 不会。新实例是新地址 |
| 安全组、VPC、密钥对 | 不会。在目标账号里重建 |
| 余额、代金券、未支付订单 | 不会 |
| 快照费 | 腾讯云写明共享镜像的快照费由拥有者出。阿里云共享动作不收费，快照仍在分享方账号下，以分享方账单为准 |

迁完之后，旧实例还在原账号里继续计费，直到你释放它。镜像和快照也一样，删掉才停。释放时盘和地址会不会留下，见 [系统盘与数据盘](cloud-disk-system-data-guide.md) 和 [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md)。

## 阿里云：同地域共享，中国站走不通

[迁移总览](https://www.alibabacloud.com/help/en/ecs/user-guide/migrate-servers)（2026-06-12）把跨账号写成两台实例。同地域：用源实例做自定义镜像，共享给对方，对方再创建。又换地域：先把镜像复制到目标地域，再从那个地域共享出去。

[镜像 FAQ](https://www.alibabacloud.com/help/en/ecs/image-management-faqs)（2026-08-04）写，中国站和国际站之间不能共享镜像。[ModifyImageSharePermission](https://www.alibabacloud.com/help/en/ecs/api-modifyimagesharepermission)（2026-09-15）的错误码 `InvalidImageId.BidMismatch` 写成不能共享给其他站点的用户。一边是 aliyun.com，一边是 alibabacloud.com，这条路直接拒绝。

只能共享自己名下的自定义镜像。别人共享来的，不能再共享出去。FAQ 写，要再给第三方，先用这张共享镜像做出自己的自定义镜像，再共享新的那张。

[共享页](https://www.alibabacloud.com/help/en/ecs/user-guide/share-a-custom-image/)（2026-09-21）把「一张镜像能共享给多少账号」放在配额中心，名字是 Quota of Shared Users per Custom Image，可以申请提高。同一功能的接口写最多 50 个账号，一次最多提交 10 个，多出来的只处理前 10 个。两处口径不一样。这次能不能共享，看配额中心，以及接口会不会返回 `QuotaExceed.ShareImageUser`。

共享这个动作不收费。镜像如果来自付费镜像，对方创建实例时，镜像使用费记在对方。镜像背后的快照还在分享方账号里，分享方的快照账单里有没有这条，以账单为准。

加密镜像要先有角色 `AliyunECSShareEncryptImageDefaultRole`，而且只能共享用客户主密钥（CMK）加密的。服务密钥加密的，接口让你先用 CopyImage 换成 CMK。同一份接口还有两个拒绝码：`OperationDeined.FullImage` 写加密镜像里有多份快照就不能共享；`OperationDeined.EncryptedSnapshot` 写镜像里含加密快照就不能共享。官网这两个错误码都少了一个 i。控制台拒绝时，不要改用接口硬共享。数据盘可以改走快照共享。

云市场镜像做成的自定义镜像，复制到另一个地域时，源镜像必须在目标地域也有售卖，否则是 `OperationDenied.NotPublished`。站点之间既然整类都不能共享，这条主要挡的是同一站点里的跨地域复制。

ECS 的自定义镜像不能共享给轻量应用服务器。轻量的自定义镜像可以共享给 ECS。轻量和云服务器的差别见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。

对方取消共享，或者你把镜像删了，已经创建出来的实例还在跑，只是系统盘不能再初始化。镜像如果还共享着，要先取消共享才能删。复制出去的那张是新的镜像 ID，删源镜像不影响副本。

## 先复制，还是先共享

三处说法不一样。

共享页写：先复制到目标地域，再从那个地域共享。迁移总览里「跨账号又跨地域」也是这个顺序。镜像 FAQ 另写：共享给目标账号，和复制到目标地域，谁先谁后都行。

[CopyImage](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-copyimage)（2026-10-22）把接收方卡住了：自己的镜像可以跨地域复制；别人共享来的，只能在同一地域里复制。错误码 `OperationDenied.SameRegionOnly` 写明，别人共享的镜像不能直接复制到另一个地域。

所以 FAQ 那句「任意顺序」，对接收方不成立。换地域时，让镜像拥有者先复制，再把目标地域里的那张共享出去。接收方如果一定要自己换地域，只能先用共享镜像做出一张属于自己的自定义镜像，再复制这张。同地域的复制，接口是允许的。

[复制页](https://www.alibabacloud.com/help/en/ecs/user-guide/copy-an-image)（2026-11-02）写，镜像跨地域复制目前不收数据传输费，改规则前会先公告。目标地域会生出新快照，占快照容量。这些快照永久保留，不跟源快照的保留策略走。同一地域同时跑的复制任务最多 5 个，多的排队。主账号一次操作最多选 5 个目标地域。镜像太大时，`SizeExceed.Image` 让你改用 CopySnapshot，再在目标地域用快照做镜像。FAQ 写，直接跨地域复制快照，传输费和存储费都有。这和「镜像复制不收传输费」不是同一条。复制页也建议，大于 2 TiB 时先复制底层快照。

账号欠费时，CopyImage 直接拒绝，错误码是 `Account.Arrearage`。

复制页还写，加密镜像要选「复制并加密」。CopyImage 的 `OperationDeined.EncryptedSnapshot` 又写，镜像里含加密快照就不能复制。控制台复制失败时，不要改用接口硬试。

## 数据盘要对方先接受邀请

镜像 FAQ 写，不能从一块数据盘单独做自定义镜像。从快照做镜像时，可以把数据盘快照包进去。包进去的加密镜像，又可能撞上上面的 `FullImage` 拒绝码。这次镜像里有没有数据盘，以创建页上的磁盘列表为准。

只做了系统盘时，Linux 的 `/etc/fstab` 里如果还写着数据盘，新实例可能起不来。FAQ 写，只有新数据盘也是从已经分好区、做好文件系统的快照创建时，才留着那行挂载。磁盘怎么初始化，见 [系统盘与数据盘](cloud-disk-system-data-guide.md)。

数据盘另外走 [快照共享](https://www.alibabacloud.com/help/en/ecs/user-guide/share-a-snapshot)（2026-07-15）。镜像共享页没有写对方还要接受邀请。快照共享要对方先在资源管理里打开 Resource Sharing，到 Resources Shared To Me 接受，不接受就建不了盘。接受之后，对方可以用这份快照建数据盘，或者再复制一份。

共享快照不收费。对方为新建的盘、以及自己做的跨地域副本付费。一份快照最多共享给 64 个账号。一个账号最多共享 1024 份快照。中国站和国际站的快照也不能互享。

接收方不能用共享快照做自定义镜像，不能用它回滚磁盘，不能归档，也不能延长保留期。源快照设成永久时，共享副本按 1096 天保留，源快照自己的保留期不变。源快照是自定义天数时，共享副本跟着同一段。不能直接把这份共享快照再共享出去。要再给第三方，先复制一份再共享副本，或者用它建盘、做一份新快照再共享。已经复制走的快照，取消共享不影响。取消之后，对方不能再查到这份快照，用它创建的盘也不能再初始化。

服务密钥加密的快照不能共享，先复制成 CMK。从导入镜像产生的快照也不能共享。共享中的快照不能直接删，先取消共享。已经过期、但仍处于共享中的快照不会自动释放，存储费还在。

用加密的共享快照建盘时要换密钥。这种盘只能是 ESSD、ESSD AutoPL、ESSD Entry。要别的类型，先复制快照再创建。

## 想让源机器继续开着，走 SMC

镜像拿到的是做镜像那一刻的磁盘。[SMC 跨账号页](https://www.alibabacloud.com/help/en/smc/use-cases/cross-account-migration-between-ecs-instances-in-the-console)（2026-10-28）是另一条：可以增量同步，源实例不用先关机。目标可以是一张自定义镜像，也可以是一台已经存在的实例。

目标账号里会有一台按量的中转实例，名字是 `No_Delete_SMC_Transition_Instance`。目标选的是镜像时，迁完它会自动释放。迁的过程中不要关它、开它、重启、释放，也不要改它的计费方式。目标如果选的是一台已有实例，这台实例自己就是中转，不会自动释放，而且 SMC 会先换掉它的操作系统。系统盘上的自动快照会删掉，回不来。想留住自动快照，要在迁移前关掉「随云盘释放」。

演练会占用按量资源，要计费。目标类型是已有实例时，演练还会给这台机器的所有盘打快照。演练期间机器不可用，数据会丢。别拿还装着业务数据的机器当目标。

控制台这条要在目标主账号下先建一个 RAM 用户，授 `AliyunSTSAssumeRoleAccess`、`AliyunSMCFullAccess`、`AliyunECSReadOnlyAccess`、`AliyunECSAssistantFullAccess`。源账号再做一个信任目标账号的角色。导入在目标账号的 SMC 控制台做。源和目标填反，方向就反了。源实例要在运行中，云助手状态要正常。导入大约 3 到 10 分钟。跨账号时，页面上可能不显示源实例的公网 IP、完整系统名或运行状态，页内写这是预期的，不影响迁移。子用户怎么建，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

默认走公网，源机器要能访问公网。迁移说明写，跨国际地域时公网可能慢，能走 VPC 就走 VPC。迁完是一套新的 VPC，和原来的私网不通。公网 IP、私网 IP 都会变。绑在硬件上的许可证可能失效。香港以及中国内地以外的地域，Windows 选阿里云许可证时，用迁出来的镜像创建实例会收许可证费，金额在创建页。选 BYOL 不收这笔，你自己激活。

中转实例和演练的费用记在目标账号。余额不够，按量资源会起不来。

## 腾讯云：共享来的镜像不能再复制

[共享页](https://www.tencentcloud.com/document/product/213/4944)（2024-05-16）写，一张自定义镜像最多共享给 500 个账号，而且只能同地域。对方不能改名、不能改描述，只能用来创建或重装，也不能再共享。共享出去的副本不占你的镜像配额。每个地域自己的自定义镜像最多 500 张，这是 [创建页](https://www.tencentcloud.com/document/product/213/4942)（2026-06-15）的上限。已经共享的镜像要删除，先取消全部共享关系。别人共享给你的，你删不掉。

账号 ID 在控制台右上角的账号信息里，不是 QQ 号。[ModifyImageSharePermission](https://www.tencentcloud.com/document/product/213/33268) 写，填的必须是根账号。协作者账号会返回 `NotMasterAccount`。不能共享给自己。BYOL 镜像返回 `BYOLImageShareFailed`。加密镜像返回 `UnsupportedOperation.EncryptedImagesNotSupported`。镜像不在 `NORMAL` 状态也不能共享。

[复制 FAQ](https://www.tencentcloud.com/document/product/213/36289)（2024-01-06）写，公共镜像、服务市场镜像、别人共享来的镜像都不能复制。全部地域都支持复制。跨地域复制目前不收费，副本仍占快照容量。页面没写容量上限，也没给固定耗时，只写看网络和排队。换地域又换账号时，只能先复制到目标地域，再把那个地域里的镜像共享出去。接收方不能自己把共享镜像复制走。阿里云至少还允许接收方在同地域复制，腾讯云这页是共享来的直接不能复制。

[快照 FAQ](https://www.tencentcloud.com/document/product/362/17820) 写，共享镜像的快照费由拥有者出，接收方不付这份快照费。共享镜像如果来自付费镜像，使用时镜像费另计，以创建实例那一页为准。

[取消共享](https://www.tencentcloud.com/document/product/213/7148) 不影响已经创建好的实例。对方只是不能再看见这张镜像，也不能再用它新建。

数据盘在创建页上有两句对不上。从实例上点创建时，那一页写，包含系统盘和数据盘的整机镜像要提工单。从镜像页创建时，又写实例有数据盘就默认做整机镜像，也可以只做系统盘。`CreateImage` 有 `DataDiskIds`。这一次窗口里有没有数据盘选项，以页面为准。自定义镜像说明另写，不能用数据盘快照单独做镜像，只有系统盘快照可以。用带数据盘快照的镜像创建实例时，会按快照的数量和容量自动建数据盘，容量可以加大，不能减小。见 [用自定义镜像创建实例](https://www.tencentcloud.com/document/product/213/36304)。

只做了系统盘、`/etc/fstab` 还指着数据盘，新实例可能起不来。数据盘快照要单独共享。[ModifySnapshotsSharePermission](https://www.tencentcloud.com/document/product/362/33299) 写，只能共享数据盘快照，一张最多 50 个账号，只能同地域。对方可以用它创建云盘，不能改这份共享快照的名字和描述。50 是快照的上限，不是上面镜像的 500。接口说明把 `Permission` 的取值解释写成了「共享镜像」，参数本身仍是 `SHARE` 和 `CANCEL`。

2018 年 7 月之后用公共镜像创建的 CVM，创建页写可以不停机做镜像。更早的要先关机，镜像才和当前环境一致。不停机做的镜像，可能缺最后写入的数据。阿里云创建镜像要不要停机，以那一页的提示为准。要一份和磁盘一致的副本，先做快照，见 [快照与备份](cloud-snapshot-backup-guide.md)。

共享页还写，自定义镜像可以在 Lighthouse 和 CVM 之间共享。轻量套餐和云服务器不是同一类机器，见轻量那篇。

## 新机器起来之后再放旧的

安全组、密钥、VPC 都不会跟着镜像走。创建页上重设登录方式。入站仍按最小权限开，见 [安全组怎么配](cloud-security-group-guide.md)。私钥只在创建时给一次，见 [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md)。公网地址是新的，白名单和解析都要改。

新实例能登录、数据在、业务已经切走，再释放旧实例。然后取消共享，删掉不再需要的自定义镜像和快照，不然快照费还在。阿里云取消或删除之后，已创建实例的系统盘不能再初始化。腾讯云取消共享不拆已经在跑的机器。两边都是先取消共享，才能删源镜像。

## 渠道账号上怎么对

共享填的是官方账号 ID，不是充值订单号。阿里云把鼠标放到右上角头像上，账号类型是 Alibaba Cloud Account 时，显示的就是账号 ID。腾讯云在账号信息里看。两个账号都要能登录各自的国际站控制台。渠道账号不要为了这次操作去改付款设置、自己绑卡。

目标账号要有余额，才能创建新实例。阿里云复制镜像在欠费时也会被拒绝。已有账号按账号 ID 充值，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 常见问题

**共享镜像里的密码和密钥，对方能不能直接用？**
共享页要求在做镜像前清掉命令历史、SSH 密钥、网络配置和访问凭证。没清掉的，会进镜像。创建实例时仍可以重设登录方式。只接受你信得过的账号共享来的镜像。阿里云和腾讯云都写了，平台不保证别人共享镜像的完整性。

**中国站的机器能共享到国际站吗？**
不能。阿里云镜像 FAQ 和快照共享页都写了，两个站点不能互享。接口会返回 `InvalidImageId.BidMismatch`。要进国际站账号，在国际站这边重新创建。

**取消共享之后，新机器会停吗？**
不会因为取消共享就停机。阿里云的后果是这台机器的系统盘不能再初始化。腾讯云是不能再拿这张镜像新建。已经在跑的实例还在。

**轻量上的系统能直接共享给云服务器吗？**
阿里云允许把轻量的自定义镜像共享给 ECS，反方向不行。腾讯云共享页写 Lighthouse 和 CVM 可以互享。共享过去之后，套餐里的流量包、固定公网 IP 都不会跟着走。

## 账号从哪里来

迁机器的前提，是源账号和目标账号都是你能登录的国际版主账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。

## 官方参考

- [Server migration](https://www.alibabacloud.com/help/en/ecs/user-guide/migrate-servers)
- [Share a custom image](https://www.alibabacloud.com/help/en/ecs/user-guide/share-a-custom-image/)
- [Copy a custom image](https://www.alibabacloud.com/help/en/ecs/user-guide/copy-an-image)
- [Image lifecycle management FAQ](https://www.alibabacloud.com/help/en/ecs/image-management-faqs)
- [ModifyImageSharePermission](https://www.alibabacloud.com/help/en/ecs/api-modifyimagesharepermission)
- [CopyImage](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-copyimage)
- [Share a snapshot](https://www.alibabacloud.com/help/en/ecs/user-guide/share-a-snapshot)
- [Migrate ECS instances across accounts](https://www.alibabacloud.com/help/en/smc/use-cases/cross-account-migration-between-ecs-instances-in-the-console)
- [Sharing a Custom Image](https://www.tencentcloud.com/document/product/213/4944)
- [Image Replication](https://www.tencentcloud.com/document/product/213/36289)
- [Creating a Custom Image](https://www.tencentcloud.com/document/product/213/4942)
- [Canceling Custom Image Sharing](https://www.tencentcloud.com/document/product/213/7148)
- [ModifyImageSharePermission（CVM）](https://www.tencentcloud.com/document/product/213/33268)
- [ModifySnapshotsSharePermission](https://www.tencentcloud.com/document/product/362/33299)
- [Snapshot FAQ](https://www.tencentcloud.com/document/product/362/17820)
- [Creating an Instance via a Custom Image](https://www.tencentcloud.com/document/product/213/36304)

## 一句话总结

机器不能过户到另一个账号。同地域把自定义镜像共享给对方的账号 ID，对方创建一台新实例。中国站和国际站不能互享。数据盘不一定在镜像里。阿里云的数据盘快照要对方先接受邀请。源快照设成永久时，共享副本按 1096 天保留，源快照本身不变。腾讯云共享来的镜像不能再复制，加密镜像和 BYOL 也不能共享，数据盘快照的上限是 50 个账号，不是镜像的 500。公网 IP 会变。阿里云取消共享之后，已经创建出来的实例，系统盘不能再初始化。

返回 [指南首页](index.md)
