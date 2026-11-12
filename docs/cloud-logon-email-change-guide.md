---
title: 代开账号的登录邮箱怎么换：阿里云改登录名，腾讯云要改三处
description: 阿里云国际站改的是登录名，UID 和资源不动，安全通知按更新更晚的那页跟着走。腾讯云登录邮箱、验证邮箱、联系邮箱各改各的，只改登录邮箱时欠费信还可以留在旧地址。邮箱不可用申诉的付款材料比验证器申诉少一档。
permalink: /cloud-logon-email-change-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md) · [验证器丢了](cloud-mfa-lost-login-guide.md) · [登录邮箱怎么换](cloud-logon-email-change-guide.md)

# 代开账号的登录邮箱怎么换

接手清单里有一句，交付后换绑邮箱。我把阿里云国际站的登录名、邮箱不可用、注册和联系人，还有腾讯云国际站的三种邮箱，按页面日期对了一遍。阿里云改的是登录名。账号 ID 不动，机器和 AccessKey 也不动。腾讯云的登录邮箱、验证邮箱、联系邮箱各有各的修改入口。只改登录邮箱，欠费和到期通知还可以继续发到旧地址。

下文说的是主账号自己的邮箱。RAM 用户那个安全邮箱，见 [验证器丢了](cloud-mfa-lost-login-guide.md)。谁在哪天改过登录名，见 [操作记录能查多久](cloud-operation-audit-guide.md)。

## 两家的修改入口

| 改完之后 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 登录用哪个 | 新的登录名。旧邮箱不能再登 | 新的登录邮箱。注册限制是一个邮箱只能注册 1 个根账号 |
| 找回密码 | 更新更晚的登录名页写，找回密码走新邮箱 | 重置链接发到账号邮箱，48 小时内有效。点开之后还要再核一次身份 |
| 欠费、到期 | 同一页写安全通知走新邮箱。联系人页仍把默认收件人叫成安全邮箱 | 联系邮箱单独改。修改页没写登录邮箱会连带改掉它 |
| 敏感操作的验证码 | 登录名页把二次验证也算在这个邮箱上 | 验证邮箱单独改。注册当时写的是同一个邮箱，后来可以拆开 |
| 账号 ID | 不变。资源、配置、RAM 用户、权限、AccessKey 按该页都不动 | 账号信息页把登录邮箱和账号 ID、APPID 分列。改邮箱的页面没写 ID 会变 |

这几页都没写多久能改一次，也没写冷却时间。控制台如果临时不让改，以页面提示为准，我不补一个官方没写的天数。

## 阿里云：人还在控制台里

登录名页（2026-10-10）写，入口在账号中心安全设置的基础设置，Username 旁边的 Modify。概览页「我的账号」里，Username 旁边也有一个图标。验证可以用手机、原登录邮箱或 TOTP。然后填新邮箱，点 Get Verification Code，再点 Confirm。

同一篇的前提写得更窄。原邮箱还能用时，它只写邮箱或安全手机，没写 TOTP。原邮箱不能用、人还记得这个地址时，它写用这个邮箱登录，再用手机收码。邮箱和安全手机都没有，才写申诉。步骤里的三种方式和前提对不上。人还在里面时，我按安全设置当时给的方式点。

基本信息页（2026-09-22）的英文步骤和登录名页一致，邮箱按钮也是 Confirm。同一页改手机的按钮却是 OK。邮箱不可用页（2026-07-03）更早，按钮写成 Get Verification Code For Free，确认是 OK。中文基本信息页两处都写成「免费获取校验码」和「确定」。我按当天按钮上的字点，不跟其中一页死磕。

RAM 用户、RAM 角色、企业 SSO 登录，改不了主账号的登录邮箱。让能进主账号的人来改。

改完之后，登录名页写三件事都换到新邮箱：登录、找回密码、安全通知。旧邮箱不能再登。UID 按该页不变。渠道账号续费看的是这个 UID，不看旧邮箱。

## 安全邮箱、登录名、联系人

注册页（2026-09-21）先把注册邮箱写成登录方式，也写成接收重要通知的渠道。完善资料时它又换了一个名字。用邮箱注册的，后面要补安全手机。用手机完成注册验证的，后面要补它称之为安全邮箱的验证，验证码仍发到注册时填的那个地址。找回密码页（2026-09-22）写，可以用用户名或邮箱，再按安全设置选短信或邮箱。

联系人页（2026-09-10）写，还没加联系人时，消息中心的通知默认发给账号安全邮箱，并指到基本信息页去改。基本信息页的英文正文和中文正文都只有登录名和安全手机，没有单独的安全邮箱编辑步骤。中文页开头的简介却把安全邮箱和登录名并列写上。名字有三套，步骤我只看到改登录名。

联系人不能拿来登录，也不能做身份验证。加上之后不会自动订阅。对方要点验证信，还要在消息中心里单独勾通知类型。姓名不支持空格，最多 20 个汉字。添加免费，不能批量导入。多一个联系人，登录名还是原来那个。

登录名页更新更晚，它写安全通知跟着新邮箱走。联系人页没有改掉「安全邮箱」这个叫法。我改完登录名，再打开消息中心看默认收件人。收件人还是旧的，就按联系人页继续改，不拿更晚的那句当作已经改完。

中文基本信息页还有一句对不齐。改登录名的步骤里写，安全手机和登录邮箱都不可用时，去邮箱不可用的申诉，用来完成安全手机修改。这句话放在改邮箱的小节下面，结果却写成改手机。英文登录名页只写要申诉，没把结果收成只改手机。邮箱不可用页自己写的是，工程师可以改注册邮箱或手机。我按邮箱不可用页的结果来等。

## 新邮箱已经被人用过

登录名页写，新地址已经属于另一个阿里云账号时，换一个没注册过的，或者先把那个账号的密码找回来。注册页的 FAQ 写，每个邮箱只能绑一个账号。

站点对比页（2026-10-14）多了一层。中国站的登录名和国际站的登录邮箱会放在一起查，保证登录凭证全局唯一。两个站点的账号不能互相改。登录名页的「已被占用」没有写出中国站。我准备新邮箱时，阿里云中国站和国际站都算进去。

## 邮箱和手机都进不去

邮箱不可用页写，还能登录就在线改，验证只用已登记的手机，没提 TOTP。手机也没有，就走申诉。登录页点 Other Sign In Difficulties?，选 Both the registered phone number and email of my account are unavailable，再点 Next。要填原来的邮箱，加上一个现在收得到的新手机和新邮箱。提交后页面给查询码和进度链接。工程师核对身份和付款信息，你授权之后，才改注册邮箱或手机。这页没写要几天。

材料表有三行：信用卡、PayPal、账号余额。信用卡是交易记录的截图或扫描件，或者卡号前 6 位加后 4 位和持卡人姓名，二者备其一。PayPal 和余额都要全屏截图，截图里要有对应账号、时间和金额。

验证器申诉页（2026-08-20）更新更晚，入口句子是 I cannot log in (MFA/TOTP and login mask could not be verified)，按钮是 Next step。那张表有四行，多了 Distributor End User。余额那一行写成 Account Balance（Top-up）。邮箱不可用页没有这一行，也没有把 UID 和账号显示名写进表里。两页都没出现「代开」。渠道结算落在哪一档，以当天表单给出的选项为准。我不用验证器那张表去填邮箱申诉。验证器或登录掩码把人挡住时，走 [验证器丢了](cloud-mfa-lost-login-guide.md)。

另有一份核实身份的工单说明，场景包括自己改不了安全手机。它要的材料是银行交易记录，或卡号前 6 位加后 4 位和持卡人姓名。入口是工单。邮箱不可用走的是登录页申诉。见 [阿里云身份验证](aliyun-international-account-verification.md)。

找回密码页的首页按钮写成 Log On。邮箱不可用页和验证器申诉页写成 Login。我按当天首页上的字点。

## 改邮箱时，安全手机经常拦在前面

手机页（2026-09-22）写，安全手机不能解绑，也不能删，只能换成新号码。账号上必须一直有一个有效号码。一个号码最多绑 6 个阿里云账号。新号码必须属于注册时选的国家或地区。

前提和步骤又不一致。手机还能收短信时，前提只写把码发到当前号码。步骤里却是原手机、登录邮箱或 TOTP 三选一。手机丢了、登录邮箱还在，我按步骤里的邮箱或 TOTP 试。两样都没有，这页指向邮箱不可用的申诉。

FAQ 写这个号码也用于登录和用验证码找回密码。站点对比页写，国际站注册用邮箱，安全验证用中国内地以外的手机，香港、澳门、台湾算在这句的「以外」里。注册 FAQ 写，国际站不能选中国内地作为注册地，也不能绑中国内地手机号。号码所属地区要和注册地区一致，注册地选定后不能改。

身份验证概览把「绑定一个与账号国家或地区不同的手机号」列成企业验证的场景。改手机页和注册 FAQ 又写必须同一地区。三处我都不合成一条。页面拒收这个号码，就停。证件规则见 [阿里云身份验证](aliyun-international-account-verification.md)。

注册页把付款方式写成购买中国内地以外资源前要补的资料，也可以稍后在费用中心改。渠道结算的账号不要为了这段自己绑卡，付款设置保持交付时的样子。

## 腾讯云：登录、验证、联系，三处都看

修改邮箱页（2025-04-15）把三处分开写。

联系邮箱收订阅消息，包括欠费、资源到期和事件，以及不用于验证的那些信。改的时候从账号信息进，页面会再把你带到用户详情，填新地址点 OK。步骤里没有身份验证，也没有把验证码发到新邮箱。改完它让你尽快验证，好让新地址开始收信。去哪点验证，这页没写。

验证邮箱用于身份验证。在安全设置里改，先核身份，再把验证码发到新邮箱。

登录邮箱是账号邮箱，一般就是注册时那个。在账号信息里改，先核身份，再把验证码发到新邮箱。基本信息页（2024-08-05）把账号邮箱、账号 ID、APPID 分列。联系邮箱在同一页，验证邮箱不在这一页，在安全设置里。

修改页建议验证邮箱、验证手机和联系邮箱、联系手机保持一致。它没写登录邮箱必须等于验证邮箱。FAQ（2024-01-03）只解释了账号邮箱用来登录，验证邮箱用来核身份。

注册页（2026-07-08）在注册当时把它们写成同一个。邮箱注册、微信注册里填写的邮箱，以及 Google 注册所用 Google 账号的邮箱，页面都写会用于登录保护、操作保护，并接收账号通知。完善资料时还有一个勾选，勾上之后，手机号也会用于登录和敏感操作验证。勾选没有写会把邮箱换成手机。我到账号保护里看实际开着哪几种。

所以交付后我三处都打开看。只改登录邮箱，欠费信还可以留在联系邮箱。验证码还可以留在验证邮箱。旧邮箱如果还在别人手里，这两处等于没换。

一个邮箱只能绑一个腾讯云账号，绑定登录方式页（2026-07-28）和账号限制页（2026-07-08）都是这个上限。限制页写的是一个邮箱只能注册 1 个根账号。绑定页的步骤里我只看到微信和通行密钥，没看到把邮箱绑上去的点击顺序。登录邮箱的修改在修改邮箱页。

## 密码信去哪，锁又按哪一页

重置密码页（2025-08-21）让你填账号邮箱。链接 48 小时有效。提交新密码之前，弹窗还要再核一次身份。这一下用验证邮箱、验证手机还是验证器，重置页没写。账号保护把改密码算进敏感操作。验证邮箱还是旧的，我先改验证邮箱，再走忘记密码。

已登录时改密码，走安全设置里的 Account Password。改密页（2023-12-26）写，当初没用邮箱注册、后来也没绑上邮箱的，改不了这个密码。现行注册页三条路都会写到一个邮箱，日期比改密页晚。我以账号信息里有没有 Account Email 为准。新密码要和最近三次不同。长度 8 到 20，页面写由字母、数字和符号组成，举例有 `.`、`/`、`_`，不能有空格。它建议至少 90 天改一次。阿里云找回密码页的规则是另一套：8 到 20 位，大写、小写、数字、标点至少占三类，不能有空格。我按正在改的那一家的页面来设。

邮箱登录的锁定，两页也不一样。限制页（2026-07-08）写连续输错 10 次会临时锁定，锁定期内不能解锁，没写锁多久。邮箱 FAQ（2024-01-03）写错一次以上就要图形验证码，错了十次以上锁定一小时，从最后一次输错开始算。我不等一个没写出来的时长，也不把「不能解锁」理解成永久封号。

验证信收不到时，验证问题页（2025-03-27）让你先核对地址，再看垃圾箱，并把 `cloud_noreply@tencent.com` 加进放行名单。企业邮箱如果拦第三方信，它写换一个邮箱，或走页面上的工单。这是文档里的发信地址，用来放行。本文不留联系邮箱。

用 Google 注册的，登录方式页把 Google 登录和邮箱登录分成两种。注册页写 Google 账号的邮箱会接收通知和保护验证，也让你设一个在腾讯云使用的密码。微信注册页写，注册后可以用邮箱或微信登录，步骤里同样要填邮箱。通行密钥一个账号最多 5 个，一个微信最多关联 50 个根账号。通行密钥在，登录邮箱该换还是要换。

## 接手时我改的顺序

先确认登的是主账号。阿里云概览里能看到 Account ID，也就是 UID。腾讯云账号信息里同时有账号 ID 和 APPID。

密码改成只有自己知道的。新邮箱用自己长期能收信的，不共用。阿里云改登录名，改完用新邮箱登一次，再看消息中心的默认收件人。腾讯云先改登录邮箱，再改验证邮箱，再改联系邮箱，三处都对上之后再退出重登。

安全手机如果还不是自己的，阿里云按登录邮箱或 TOTP 去换，新号码先看是不是注册地的号，中国内地号码按注册 FAQ 不收。腾讯云一个手机号最多 5 个根账号，见 [腾讯云身份验证](tencent-cloud-international-identity-verification.md)。

付款设置不动。注册页和购买说明里的绑卡，不拿来改渠道结算。续费提交账号 ID。登录邮箱换了，UID 和账号 ID 按上面的页面不变。在 [云大仙](https://www.yundaxian.com/) 开的账号按这个 ID 原位充，见 [国际云充值](https://www.yundaxian.com/recharge)。别处开的国际站账号，不在这个充值范围内。

## 常见问题

**改登录邮箱会不会换一个账号 ID，机器要不要迁？**
登录名页写不会。UID、资源、配置、RAM、权限、AccessKey 都留在原账号。腾讯云修改邮箱页没写账号 ID 会变。

**阿里云改完，欠费信是不是一定去了新邮箱？**
登录名页写安全通知走新邮箱。联系人页仍把默认收件人叫成安全邮箱，而且加联系人不会自动订阅。我改完看消息中心，不把两页合成一句。

**腾讯云是不是改一处就够？**
修改页给了三个入口。注册页写注册当时是同一个邮箱。它没写后来改登录邮箱会改掉验证邮箱和联系邮箱。

**新邮箱提示已被占用，换一个大小写可以吗？**
两家我看的页面都没写大小写算不算另一个地址。阿里云还要算上中国站登录名。页面说已被占用，我就换一个确实没绑过的。

**邮箱申诉要不要把四种付款材料都交？**
邮箱不可用页的表是三行，没有 Distributor End User，也没写天数。验证器申诉才是四行。只准备和当天表单对得上的那一档。

**收不到腾讯云的验证码，要不要给人发邮件？**
先对地址和垃圾箱，再放行文档里的那个发信地址。还没有，按验证问题页走工单。这里不留邮箱，也不留别的联系方式。

## 账号从哪里来

登录邮箱换的是已经在你名下的国际版主账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。交付后自己改登录邮箱。渠道账号不要为了注册页上的付款说明去绑卡。

## 官方参考

- [Change your logon email address](https://www.alibabacloud.com/help/en/account/update-profile)
- [Manage basic account information](https://www.alibabacloud.com/help/en/account/manage-basic-account-information)
- [How to update your account email address](https://www.alibabacloud.com/help/en/account/account-email-unavailable)
- [Manage addresses and contacts](https://www.alibabacloud.com/help/en/account/add-personal-addresses-and-personal-contacts)
- [Register an Alibaba Cloud account](https://www.alibabacloud.com/help/en/account/step-1-register-an-alibaba-cloud-account)
- [China Website vs International Website](https://www.alibabacloud.com/help/en/account/aliyun-vs-alibaba-cloud)
- [Change your mobile phone number](https://www.alibabacloud.com/help/en/account/specify-or-change-a-bound-mobile-phone-number)
- [Reset a forgotten password](https://www.alibabacloud.com/help/en/account/self-service-login-or-password-reset)
- [Modifying Email Addresses](https://www.tencentcloud.com/document/product/378/36000)
- [Manage Basic Information](https://www.tencentcloud.com/document/product/378/62739)
- [Email and Mobile Number](https://www.tencentcloud.com/document/product/378/17357)
- [Signing Up](https://www.tencentcloud.com/document/product/378/17985)
- [Resetting Your Login Password](https://www.tencentcloud.com/document/product/378/36003)
- [Modifying Account Password](https://www.tencentcloud.com/document/product/378/36001)
- [Tencent Cloud Account Limits](https://www.tencentcloud.com/document/product/378/80431)
- [Email Verification Issues](https://www.tencentcloud.com/document/product/378/68780)

## 一句话总结

阿里云国际站改的是登录名，UID 不动。更新更晚的那页写安全通知跟着新邮箱走，联系人页仍把默认收件人叫成安全邮箱，改完看消息中心。腾讯云登录邮箱、验证邮箱、联系邮箱各改各的，只改登录邮箱时，欠费信和验证码还可以留在旧地址。邮箱和手机都丢了，走邮箱不可用申诉。那张材料表没有验证器申诉里的 Distributor End User 一行。

返回 [指南首页](index.md)
