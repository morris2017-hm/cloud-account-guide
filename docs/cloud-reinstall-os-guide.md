---
title: 重装系统会清掉什么：更换操作系统、系统盘快照，以及香港能不能 Linux 换 Windows
description: 阿里云 ECS 更换系统盘会释放原盘，操作系统迁移才把数据留在系统盘上。腾讯云 CVM 重装会清空系统盘。中国香港及海外地域不能在 Linux 和 Windows 之间互换，原系统盘快照也不能回滚到新盘。
permalink: /cloud-reinstall-os-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md)

# 重装系统会清掉什么

系统装坏了，或者公共镜像选错了，控制台上就有重装。我把阿里云国际站和腾讯云国际站现在的说明对了一遍。清掉的是系统盘里的文件。账号还是原来的账号，地域也不变。中国香港、新加坡、东京、美国、欧洲还有一条限制：Linux 和 Windows 不能互相换。

下文的 ECS 指阿里云国际版云服务器，CVM 指腾讯云国际版云服务器。轻量应用服务器和 Lighthouse 的重置是另一套，见 [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md)。按钮和镜像列表会变，以控制台预检查为准。

## 更换系统盘，和腾讯云的重装

阿里云更换操作系统的页面上有两条路。这一节是会把原来的系统盘放掉的那条。留住系统盘数据的那条，我放在后面。腾讯云国际站我对着看的是重装系统（2024-01-08）和镜像更换 FAQ（2024-01-06）。阿里云把更换前的手动快照写成必须。腾讯云要求事先备份系统盘里要留的数据，想留系统环境就做成自定义镜像再重装。

| 项目 | 阿里云更换系统盘 | 腾讯云重装系统 |
| :--- | :--- | :--- |
| 系统盘上的文件 | 原盘释放，数据和分区删除。新盘有新的磁盘 ID | 系统盘清空。FAQ 写回到初始状态。可以选当前镜像，也可以换一张 |
| 云盘类型 | 这次操作改不了类型。容量可以加大 | 类型改不了。云盘系统盘容量只能加大，不能缩小，加大的部分按云硬盘计费 |
| 数据盘 | 同族更换时保留，登录后重新挂载。Linux 和 Windows 对调时，文件系统不能直接认 | 数据保留，重新挂载之后才能用 |
| 地址 | 实例 IP、弹性网卡 MAC 保持不变 | 说明写公网 IP 不变。私网 IP 没有单独的一句 |
| Linux 与 Windows 互转 | 只支持中国内地地域 | 只支持中国内地的 CVM |
| 耗时 | 大约 10 分钟，完成后自动重启 | 一般 10 到 30 分钟 |
| 费用 | 更换这个功能本身不收费。付费镜像、加大容量另计 | 加大云盘容量才按云硬盘价格计 |
| 抢占式实例 | 这份说明没有单独写 | 不支持重装 |

阿里云依据是更换系统盘说明，页面标注 2026-10-16。正在运行的实例要先关机。按量实例建议用标准停机。包年包月如果在这次把系统盘加大了，要先付清订单，更换才会开始。

腾讯云重装过程中不要做别的操作。没超过 30 分钟就继续等。超过了，说明写提交工单。重装不会自动把盘变大，只有你指定了更大的容量才会变。扩容之后系统里为什么还是旧容量，见 [系统盘与数据盘](cloud-disk-system-data-guide.md)。

想把当前系统留一份副本，腾讯云的建议是先做自定义镜像，再用这张镜像重装。镜像是副本。重装仍会清掉系统盘里此刻的文件，装回去的是做镜像时的那一份。之后又写进系统盘的东西不在镜像里。自定义镜像用快照存数据，留着会占快照容量。创建说明更新于 2026-06-15。快照怎么计费，见 [快照与备份](cloud-snapshot-backup-guide.md)。

换地域要另开机器，公网 IP 也会换，见 [地域怎么选](cloud-region-selection-guide.md)。

## 香港不能把 Linux 换成 Windows

阿里云跨系统族更换系统盘，只支持中国内地地域。其他地域只能 Linux 换 Linux，或者 Windows 换 Windows。腾讯云同平台重装，所有地域都可以。跨平台只支持中国内地。中国香港不在内地里。新加坡、东京、首尔、硅谷、弗吉尼亚、法兰克福同样只能同平台。

国际版账号要买内地地域，还有验证这一步。阿里云见 [阿里云身份验证](aliyun-international-account-verification.md)。腾讯云没完成身份验证就不能购买或使用内地地域服务，见 [腾讯云身份验证](tencent-cloud-international-identity-verification.md)。

腾讯云还写了，有些已经存在的 20 GB 本地盘实例，控制台不支持跨平台重装，需要提工单。新的云盘和本地盘实例，在允许跨平台的地域里可以。抢占式实例直接不支持重装。抢占式总览页 2026-11-04 仍写着不能重装系统，也不能升降配，也不能扩容云盘。阿里云这份更换说明没有单独写抢占式。预检查失败，就表示这台不能换。

数据盘的文件系统也跟着系统族走。这条主要撞在内地的跨族更换上。阿里云写，同族更换时数据盘不受影响，登录后重新挂载。Linux 换成 Windows 时，Windows 不能直接认 ext4 和 XFS，说明举例用 Ext2Fsd 去读。没有重要数据时，也可以重新初始化。Windows 换成 Linux 时，Linux 不能直接认 NTFS，说明举例安装 ntfs-3g，同样可以在没有重要数据时重新初始化。初始化会把这块数据盘清掉。腾讯云重装说明没有再写跨平台之后认不认文件系统。内地真要跨平台，挂载前先确认，认不了就不要格式化。

用一张带数据盘的自定义镜像来做阿里云更换时，也只换系统盘。原来挂在实例上的数据盘不动。新系统里的开机脚本如果会去动数据盘，那是镜像自己的行为。

## 阿里云想留住系统盘，只能走列出的迁移

操作系统迁移把数据留在原来的系统盘上。它只覆盖说明里列出的版本，页面标注 2026-10-30。它不做热迁移。迁移时会换内核、装软件包，机器重启多次，网站会断。开始前自己先做一份手动快照。迁移过程中不要手动关机、重启或释放。

实例要在运行中。云助手要在最近 24 小时内活跃过。快照服务要开着。用 RAM 子用户操作时，主账号要授 `AliyunSMCFullAccess`。子用户怎么配，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。

Linux 这边，文档列出来的主要是 CentOS 7 或 8 迁到 Alibaba Cloud Linux、AnolisOS，以及部分 Red Hat。Red Hat 7.9 可以迁到 Alibaba Cloud Linux 3 或 Red Hat 8。Red Hat 8 可以迁到 Red Hat 9 或 Alibaba Cloud Linux 3。目标 Red Hat 不能指定小版本。x86 的 Alibaba Cloud Linux 2 可以迁到 3。CentOS 7.9 到 Red Hat 7、CentOS 8.5 到 Red Hat 8，说明写明只对部分用户开放，要提工单。ARM 的目标比 x86 少，表上没有 Red Hat。小版本以迁移说明那张表为准。控制台检查不通过，就做不了。

从 CentOS 7 迁到 Alibaba Cloud Linux 2 或 3 时，说明写内核互不兼容。CentOS 7 是 3.10，Alibaba Cloud Linux 2 是 4.19，Alibaba Cloud Linux 3 是 5.10。CentOS 8 迁到 Alibaba Cloud Linux 3 时，说明写软件包生态是兼容的，业务仍要自己验证。靠自行编译内核模块的程序，先在目标系统上试过再迁。系统里如果装了 i386 或 i686 的包，迁往 Alibaba Cloud Linux 会失败，因为对方不提供这些包。MySQL 只能迁到相同或更高版本。

资源上也有下限。内存至少留出 600 MiB。`/usr` 至少 250 MiB 空闲，`/var` 至少 4 GiB，`/boot` 至少 100 MiB。机器的 CPU、内存或磁盘很紧时，说明建议先把业务停掉再迁，避免空间不够失败。

Windows 不能跨语言迁。Windows 这条还写了，正在创建快照时不能开始迁移。Windows Server 2008 R2 不能直接到 2016 或 2019，要先迁到 2012 R2。2012 R2 可以到 2016 或 2019。2016 可以到 2019 或 2022。2019 可以到 2022。实例至少 2 个 vCPU 和 2 GiB 内存。C 盘至少 20 GiB 空闲，并且空闲空间要大于 `C:\Windows` 目录的大小。

Windows 迁移另有一张地域表。中国香港在表里，东京、首尔、新加坡、吉隆坡、雅加达、马尼拉、曼谷、法兰克福、伦敦、硅谷、弗吉尼亚、利雅得也在，一批中国内地地域也在。完整名单以迁移说明那张表为准。所以香港的 Windows 可以按这张表做版本迁移，仍然不能换成 Linux。Linux 迁移那一节没有同样的地域表，能不能做以预检查为准。

迁移会自动给云盘打快照。Windows 迁移还会临时挂上一块中转盘，那是安装介质的快照。不要去动它。做完会自动卸掉并释放。演练或取消可能回滚到迁移前的快照，迁移过程中新写的数据会丢。

完成后机器自动启动。说明写实例 ID、镜像 ID、VPC、公网和私网 IP、主机名、机器码都不变。系统变成目标版本，原系统盘数据还在。程序未必还能跑，要自己测。想退回去，用这次自动做的快照回滚云盘。用不到的快照及时删，不然快照费还在。

状态如果停在运行中（操作系统更换失败），页面上能看到原因。处理好再点重试。说明写成功与否不保证，并建议先拿一台测试机走一遍。

## 旧系统盘的快照，回滚不了新盘

更换系统盘之后，原系统盘的快照还在，但是不能用来回滚这块新盘。磁盘 ID 已经换了。

手动快照会保留。自动快照看「随云盘释放」这个开关。开了，自动快照跟着旧盘删掉。关了，自动快照留到自己的生命周期结束，这段时间继续计费。原来的自动快照策略不会套到新系统盘上，要重新设。

旧系统盘里还有要捞的文件时，用更换前那份手动快照新建一块按量云盘，挂到这台实例上拷出来。拷完把这块临时盘释放掉。不释放的话，容量费一直在。

## 登录方式，以及几种起不来的情况

更换系统盘时登录方式要重设。Linux 可以用密钥对，也可以设密码。用户名可以是 `root` 或 `ecs-user`，说明推荐 `ecs-user`。Windows 默认用户名是 `administrator`。自定义镜像或共享镜像如果里面已经有密码，可以沿用镜像里的密码。选更换后再设置的话，绑定密钥或重置密码之前登不进去。

腾讯云重装窗口里也要选登录方式。只有自定义镜像和共享镜像可以选跟随镜像。重装接口没指定密码时，会生成随机密码，发到消息中心。同一份接口说明把余额不足、存在未支付订单列成失败原因。控制台如果拦下来，以页面提示为准。

镜像列表是空的，常见是镜像和规格不匹配。支持 NVMe 的规格要用带 NVMe 驱动的镜像。只支持 UEFI 启动的规格要用 UEFI 镜像。八代这一类规格只支持特定系统。内存不到 1 GiB 的阿里云实例只能用 Linux 镜像。换成 Windows 时，系统盘至少还要有 1 GiB 可用空间，否则更换后起不来。Red Hat 镜像只给通过 Red Hat 认证的规格用。Alibaba Cloud Linux Pro 系列，说明写目前不支持换成别的操作系统。很老的非 I/O 优化实例，控制台不能换成 Windows，只能走 API，而且目标只限说明里列出的几张旧版 Windows Server 公共镜像。

腾讯云用 Windows Server 2016 或 2019 镜像时，内存要大于 2 GB。两边的内存下限各看各的说明。

阿里云按量实例在更换前如果选了节省停机，换完可能因为库存起不来。说明建议这次关机用标准模式。腾讯云处于关机不收费时不能重装，要先启动。重装这种运维导致的关机，也不按关机不收费来停 CPU 和内存的费用。这两种停机各自还在计什么，见 [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md)。

主机名也会卡跨族更换。阿里云写 Windows 主机名要是 2 到 15 个字符。这条用在能做跨族更换的内地地域。

## 常见问题

**网站程序放在系统盘里，重装还能看到吗？**
看不到。阿里云更换系统盘会释放原盘。腾讯云重装会把系统盘清空。程序和数据库如果只放在系统盘，先做手动快照或自定义镜像。希望以后重装还在的数据，放在数据盘上。

**数据盘是不是一点都不受影响？**
文件还在。新系统不会自动沿用原来的挂载，要自己挂。内地从 Linux 换到 Windows，或者反过来，文件系统可能对不上。这时不要初始化。

**公网 IP 会变吗？**
阿里云把实例 IP 和弹性网卡 MAC 写成不变。腾讯云把公网 IP 写成不变。这次操作也不换地域。地址没变的话，指着这个地址的解析不用改。安全组我没有在删除项里看到。重装完从外网测一次 22 或 3389、80、443。规则怎么写，见 [安全组怎么配](cloud-security-group-guide.md)。

**想升级 CentOS，又怕系统盘被清掉，该点哪一个？**
点操作系统迁移，而且目标要在允许的版本对里。点更换系统盘会把原系统盘放掉。迁移会重启多次，先做手动快照。

**抢占式机器能重装吗？**
腾讯云不能。阿里云以更换页面的预检查为准。

## 账号从哪里来

重装发生在已经有的那台 ECS 或 CVM 上，不会因此新开一个云账号。还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。已有账号按账号 ID 续费，见 [国际云充值](https://www.yundaxian.com/recharge)。

## 官方参考

- [Replace the operating system (replace the system disk)](https://www.alibabacloud.com/help/en/ecs/user-guide/replace-the-operating-system-of-an-instance)
- [OS migration and upgrade](https://www.alibabacloud.com/help/en/ecs/user-guide/migrate-the-operating-system-of-an-ecs-instance)
- [Restore data from a system disk snapshot after replacing the system disk](https://www.alibabacloud.com/help/en/ecs/user-guide/use-a-snapshot-of-the-original-system-disk-to-restore-data-after-the-operating-system-of-an-ecs-instance-is-replaced)
- [Reinstalling the System](https://www.tencentcloud.com/document/product/213/4933)
- [Image Changing (Reinstalling the System)](https://www.tencentcloud.com/document/product/213/36290)
- [ResetInstance](https://www.tencentcloud.com/document/product/213/33242)
- [Creating a Custom Image](https://www.tencentcloud.com/document/product/213/4942)
- [Spot Instance Overview](https://www.tencentcloud.com/document/product/213/17816)
- [Shutting Down Pay-as-You-Go Instances to Avoid Charges](https://www.tencentcloud.com/document/product/213/19918)

## 一句话总结

要一块全新系统，用阿里云更换系统盘或腾讯云重装，系统盘里的文件会清掉。数据盘的文件还在，登录后要重新挂载。中国香港和海外地域不能在 Linux 和 Windows 之间换。想把阿里云系统盘上的数据留下来，只能走说明列出的操作系统迁移。原系统盘的快照回滚不了新盘。

返回 [指南首页](index.md)
