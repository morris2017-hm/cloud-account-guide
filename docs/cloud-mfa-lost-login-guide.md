---
title: 代开账号的验证器丢了还能登录吗：登录掩码、申诉材料，以及解绑不等于关掉
description: 阿里云国际站主账号的验证器或登录掩码把人挡在外面时要走申诉，材料按四种付款方式分开。安全邮箱两页对能不能用来登录说法不一致。腾讯云解绑必须有动态码，手机丢了按 FAQ 提工单，申请人信息要和实名一致。
permalink: /cloud-mfa-lost-login-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md) · [计费方式与关机扣费](cloud-billing-mode-stop-charging-guide.md) · [系统盘与数据盘](cloud-disk-system-data-guide.md) · [公网 IP 与 EIP](cloud-public-ip-eip-bandwidth-guide.md) · [重装系统](cloud-reinstall-os-guide.md) · [升降配](cloud-instance-type-change-guide.md) · [私钥与 VNC](cloud-ssh-keypair-password-vnc-guide.md) · [自动续费哪天扣](cloud-subscription-auto-renewal-guide.md) · [按量转包年包月](cloud-billing-method-switch-guide.md) · [机器能过户吗](cloud-cross-account-image-migration-guide.md) · [操作记录能查多久](cloud-operation-audit-guide.md) · [验证器丢了](cloud-mfa-lost-login-guide.md)

# 代开账号的验证器丢了还能登录吗

账号到手我会把验证器绑上。手机换了，或者 App 被卸了，登录页还在要那 6 位数。我把阿里云国际站的账号中心、RAM，还有腾讯云国际站的账号保护对了一遍。这几页都没写备用恢复码。阿里云绑定页（2026-09-22）写，卸 App 或换手机之前先解绑，否则登不进去。腾讯云解绑页（2026-08-13）要你输入当前的 6 位动态码。手机不在了，这条就走不通。

下文的验证器指虚拟 MFA，按时间生成 6 位数，大约 30 秒换一次。主账号怎么锁、子用户怎么建，见 [子用户与权限](ram-cam-subuser-permission-guide.md)。谁在哪天绑过、解过，见 [操作记录能查多久](cloud-operation-audit-guide.md)。

## 进得去，和根本进不去

| 情况 | 阿里云国际站 | 腾讯云国际站 |
| :--- | :--- | :--- |
| 验证器还在，人也进得了控制台 | 安全设置里关账号保护，或只关其中一种方式。验证时可以用 App，也可以改用电话或邮箱。可能还要再登录点一次同意 | 解绑窗口输入 6 位动态码。解绑之后，登录保护和操作保护上的 MFA 验证会自动关掉 |
| 验证器没了，登录页过不去 | 走申诉。申诉页（2026-08-20）写，先确认没有别的 MFA，也没有别的登录方式 | FAQ（2024-12-05）写提工单。更新更晚的解绑页没有写这条，因为它假定你拿得出动态码 |
| 地址把你挡在外面 | 登录掩码。清掉走同一份申诉。掩码页写审核一般三个工作日，申诉页自己没写天数 | 我看的账号保护、绑定、解绑和 FAQ 里，没有一份和控制台登录掩码对等的开关 |

两家的入口也不一样。阿里云主账号在账号中心的 Security Settings。腾讯云主账号也在 Security Settings 的 Account Protection。子用户不在这一页解，见后面。

## 阿里云主账号，人还在里面

绑定页写，账号保护可以勾一种或多种场景。绑验证器之前先核身份。选邮箱时，验证码 15 分钟有效。这 15 分钟是邮箱码，不是 App 上那 6 位。App 的码大约 30 秒就换，用过的那一组不能再用。

页面推荐 Google Authenticator，也写了 Microsoft Authenticator。短信是另一种方式。账号还没绑手机时，绑定页写可以先用邮箱核身份，再去绑手机，然后才能开短信保护。国际站短信能不能收到，这页没写，以页面当时发没发出为准。

还进得去、验证器也还在，就在 Account Protection 里关。可以整个账号保护关掉，也可以只关一种验证方式。验证身份时，页面写可以改用电话或邮箱。如果跳出安全审核，或者解绑要异步确认，得重新登录，在通知或待办里点同意。点了一次关闭，不代表已经解开。

Google 登录绕不开。登录说明（2026-08-28）写，用 Google 时如果开了 MFA，还是要再验证。GitHub 那几步没提 MFA。我没把第三方登录当成丢掉验证器之后的入口。

## 登录页已经过不去

申诉页写两种情况用同一份表：虚拟 MFA 不可用，或者登录掩码把你挡在指定地址外面。提交前先确认没有别的已登记 MFA，也没有别的登录方式。工程师会核对身份和付款信息，你授权之后，才解绑验证器或清掉掩码。

登录页上的按钮，两页英文对不上。申诉页（2026-08-20）写的是 Other Sign In Difficulties，再选 I cannot log in (MFA/TOTP and login mask could not be verified)。登录说明（2026-08-28）写的是 Having trouble logging on，再选 Account logon failed。我按当天登录页上的字点，不跟其中一页死磕。

表单里要填一个你现在收得到的邮箱和手机。查询码和进度链接会出现在提交后的页面上，也会发到这个邮箱。它不一定是账号的登录邮箱。两个邮箱我都看。

材料按付款方式分四档。申诉页没有出现「代开」这两个字。渠道结算的账号落在哪一档，以表单当时给的选项为准，我没有把它归进下面任何一行。

| 申诉页上的付款方式 | 要准备的 |
| :--- | :--- |
| Credit Card | 登录邮箱、账号显示名，再加上二者之一：任一笔关联交易记录的扫描件，或者卡号前 6 位和后 4 位外加持卡人姓名 |
| PayPal | 登录邮箱、UID（不知道可以不填）、账号显示名、一笔交易的全屏截图。截图里要有 PayPal 账号、时间、金额 |
| Distributor End User | 登录邮箱、UID（不知道可以不填）、账号显示名 |
| Account Balance（Top-up） | 登录邮箱、UID（不知道可以不填）、账号显示名、一笔充值的全屏截图。截图里要有阿里云账号、时间、金额 |

信用卡那一档没把 UID 列进去。另外三档写 UID 可以不填。账号显示名在控制台哪一行，这页没写，我按账号中心页面上的名字对。

申诉页没写要几天。登录掩码页（2026-09-22）把找回指到同一份申诉，并写审核一般三个工作日。天数以申诉页当时显示的为准。这页写了三天，我就按三个工作日来等，不把它说成承诺。

找回之后，申诉页写要重新开验证器或登录掩码。

## 登录掩码和 RAM 的地址限制不是同一个开关

主账号的登录掩码在 Security Settings 的 Logon Mask。填上之后，只有名单里的地址或网段能进控制台。别的地址直接被挡。最佳实践写的是固定公网，并再加一条你信得过的备用线路。步骤里的例子却是 `192.168.0.1`。前头写公网，例子是内网。我留备用公网，不照着这个例子填。

这层掩码只管阿里云账号在浏览器里进控制台。会话时长那一段写明，它不管 RAM 用户的登录，也不管 AccessKey 调 API。

RAM 自己还有一张表，在安全设置的 Network Access Control 里，更新于 2026-08-13。控制台登录（密码或 SSO）和 AccessKey 调 API 分成两栏。空着表示不限制。只填了 API 那栏，不代表控制台也被挡住。登录这一栏只写了 IPv4，最多 200 个，多个地址用空格、逗号或分号隔开。某个 AccessKey 自己还有更细的网络策略时，细的那条优先。

RAM 用户被这张表挡住时，设置页写：让管理员用阿里云账号登录，再去改。主账号自己的登录掩码如果也填错了，这条就走不通，改走上面的申诉。两层我分开看，不在 RAM 里找主账号的 Logon Mask。

主账号浏览器会话默认 48 小时，可以改成 1 到 72 小时，下次登录才生效。RAM 用户的会话是另一处，默认 6 小时，范围 1 到 24 小时。角色和 SSO 的会话也不会超过这个 RAM 上限。

## 安全邮箱能不能拿来登录，两页不一致

RAM 的方法对比页（2026-09-17）把安全邮箱写成紧急登录的备用，主设备丢了可以用。它还建议每个 RAM 用户至少配两种方式。一个安全邮箱最多绑 5 个 RAM 用户。通行密钥能一次做完密码和二次验证这两步。U2F 已经升级成通行密钥。

安全设置页（2026-08-13）的备注仍写：安全邮箱只用于敏感操作的二次验证。虚拟 MFA 和通行密钥是允许的方式，默认开着，而且不能改掉这个允许。允许，不等于这个用户已经绑上。安全邮箱也是默认勾上的允许项，但要先给这个 RAM 用户绑上邮箱才生效。

方法对比页更新更晚。它没有把安全设置页那句备注改掉。验证器丢了之后，登录页给不给邮箱，我看当天的登录页。没有邮箱入口，就不用更晚的那句去硬闯。

登录 MFA 还有两句日期不一样。方法页写，2025-03-17 起给所有 RAM 用户打开登录 MFA。FAQ（2026-08-26）写，2025-03-28 起按账号 UID 逐步收紧，挂了 `AdministratorAccess` 的 RAM 用户必须用 MFA 登录，不能关掉。我没有合成一个日子。全局默认现在写的是 Force all users，也就是登录都要 MFA。选了这一档，敏感操作也会再要一次 MFA。

解绑和关掉不是一回事。解绑页（2026-09-09）写，设备解开之后，只要安全设置仍要求 MFA，下次登录还得再绑一台。只解掉安全手机，登录不再要短信，验证器仍要。想让控制台登录不再要 MFA，得改两处：全局改成按每个用户，或只在异常登录时要；再把这个用户的 MFA Required 改成 Not Required。`AdministratorAccess` 走不了后面这一步。

用户自己解绑要输入动态码。全局里 Allow users to manage MFA devices 默认是开的，开了也过不了「要动态码」这一下。管理员在这个用户的 Authentication 页解绑，步骤里没写要输入该用户的动态码。主账号还能登录时，RAM 用户丢了手机，我按管理员这条走，不给 RAM 用户去提交主账号那份申诉。申诉页写的是阿里云账号，没有写 RAM 用户单独的表。

验证码对不上，先别申诉。FAQ 写对手机时间，用最新一组还没用过的码。绑定页开太久，二维码会过期，刷新再扫。同一个码扫了多次，App 里会出现好几条，数字不一样，先删掉重复的再扫。还不行，工单里要错误页截图、设备上的时间、账号名、操作时间。这是码错误，不是手机丢了。

RAM 还有一个 7 天记忆，默认关。打开之后，同一台设备 7 天内不再要 MFA。换一个 RAM 用户在这台设备登录，记忆就失效。它不是主账号那 48 小时会话，也不是腾讯云那个 7 天免验证。

## 腾讯云要动态码，没有码就走工单

绑定页（2026-08-13）写 Google Authenticator 或 Microsoft Authenticator。App 里已经有好几个账号时，要挑对应当前账号的那一组码。绑完要选范围：登录保护、操作保护，可以两个都开。换手机的写法是先解绑，再在新手机上绑。这页没写把种子导出去。

解绑页写，解开之后，登录和操作保护上的 MFA 验证会自动关掉。这句说的是 MFA。短信和邮箱保护会不会一起关，这页没写。换机之前我先看账号保护里还开着哪些方式。

账号保护页（2026-08-19）把敏感操作举例成：改登录方式、改密码、改验证手机。这类验证 60 分钟内有效。登录保护和操作保护都开着，而且登录时已经做过二次验证，这 60 分钟里再做敏感操作，不再问一次。过了 60 分钟还是要。

可以同时开多种方式。默认用你上次用过的那种，验证页底部可以换另一种已经开着的。所以只开了验证器、没开邮箱也没开短信时，手机一丢，登录页上没有可换的。U2F 只写了电脑，手机上的验证不支持，而且要先绑上密钥才能选它。虚拟 MFA 也是先绑上，才能把它选成验证方式。

关掉登录保护之后，日常登录按这页写就只剩账号和密码。有三类不算：30 天没登录、登录环境异常、注销账号。这三类关掉保护也不算数，系统仍要核身份。操作保护关掉之后，常规敏感操作不再核身份。上面那三类仍然要。

开启保护时，页面上还有一个可选的 7 天免验证，信任这台登录设备。不需要就别开。这页没写，信任过的那台设备丢了之后怎么取消这 7 天。

手机已经丢了、登录页过不去，看 FAQ，不是看解绑页。有另一个腾讯云账号时，FAQ 写用那个账号提工单，分类是 Account Center，再进 Account Security Settings (MFA)。没有另一个账号时，写到官网底部的 Contact Us。我按页面上的入口走，这里不留邮箱，也不留别的联系方式。

个人模板要三样：登录邮箱，证件上的姓名加护照号或驾照号，现在在用的手机号。FAQ 写，申请人信息和实名认证里填的必须一致。企业要：登录邮箱，公司正式名称加公司编号，申请人姓名加护照号或驾照号，营业执照照片或彩色扫描件，现在在用的手机号。账号还没做身份验证时，这页没有写可以改交什么。证件规则见 [腾讯云身份验证](tencent-cloud-international-identity-verification.md)，不要拿一份对不上的证件去填。

FAQ 还写腾讯云有硬件和虚拟两种设备。绑定页更新更晚，只写了那两个 App。硬件怎么在国际站绑上，我没在绑定页看到步骤，就不写。

子用户的开关不在主账号这一页。子用户安全页（2024-01-23）写，主账号在 CAM 的用户安全里改 Identity Security。开了虚拟 MFA 之后，子用户下次登录要自己绑。关掉也是主账号在这里关。解绑页写，子账号的 MFA 让主账号或有管理权限的人在 CAM 里处理。子用户手机丢了、主账号还能进，我按 CAM 这条，不套主账号那份个人模板。

## 接手时我留的后路

验证器只放在我自己还拿得住的手机上。绑定页写，多人共用一个账号时可以截二维码。主账号那张二维码等于另一套密码，我不发到聊天记录里。每个人用自己的子用户，各绑各的。

阿里云的登录掩码，我有第二条固定公网再填。只填一个会变的地址，下次换网络就得等申诉。RAM 的地址表和主账号掩码分开看。

腾讯云如果准备绑验证器，我同时看邮箱或短信还能不能开。只留验证器一种，丢手机就只剩工单。解绑会把 MFA 验证关掉，换完新手机要再绑，并重新勾登录保护和操作保护。

码对不上，先对时间，再看 App 里是不是扫出了重复条目。确认手机不在了，再走申诉或工单。申诉和工单用的姓名、证件，要和这个账号上已经提交的身份验证一致。阿里云还没验证时材料按付款方式那四档，不按腾讯云的证件模板套。

## 常见问题

**这几页有没有备用恢复码？**
没有。阿里云写的是解绑之前别卸 App。腾讯云解绑要当时的动态码。

**用 Google 账号登录，能跳过验证器吗？**
登录说明写，开了 MFA 还是要再验证。GitHub 那几步没提，我也不把它当入口。

**RAM 用户的验证器丢了，主账号还能进，要去申诉吗？**
不用走主账号那份申诉。管理员在 RAM 里解绑，步骤没写动态码。用户自己解绑要动态码。解开之后，设置如果还要求 MFA，下次登录得再绑。挂了 `AdministratorAccess` 的，FAQ 写不能把登录 MFA 关掉。

**安全邮箱能不能代替验证器登录？**
方法对比页写可以当紧急登录的备用。安全设置页写它只用于敏感操作。两页都在，登录页没有邮箱入口时，按没有这条备用处理。

**申诉要几天？材料是不是四档都交？**
登录掩码页写一般三个工作日，申诉页没写天数。材料只准备和这个账号付款方式对得上的那一档。

**关掉腾讯云登录保护之后，是不是再也不验证？**
日常登录按账号保护页写，只剩密码。30 天没登录、登录环境异常、注销账号，关掉也不算数。

**工单里的姓名能随便填一份吗？**
FAQ 写必须和实名认证里的一致。还没认证的账号，这页没写替代材料。见 [腾讯云身份验证](tencent-cloud-international-identity-verification.md)。

## 账号从哪里来

验证器绑的是已经在你名下的国际版主账号。还在比较阿里云账号购买和腾讯云账号购买、手里还没有 alibabacloud.com 或 tencentcloud.com 的独立主账号时，先看 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 和 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云代开](https://www.yundaxian.com/open/tencent)。交付后自己改密、自己绑验证器。平台解绑要工程师授权，不在开户页面上。

## 官方参考

- [Attach or detach a virtual MFA device](https://www.alibabacloud.com/help/en/account/manage-vitual-mfa)
- [Recover access when MFA or an IP mask blocks logon](https://www.alibabacloud.com/help/en/account/virtual-mfa-not-available-or-ip-mask-restricted-login)
- [Secure account logon](https://www.alibabacloud.com/help/en/account/protect-account-login)
- [Log on to an Alibaba Cloud account](https://www.alibabacloud.com/help/en/account/step-2-log-in-to-your-alibaba-cloud-account)
- [MFA methods supported by RAM](https://www.alibabacloud.com/help/en/ram/user-guide/what-is-multi-factor-authentication/)
- [MFA FAQ](https://www.alibabacloud.com/help/en/ram/support/faq-about-mfa)
- [Manage RAM user security settings](https://www.alibabacloud.com/help/en/ram/user-guide/manage-security-settings-of-ram-users)
- [Unbind an MFA device for a RAM user](https://www.alibabacloud.com/help/en/ram/user-guide/unbind-an-mfa-device-from-a-ram-user)
- [Binding a Virtual MFA Device](https://www.tencentcloud.com/document/product/378/32528)
- [Unbinding a Virtual MFA Device](https://www.tencentcloud.com/document/product/378/82081)
- [Account Protection](https://www.tencentcloud.com/document/product/378/8392)
- [MFA FAQs](https://www.tencentcloud.com/document/product/378/12036)
- [Setting Security Protection for Sub-Users](https://www.tencentcloud.com/document/product/598/32657)

## 一句话总结

阿里云主账号的验证器丢了，走申诉，材料按四种付款方式只准备一档。登录掩码填错走同一份申诉，掩码页写一般三个工作日。安全邮箱能不能用来登录，两页说法不一致，以登录页为准。腾讯云解绑要动态码，手机会丢就按 FAQ 提工单，姓名要和实名一致。解绑不等于关掉登录 MFA，挂了管理员策略的 RAM 用户按 FAQ 关不掉。

返回 [指南首页](index.md)
