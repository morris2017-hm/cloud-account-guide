---
title: 阿里云 RAM、腾讯云 CAM 子用户怎么设：国际版账号接手后的权限分配指南
description: 阿里云国际版、腾讯云国际版账号购买或代开到手后，如何用 RAM / CAM 子用户分权：主账号保护、最小权限、MFA、AccessKey 管理与离职交接清单。
permalink: /ram-cam-subuser-permission-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md)

# 阿里云 RAM、腾讯云 CAM 子用户怎么设

不管是自己注册，还是通过阿里云代开、腾讯云代开拿到国际版账号，到手后最常见的错误是：一个主账号密码在团队里传来传去，开发、运维、财务都用它登录。这样做一旦有人离职、电脑中毒或密码泄露，整个账号连同余额和服务器都暴露在风险里，而且出了问题很难查是谁操作的。

阿里云国际版的 RAM（Resource Access Management）和腾讯云国际版的 CAM（Cloud Access Management）就是为解决这个问题设计的：主账号只负责「拥有」，日常操作交给权限受限的子用户。本文讲清楚怎么分、分到什么粒度，以及交接时要做什么。具体菜单名称和策略名称以官方控制台为准。

## 先理解三个角色

| 角色 | 阿里云国际版 | 腾讯云国际版 | 应该谁来用 |
| :--- | :--- | :--- | :--- |
| 主账号 | 阿里云账号（16 位 UID） | 主账号（账号 ID） | 只给账号负责人，平时尽量不登录 |
| 子用户 | RAM 用户 | CAM 子用户 | 每个成员一个，按职责授权 |
| 程序身份 | 带 AccessKey 的 RAM 用户或 RAM 角色 | 带 API 密钥的子用户或角色 | 脚本、CI、应用程序 |

主账号对账号内所有资源拥有完全权限，这个权限无法被限制，所以它必须是被保护得最好的那个身份。子用户本身不拥有资源，它的权限完全来自你给它绑定的策略，可以随时收回。官方说明见 [What is RAM](https://www.alibabacloud.com/help/en/ram/what-is-ram) 和腾讯云 [CAM 文档](https://www.tencentcloud.com/document/product/598/10583)。

## 第一步：先把主账号锁好

在创建任何子用户之前，先处理主账号本身：

1. 修改登录密码，换绑成你长期掌控的邮箱和手机号。
2. 绑定虚拟 MFA（Google Authenticator、Microsoft Authenticator 等），腾讯云参考 [绑定虚拟 MFA 设备](https://www.tencentcloud.com/document/product/378/32528)。
3. 腾讯云国际版建议同时开启登录保护和操作保护，阿里云国际版在安全设置里开启登录验证相关选项。
4. 检查主账号下是否已有 AccessKey / API 密钥。主账号的密钥等于整个账号的钥匙，没有必要就删除，需要程序调用时改用子用户的密钥。
5. 把 MFA 的恢复信息（备用码或绑定设备）妥善保存，至少有一份离线备份。

如果账号是渠道代开的，交付后第一时间完成上面几步，这也是验收的一部分。完整的验收清单见 [阿里云代开流程与验收](aliyun-agency-opening.md) 和 [腾讯云代开流程与验收](tencent-cloud-agency-opening.md)。

## 第二步：按职责创建子用户

原则只有一条：**一人一个子用户，一个程序一个身份，不共用。**

常见的小团队可以这样拆：

| 子用户 | 典型权限范围 | 是否需要控制台登录 | 是否需要密钥 |
| :--- | :--- | :--- | :--- |
| 运维负责人 | ECS / CVM、VPC、安全组、云盘的管理权限 | 是 | 视情况 |
| 开发人员 | 只读或特定资源的操作权限 | 是 | 一般不需要 |
| 财务 | 费用中心 / 账单只读，必要时允许付款相关操作 | 是 | 否 |
| 部署脚本 / CI | 只允许操作指定的资源，比如上传到某个 OSS / COS 存储桶 | 否 | 是 |
| 监控程序 | 云监控只读 | 否 | 是 |

阿里云创建方法见 [Create a RAM user](https://www.alibabacloud.com/help/en/ram/user-guide/create-a-ram-user)。创建时注意：给人用的子用户勾选控制台访问，不要顺手生成 AccessKey；给程序用的子用户只开 API 访问，不开控制台登录。

## 第三步：最小权限授权

刚开始最省事的做法是给所有人绑「AdministratorAccess」之类的全权限策略，但这样分用户就失去了意义。建议按下面的顺序收紧：

- **先用系统策略按产品授权**：例如只给 ECS 管理、只给 OSS 只读。阿里云和腾讯云都提供大量预置策略，按产品名搜索即可。
- **再用用户组管理**：把「运维组」「开发组」「财务组」建成用户组，策略绑在组上，新人入职加入对应组即可，离职移出组。
- **最后对敏感资源写自定义策略**：比如部署脚本只能写某一个存储桶，监控程序只能读不能改。
- **费用相关权限单独给**：能看账单、能续费、能退订的人越少越好，避免误操作把包年包月资源退掉。

授权后，用子用户实际登录一次，确认该能做的能做、不该做的提示无权限，再交给对应成员。

## 第四步：所有子用户都开 MFA

阿里云可以为 RAM 用户单独绑定 MFA，见 [Bind an MFA device to a RAM user](https://www.alibabacloud.com/help/en/ram/user-guide/bind-an-mfa-device-to-a-ram-user)；腾讯云的子用户同样支持 MFA 和登录保护。建议在账号的安全设置里要求子用户登录必须使用 MFA，这样即使某个成员的密码泄露，也不会直接被登录。

## 第五步：AccessKey / API 密钥管理

程序密钥是被盗用最多的入口，常见事故是密钥被提交到公开的代码仓库，然后被人批量开机挖矿，余额和信用都被耗光。几条硬规矩：

1. 密钥不写进代码，不提交到 Git，放在环境变量或密钥管理服务里。
2. 每个程序单独一对密钥，权限只覆盖它要用的资源。
3. 定期轮换：先创建新密钥、更新程序、确认正常后再禁用并删除旧密钥。
4. 发现泄露时第一时间禁用密钥，再排查有没有被创建陌生实例。
5. 能在服务器上用实例角色（阿里云 RAM 角色、腾讯云 CAM 角色）的场景，优先用角色，避免在机器上存放长期密钥。

## 成员离职或外包结束时的交接清单

- [ ] 禁用或删除该成员的子用户，而不是只改密码
- [ ] 删除该子用户名下的所有 AccessKey / API 密钥
- [ ] 检查他是否创建过其他子用户或密钥
- [ ] 如果他曾知道主账号密码或持有 MFA，立即修改主账号密码并重新绑定 MFA
- [ ] 在操作审计（阿里云 ActionTrail、腾讯云 CloudAudit）里回看近期的关键操作

## 常见问题

**代开的账号能创建子用户吗？**
正规代开交付的是独立主账号，可以正常创建 RAM / CAM 子用户。如果你登录后发现自己无法创建子用户、无法修改密码或 MFA，说明拿到的很可能是别人的子用户而不是主账号，参考 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md) 里的核对方法。

**子用户能看到余额和充值吗？**
取决于你给的权限。默认没有费用相关权限的子用户看不到账单，需要单独授权。

**子用户多了会不会影响风控？**
正常的多成员协作是云平台支持的用法。真正容易触发风控的是多人在不同国家频繁切换 IP 登录同一个主账号，这恰恰是子用户能避免的。更多见 [国际云账号风控与防封清单](cloud-account-risk-control.md)。

## 账号从哪里来

分权管理的前提是你手里有一个真正属于自己的独立主账号。如果没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)，交付后自行改密、绑 MFA、创建子用户；已有账号可通过 [国际云充值](https://www.yundaxian.com/recharge) 用 USDT 按账号 ID 续费（30 USDT 起充）。

## 一句话总结

主账号锁起来不用，每个人、每个程序各用一个子用户，权限按需给、MFA 全开、密钥不进代码，有人离开就删用户删密钥。

返回 [指南首页](index.md)
