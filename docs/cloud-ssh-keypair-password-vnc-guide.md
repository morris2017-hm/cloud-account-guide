---
title: 私钥丢了还能登录吗：密钥对、重置密码，以及 VNC
description: 阿里云 ECS、腾讯云 CVM 的私钥只在创建时交给你一次，系统只留公钥。绑定密钥对后 SSH 的密码登录会关掉。忘了密码要在控制台重置。VNC 用系统密码，不看私钥，也不走安全组。
permalink: /cloud-ssh-keypair-password-vnc-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md)

# 私钥丢了还能登录吗

创建 Linux 机器时如果选了密钥对，私钥文件一丢，SSH 就进不去。很多机器这时已经把密码登录关掉了。我把阿里云国际站和腾讯云国际站现在的登录说明对了一遍。两边都只把公钥留在云上。密码可以在控制台重置，运行中的机器多半要重启或关机。VNC 认的是系统用户名和密码，不认私钥，也不走 22 端口。

下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。阿里云轻量应用服务器的急救入口叫 Rescue Logon，按钮和这篇不一样。22 和 3389 怎么放行，见 [安全组怎么配](cloud-security-group-guide.md)。重装时登录方式要重选，见 [重装系统](cloud-reinstall-os-guide.md)。

## 私钥只在创建时给你一次

密钥对分成公钥和私钥。公钥放在云上，最后写进 Linux 的 `authorized_keys`。私钥在你自己电脑上。阿里云 CreateKeyPair（2026-06-18）写，系统只保存公钥，私钥以未加密的 PEM 在创建结果里返回，要自己收好。控制台创建成功后，浏览器会下载一个 `.pem`。登录凭证说明（2026-05-13）没有写事后还能再把这把私钥取回来。腾讯云 SSH 密钥说明（2026-02-11）写得更直：私钥只能下载一次，腾讯云不保留。谁拿到私钥，谁就能登录这台机器。

丢了就新建。腾讯云管理密钥的说明（2024-01-08）写，丢了就新建一把，再绑到实例上。阿里云同样是再创建，或把你还握着的公钥导入。已经创建过的那把，说明里没有第二下载。

阿里云的密钥对属于地域。登录凭证说明写，实例只能绑同一地域的密钥对。资源一览把「跨地域继续用同一把」写成：在本地生成，再把公钥导入目标地域。每个地域最多 500 把，数字在 CreateKeyPair 的限额说明里。腾讯云是每个账号最多 100 把，生成算法写的是 RSA 2048。导入已有公钥时，腾讯云要求这把公钥不带密码，否则控制台登录不了。

密钥在本机生成。阿里云登录凭证说明写，不要在你准备连上去的那台 ECS 里用 ssh-keygen 生成，也不要把私钥留在那台机器上。

## 绑上去之后，SSH 密码登录会关掉

| 项目 | 阿里云 ECS | 腾讯云 CVM |
| :--- | :--- | :--- |
| 控制台能绑的系统 | Linux。AttachKeyPair（2026-09-11）写 Windows 不支持 | 仅 Linux |
| 一台机器几把 | 控制台一台一把。已经绑过的，新密钥替换旧的 | 再绑一把时，旧密钥失效 |
| 什么时候生效 | 运行中要重启。已停止的要启动 | 控制台要求先关机 |
| 绑上之后的 SSH | 用户名密码认证关掉 | 不能再用密码登录 |
| 解绑之后 | 重启后改回用户名密码认证。不生成新密码 | 接口写可以改用密码。当初没设密码的，SSH 仍进不去 |
| 能不能删掉密钥 | 还绑在实例上的不能删 | 绑在实例或自定义镜像上的不能删 |

阿里云控制台的绑定、替换、解绑，都要等重启才生效。DetachKeyPair（2026-06-18）写，解绑后默认改回用户名加密码，同时要重启。它没有写会发给你一个新密码。创建时只选了密钥、没设过密码的，解绑之后还要再重置一次密码。

腾讯云控制台的绑定和解绑都要求实例已经关机。AssociateInstancesKeyPairs 写，原来用密码登录的，绑上密钥后就不能再用密码。实例上如果已经有密钥，旧的会失效。DisassociateInstancesKeyPairs 写，解绑后可以用密码登录；没设过密码的，解绑后 SSH 进不去，要再重置密码。控制台那页只写了解绑步骤，没有把这段再写一遍。页面提示和接口说明不一致时，以控制台为准。

这两套绑定接口都有强制关机参数，默认是关的。官方建议先普通关机。重置密码的接口把强制关机比成拔电源，可能造成数据丢失或文件系统损坏。能在控制台先做普通关机，就不要直接强制关机。

Windows 走远程桌面和密码。阿里云用户指南另有一句：Windows 要自己打开 SSH，控制台不能管这把密钥。和接口说明放在一起看，控制台这套密钥对是给 Linux 用的。

## 用户名填错，重置也进不去

阿里云实例没有默认密码。云上也不保存密码，控制台查不到你当初设的那一串。Linux 默认用户名是 `root`。创建时如果选了 `ecs-user`，登录名就是 `ecs-user`。这个登录名只能在创建时、用指定的 Linux 镜像设。创建之后不能把控制台登录名直接改成 `ecs-user`。同一页写，离线重置密码和绑定密钥对，只作用于创建时设定的那个登录名。你后来在系统里自己加的用户，控制台重置不会改它的密码。

腾讯云重置密码窗口里，用户名默认是系统默认账号：Windows 是 `Administrator`，Ubuntu 是 `ubuntu`，其他 Linux 是 `root`。也可以改成指定用户名。Ubuntu 上用 `root` 去登，密码对了也会失败。创建时如果选了随机密码，密码在消息中心，不在实例详情里。VNC 说明（2024-01-08）也是这个用户名。

阿里云重置分在线和离线，说明见登录凭证页。在线重置不用重启，前提是云助手已经装上。选不了这一项，或者重置失败，就改离线。离线重置要重启才生效。在线失败的常见原因，说明写是机器上的安全软件拦住了云助手下发的改密命令。腾讯云这边，已经关机的实例可以直接重置。还在运行的，重置会强制关机，页面要求先勾同意。业务正忙时，把这步放到低峰。

腾讯云按量实例如果正处在关机不收费，重置密码这步做不了，要先启动。见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。

密码规则两边不一样。阿里云登录凭证说明里的强密码，要同时有大写、小写、数字和特殊字符。腾讯云 ResetInstancesPassword 对 Linux 的要求是 8 到 30 位，建议超过 12 位，不能以 `/` 开头，并且在小写、大写、数字、特殊字符四类里至少占三类。控制台不收这个密码时，以页面提示为准。

## VNC 不看私钥，也不看安全组

SSH 或远程桌面不通时，用浏览器里的 VNC 看机器还在不在。它从控制台连进去，不走你本地到 22 端口的那条路。

阿里云 VNC 说明更新于 2026-09-21。实例要处于 Running 或 Stopping。已经 Stopped 的连不上。从 2023-07-10 起，VNC 不再单独设一串 VNC 密码，用的就是系统登录用户名和密码。只用过密钥对、没有系统密码时，同一页写要先重置密码，再进 VNC。空闲 300 秒会断开。规格 `ecs.ebmhfc7.48xlarge` 不支持 VNC。Linux 默认没有图形界面，进去是字符终端。本地不能直接粘贴，要用页面上的 Enter Copy Commands，最多 2000 个字符，特殊字符和中文不行。安全组把入站拦住时，说明把 VNC 列为仍然可用。VNC 能登录、Workbench 或 SSH 不能登录，密码重置说明的判断是：密码已经进系统了，问题在 SSH 配置。

用 RAM 子用户打开 VNC，至少要有 `ecs:DescribeInstances` 和 `ecs:DescribeInstanceVncUrl`。没有这两项，页面会提示让主账号授权。

腾讯云 Linux 的 VNC 在实例的 Log In 里选。用户名是 `root`，Ubuntu 是 `ubuntu`。密码输入时不显示。一次只能一个人连。不支持复制粘贴、中文输入法，也不能传文件。前提是你已经有管理员账号和密码。随机密码去消息中心找。忘了就先重置。浏览器要求 Chrome、Firefox，或 IE 10 及以上。

绑定密钥之后，SSH 的密码登录是关的。VNC 走的是系统账号密码。重置密码的说明没有写它会把已经绑定的密钥解掉。所以可能出现 VNC 能进、SSH 仍拒绝密码。这时要么新建一把密钥再绑定，要么解绑后改回密码登录。已经通过 VNC 进了系统、又想两条路都留着，阿里云登录凭证说明写可以改 `sshd_config` 里的 `PasswordAuthentication`，改完重启 SSH 服务。同一页把打开密码认证标成不推荐。

## 私钥丢了，可以怎么处理

先确认地域和用户名。阿里云到重置密码对话框里看创建时的登录名。腾讯云分清是不是 Ubuntu。

还想继续用密钥登录：在同一地域新建一把，或把你还握着的公钥导入。阿里云绑定后，运行中的实例要重启，已停止的要启动。腾讯云先在控制台普通关机，再绑定。新密钥会替换旧的。浏览器弹出的新私钥当场收好。

想改回密码：阿里云解绑并重启，再重置密码。解绑本身不产生密码。腾讯云先关机再解绑。接口写没设过密码的还要再重置。运行中的腾讯云实例，在控制台重置密码会强制关机。

重置之后用 VNC 进一次。VNC 能进，再试 SSH。SSH 仍失败，先看安全组有没有放行 22，再看是不是还绑着密钥。Windows 用重置后的密码走远程桌面。

## 常见问题

**控制台能把旧私钥再下载一次吗？**
不能。阿里云说明系统只保存公钥，私钥在创建结果里返回一次。腾讯云写明只能下载一次。丢了就新建一把再绑定。

**绑定密钥对之后，原来的密码还能 SSH 吗？**
阿里云 AttachKeyPair 写，绑定后用户名密码认证关掉。腾讯云 SSH 密钥说明写，绑定后不能再用密码登录。解绑并让它生效之后，才改回密码这条路。解绑不表示你已经知道密码。

**VNC 能用私钥登录吗？**
不能。阿里云写 VNC 是密码认证。腾讯云 VNC 的前提也是管理员账号和密码。

**换地域之后，原来的密钥对还能绑吗？**
阿里云不能直接绑。密钥对留在原地域。到目标地域新建一把，或把公钥导入目标地域。实例本身也不能直接改地域，迁移见 [地域怎么选](cloud-region-selection-guide.md)。

## 账号从哪里来

登录凭证改的是已经有的那台 ECS 或 CVM。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [Instance logon names, passwords, and key pairs](https://www.alibabacloud.com/help/en/ecs/user-guide/instance-logon-credential-management)
- [What are ECS resources](https://www.alibabacloud.com/help/en/ecs/user-guide/overview-47)
- [CreateKeyPair](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-createkeypair)
- [AttachKeyPair](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-attachkeypair)
- [DetachKeyPair](https://www.alibabacloud.com/help/en/ecs/developer-reference/api-ecs-2014-05-26-detachkeypair)
- [Connect to an instance using VNC](https://www.alibabacloud.com/help/en/ecs/user-guide/log-on-to-an-instance-by-using-vnc)
- [SSH Keys](https://www.tencentcloud.com/document/product/213/6092)
- [Managing Instance SSH Keys](https://www.tencentcloud.com/document/product/213/16691)
- [AssociateInstancesKeyPairs](https://www.tencentcloud.com/document/product/213/33232)
- [DisassociateInstancesKeyPairs](https://www.tencentcloud.com/document/product/213/33228)
- [Resetting Instance Password](https://www.tencentcloud.com/document/product/213/16566)
- [ResetInstancesPassword](https://www.tencentcloud.com/document/product/213/33240)
- [Connecting to a Linux Instance via VNC](https://www.tencentcloud.com/document/product/213/32494)

## 一句话总结

私钥只在创建时给出，云上只留公钥。绑定密钥对之后，SSH 的密码登录会关掉，新密钥替换旧密钥。忘了密码就在控制台重置。阿里云在线重置依赖云助手，腾讯云运行中重置会强制关机。VNC 用系统密码，不看私钥，安全组拦住 SSH 时它仍然可以用来看机器。

返回 [指南首页](index.md)
