# 国际云账号购买、代开与 USDT 充值指南

> 一份面向出海团队和个人开发者的中文实操手册，覆盖阿里云国际版、腾讯云国际版的账号购买与代开流程，以及 AWS、Azure、Google Cloud 的开户与充值要点。

本仓库整理的是「怎么判断、怎么核对、怎么接手」这类经验，不替任何云厂商报价。凡涉及官方规则的地方都附了官方文档，规则以官方最新说明和控制台显示为准。示例中的服务方以 [云大仙](https://www.yundaxian.com/) 为例，你可以把同样的核对方法用在任何渠道上。

在线阅读版（GitHub Pages）：见 [`docs/`](docs/) 目录，启用方式见 [发布 GitHub Pages](#发布-github-pages)。

## 目录

- [1. 先分清：国际版和国内版不是一个账号](#1-先分清国际版和国内版不是一个账号)
- [2. 账号从哪里来：三种来源的取舍](#2-账号从哪里来三种来源的取舍)
- [3. 阿里云账号购买与阿里云代开](#3-阿里云账号购买与阿里云代开)
- [4. 腾讯云账号购买与腾讯云代开](#4-腾讯云账号购买与腾讯云代开)
- [5. AWS、Azure、Google Cloud 怎么办](#5-awsazuregoogle-cloud-怎么办)
- [6. 用 USDT 付款与续费](#6-用-usdt-付款与续费)
- [7. 账号到手后的安全清单](#7-账号到手后的安全清单)
- [8. 常见问题](#8-常见问题)
- [9. 延伸阅读](#9-延伸阅读)
- [发布 GitHub Pages](#发布-github-pages)
- [许可与免责声明](#许可与免责声明)

## 1. 先分清：国际版和国内版不是一个账号

很多「买错号」的问题，根源是没分清站点。阿里云和腾讯云都各有两套完全独立的账号体系：

| 对比项 | 阿里云中国站 | 阿里云国际版 | 腾讯云中国站 | 腾讯云国际版 |
| :--- | :--- | :--- | :--- | :--- |
| 官网 / 控制台 | aliyun.com | alibabacloud.com | cloud.tencent.com | tencentcloud.com |
| 计价币种 | 人民币 | 美元 | 人民币 | 美元 |
| 常用地域 | 中国内地为主 | 中国香港、新加坡、日本、美国、欧洲等 | 中国内地为主 | 中国香港、新加坡、东京、硅谷等 |
| 网站备案 | 内地地域对外提供网站需 ICP 备案 | 香港及海外地域一般不涉及 | 内地地域对外提供网站需 ICP 备案 | 香港及海外地域一般不涉及 |
| 官方注册付款 | 国内支付渠道 | 一般需绑定支持的国际卡；渠道账号走渠道结算 | 国内支付渠道 | 需绑卡或信用授权；渠道账号走渠道结算 |
| 账号互通 | 与国际版不互通 | 与中国站不互通 | 与国际版不互通 | 与中国站不互通 |

几条容易踩坑的规则：

- 两个站点的余额、订单、发票、工单各算各的，中国站账号不能「升级」成国际版账号。
- 阿里云国际版购买中国内地地域资源前，需要先完成身份验证，见 [Account verification overview](https://www.alibabacloud.com/help/en/account/account-verification-overview)。
- 腾讯云国际版的**注册地一旦选定就不能修改**，它关系到税率、手机区号和后续身份验证，见 [注册说明](https://www.tencentcloud.com/document/product/378/17985)。
- 阿里云国际版官方付款 FAQ 写明，不接受中国内地发行的信用卡、预付卡、虚拟卡等付款方式，见 [Payment FAQ](https://www.alibabacloud.com/help/en/user-center/support/payment-faq)。

结论：用户在海外、做跨境或出海业务、不想等备案，选国际版；用户在内地且能备案，选中国站。两边都有业务，就分别开户。更细的对比可参考 [阿里云国际版和中国站的区别](https://www.yundaxian.com/article/aliyun-international-vs-china-site-differences)。

## 2. 账号从哪里来：三种来源的取舍

拿到一个国际版账号，一般只有三条路：

| 维度 | 自己注册 + 绑卡 | 来路不明的成品号 / 共享子账号 | 渠道代开独立主账号 |
| :--- | :--- | :--- | :--- |
| 门槛 | 需要能通过验证的国际信用卡、手机号 | 几乎没有 | 不需要自备国际卡，用渠道结算 |
| 账号归属 | 完全属于你 | 注册信息属于别人，可能被找回或收回 | 交付后由你改密、绑 MFA |
| 风控隐患 | 卡被拒、信息不一致可能触发审核 | 批量注册、卡段关联，被封概率高 | 避开个人卡拒付这一常见诱因，但仍须遵守使用政策 |
| 续费 | 依赖卡片有效和额度 | 往往无法正规续费，只能换号迁机 | 可按账号 ID 原位充值 |
| 适合 | 有可用国际卡的团队 | 不建议用于任何正式业务 | 没有合适国际卡、需要尽快上线的团队 |

判断标准其实只有一条：**这个账号出了事，你能不能证明它是你的，能不能把它找回来。** 成品号在这一点上天然不合格。

无论选哪个服务方，付款前都问清楚这六件事，有一项答不上来就先别付：

1. 交付的是独立主账号，还是 RAM / CAM 子用户、共享账号？
2. 能不能直接登录官方控制台（alibabacloud.com / tencentcloud.com）？
3. 交付后能否自行改密码、换绑邮箱、绑定 MFA？
4. 注册地是哪里，是否涉及身份验证？
5. 实付多少、入账多少，入账的是现金余额还是渠道额度？
6. 以后怎么续费，到账有差异时找谁处理？

## 3. 阿里云账号购买与阿里云代开

「阿里云账号购买」在正规语境下，指的是通过渠道新开一个阿里云国际版独立主账号，而不是从别人手里接一个旧号。「阿里云代开」解决的核心问题是付款：用渠道结算替代个人绑卡。

一次完整的代开大致分四步：

1. **准备**：一个从未在 alibabacloud.com 注册过、你能长期控制的邮箱；想清楚要用的产品（ECS、轻量应用服务器、OSS、CDN）和地域。
2. **下单**：在服务方页面选择阿里云国际新开 / 代开，确认起充金额、实付金额与入账面值。云大仙公开页面写明阿里云国际为 $30 起充，具体折算以下单页为准，入口见 [阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun)。
3. **付款**：按订单显示的网络和地址转 USDT，保存交易哈希（TxID）和订单号。
4. **验收**：登录 `account.alibabacloud.com`，核对 16 位账号 ID（UID）、主账号身份、余额，然后立刻做第 7 节的安全加固。

验收时重点确认是不是主账号：主账号可以自由创建和删除 RAM 用户、修改自身密码和 MFA；如果你登录后发现权限受限，或者只能从 RAM 用户入口登录，说明拿到的不是独立主账号。

延伸阅读：

- [阿里云账号购买避坑指南](https://www.yundaxian.com/article/aliyun-account-purchase-pitfalls-guide)：成品号、共享子号和代开的区别
- [阿里云代开全流程拆解](https://www.yundaxian.com/article/aliyun-agency-opening-process-and-verification)：逐步验收清单
- [阿里云代开还是自己注册](https://www.yundaxian.com/article/aliyun-agency-opening-vs-self-registration)：两种方式的时间和门槛对比
- [阿里云国际账号购买价格怎么算](https://www.yundaxian.com/article/aliyun-international-account-price-and-cost-breakdown)：起充、月费与链上手续费怎么拆
- [阿里云国际站地域怎么选](https://www.yundaxian.com/article/aliyun-international-region-selection-guide)

## 4. 腾讯云账号购买与腾讯云代开

腾讯云国际版的官方规则比阿里云更「硬」，有三条要先记住：

- 账号需要完成银行卡绑定或信用授权后才能购买资源（见 [账号概述](https://www.tencentcloud.com/document/product/378/80429)）。这是自己注册最常卡住的地方。
- 注册地一经选择不可更改。
- 身份验证完成后不能撤销；未完成身份验证的账号不能购买或使用中国内地地域的服务（见 [身份验证 FAQ](https://www.tencentcloud.com/document/product/378/3629)）。

所以「腾讯云账号购买」时最要紧的是确认注册地和账号归属：从别人手里接过的成品号，注册地、手机号、身份验证全是别人的，原注册人可以通过官方找回工具把账号拿回去。

「腾讯云代开」的流程和阿里云基本一致：准备干净邮箱，**先和服务方确认注册地**，下单核对实付与入账（云大仙公开页面为 $30 起充），按订单网络转 USDT 并保存 TxID，交付后登录 tencentcloud.com 核对账号 ID、注册地和余额。办理条件见 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。

交付后加固：改密码、按 [绑定虚拟 MFA 设备](https://www.tencentcloud.com/document/product/378/32528) 绑定验证器、开启登录保护和操作保护、日常运维改用 CAM 子用户。

延伸阅读：

- [腾讯云账号购买避坑指南](https://www.yundaxian.com/article/tencent-cloud-account-purchase-pitfalls-guide)
- [腾讯云代开全流程：下单、付款、验收到续费](https://www.yundaxian.com/article/tencent-cloud-agency-opening-process-and-acceptance)
- [腾讯云国际版注册教程](https://www.yundaxian.com/article/tencent-cloud-international-registration-guide)：自己注册时的注册地、手机号与绑卡要求
- [Lighthouse 和 CVM 怎么选](https://www.yundaxian.com/article/tencent-cloud-lighthouse-vs-cvm-guide)

## 5. AWS、Azure、Google Cloud 怎么办

- **AWS**：账号标识是 12 位 Account ID。续费前先确认账单付款方和你的账务权限；渠道余额或额度不等于 AWS 官方账户余额，验收时要分开核对。云大仙公开页面写明 AWS 为 $200 起充、可按 Account ID 原位充值，参考 [AWS 按 Account ID 充值实操](https://www.yundaxian.com/article/aws-account-top-up-guide)。
- **Azure / Google Cloud**：订阅类型、配额、区域限制差异大，不适合「一键下单」，需人工确认后办理。云大仙公开页面为 $200 起，见 [产品与价格](https://www.yundaxian.com/products)。
- 多云怎么选：参考 [阿里云国际 vs 腾讯云国际 vs AWS vs GCP/Azure 选型](https://www.yundaxian.com/article/alibaba-cloud-vs-tencent-cloud-vs-aws)。

## 6. 用 USDT 付款与续费

USDT 付款最常见的事故不是被骗，而是**选错网络**和**少付手续费**。

| 网络 | 所在链 | 地址特征 | 注意 |
| :--- | :--- | :--- | :--- |
| TRC20 | TRON | 以 `T` 开头 | 普及率高，网络费一般较低 |
| ERC20 | Ethereum | 以 `0x` 开头 | 网络费相对高 |
| BEP20 | BNB Smart Chain | 以 `0x` 开头 | 与 ERC20 地址格式相同，最容易选错 |

付款三原则：

1. 付款网络必须和订单显示的网络一致，ERC20 与 BEP20 地址长得一样，转账前逐字核对。
2. 从交易所提币时，提币手续费可能从金额里扣，要保证**到账金额**等于订单金额。
3. 保存 TxID、订单号和付款截图，到账有差异时这是唯一凭据。

续费时不需要重新开号：提交平台账号 ID（阿里云 UID、腾讯云账号 ID、AWS Account ID）即可原位充值，服务器和 IP 不变。但要记住，**余额充足不等于包年包月资源已经续费**，到期前请在控制台单独检查续费状态。云大仙的续费入口与核对清单见 [国际云充值](https://www.yundaxian.com/recharge)，链上对账细节见 [USDT 充值续费实操](https://www.yundaxian.com/article/usdt-trc20-erc20-cloud-billing-settlement-guide)。

## 7. 账号到手后的安全清单

不管账号从哪里来，接手后的第一个小时做完这些：

- [ ] 登录官方控制台，记下账号 ID，确认是主账号
- [ ] 修改登录密码，换绑成你自己长期控制的邮箱
- [ ] 绑定虚拟 MFA（Google Authenticator / Microsoft Authenticator 等）
- [ ] 开启登录保护、操作保护（腾讯云）或安全设置中的相关选项（阿里云）
- [ ] 创建 RAM / CAM 子用户，按最小权限授权，主账号只留给负责人
- [ ] 核对余额与订单入账是否一致，设置余额提醒
- [ ] 在目标地域开一台最小规格实例做连通性测试，再部署正式业务
- [ ] 避免多人在不同国家频繁切换 IP 登录主账号

账号被限制或封停时的处理思路，可参考 [阿里云国际账号防封清单](https://www.yundaxian.com/article/aliyun-international-risk-control-and-safe-takeover) 和 [被封或被限制怎么办](https://www.yundaxian.com/article/aliyun-international-account-suspended-appeal-guide)。

## 8. 常见问题

**阿里云国际版和阿里云国际站是一回事吗？**
是，都指 alibabacloud.com。

**没有海外信用卡能用国际版吗？**
可以考虑渠道代开，用渠道结算替代个人绑卡，参考 [没有海外信用卡怎么购买阿里云国际站服务器](https://www.yundaxian.com/article/buy-alibaba-cloud-without-credit-card-usdt)。有可用的国际卡，自己注册是最直接的方式。

**代开账号和官方注册的账号有什么区别？**
登录入口相同，正规代开交付的也是独立主账号，区别主要在结算关系：代开账号走渠道结算，付款设置不要自行改动，有疑问先问服务方。

**起充金额是开户费吗？**
以服务方说明为准。云大仙公开说明为：起充金额进入账号用于消费云资源，不另收空号开户费；实付与入账的折算以下单页为准。

**国际版香港服务器需要备案吗？**
中国香港及海外地域一般不涉及 ICP 备案。

**渠道余额等于官方现金余额吗？**
不能直接等同。到账的是现金、额度还是其他权益，以订单约定和账单中心为准。

**价格和折扣在哪里看？**
本仓库不写具体价格，因为折扣和汇率会调整。请以服务方实时页面为准，例如 [云大仙产品与价格](https://www.yundaxian.com/products)。

## 9. 延伸阅读

本仓库的专题指南（GitHub Pages 在线版同步）：

- [阿里云国际版充值方式对比](docs/aliyun-international-top-up.md)：官方付款、渠道充值与 USDT 续费怎么选
- [阿里云国际版还是腾讯云国际版](docs/aliyun-vs-tencent-cloud-international.md)：账号购买与代开前怎么选平台
- [国际云账号风控与防封清单](docs/cloud-account-risk-control.md)：接手后的加固步骤与日常禁忌
- [AWS 账号开通与充值常见问题](docs/aws-account-faq.md)：Account ID、根用户安全、配额与续费
- [阿里云国际版 CDT 每月 220GB 免费流量](docs/aliyun-international-cdt-free-traffic.md)：免费额度规则、阶梯价格、开通与告警设置
- [阿里云、腾讯云国际版地域怎么选](docs/cloud-region-selection-guide.md)：按用户分布、备案、线路、成本和合规选地域，附跨地域迁移方法
- [阿里云 RAM、腾讯云 CAM 子用户怎么设](docs/ram-cam-subuser-permission-guide.md)：主账号保护、最小权限、MFA、AccessKey 管理与离职交接
- [阿里云、腾讯云国际版欠费停机怎么办](docs/cloud-overdue-payment-suspension-guide.md)：宽限期、保留期、释放规则、充值恢复步骤与余额预警
- [阿里云、腾讯云国际版账单与用量怎么看](docs/cloud-international-billing-usage-guide.md)：月账单与明细出账时间、控制台入口、实例级用量与费用突增排查
- [阿里云、腾讯云国际版快照与备份怎么做](docs/cloud-snapshot-backup-guide.md)：增量原理、自动快照策略、保留期、跨地域复制与计费要点
- [阿里云、腾讯云国际版安全组怎么配](docs/cloud-security-group-guide.md)：默认拒绝入站、按最小权限开放 SSH/RDP/HTTP/HTTPS、多安全组与常见误配
- [阿里云国际版账号身份验证怎么做](docs/aliyun-international-account-verification.md)：何时必须验证、护照与企业证件、审核时间，以及验证后不能自行解绑
- [轻量应用服务器还是云服务器](docs/lightweight-server-vs-ecs-cvm-guide.md)：阿里云轻量与 ECS、腾讯云 Lighthouse 与 CVM 的套餐差异、流量超额、磁盘快照限制和迁到 ECS 的边界
- [腾讯云国际版身份验证怎么做](docs/tencent-cloud-international-identity-verification.md)：注册地与证件必须一致、个人最多 3 个主账号、企业默认 10 个，通过后不能解除，满 30 天才能变更主体
- [包年包月、按量付费还是抢占式](docs/cloud-billing-mode-stop-charging-guide.md)：ECS/CVM 三种计费方式怎么选，节省停机和关机不收费会停掉哪些费用，云盘与公网 IP 为什么还在出账
- [系统盘和数据盘怎么选](docs/cloud-disk-system-data-guide.md)：云盘类型的实际 IOPS 怎么按容量折算，扩容不能缩小，以及释放实例后云盘是否继续计费
- [公网 IP 和弹性公网 IP 怎么选](docs/cloud-public-ip-eip-bandwidth-guide.md)：普通公网 IP 与 EIP 的生命周期、按流量和按带宽的 5 Mbps 阶梯，以及未绑定 EIP 的地址占用费
- [重装系统会清掉什么](docs/cloud-reinstall-os-guide.md)：更换系统盘和操作系统迁移的差别，系统盘清空后快照不能回滚到新盘，香港及海外地域不能在 Linux 和 Windows 之间互换
- [升降配会停机吗](docs/cloud-instance-type-change-guide.md)：同可用区变更实例规格要重启，系统和云盘数据留下；阿里云跨可用区会更换私网 IP，固定公网 IP 只是尽量保留，降配退款以订单页为准
- [私钥丢了还能登录吗](docs/cloud-ssh-keypair-password-vnc-guide.md)：ECS/CVM 的私钥只在创建时给出，绑定密钥对后 SSH 密码登录会关掉；忘了密码要在控制台重置，VNC 用系统密码，不看私钥，也不走安全组
- [自动续费哪天扣钱](docs/cloud-subscription-auto-renewal-guide.md)：阿里云国际站在到期前 3 天开始尝试扣款，成功即停；腾讯云在到期日当天扣，当天余额不够则在停服前按天再看。打开开关不会当场续费，手动续过的那一轮也不会再自动扣一笔

云大仙相关页面：

- 云大仙文章列表：<https://www.yundaxian.com/blog>
- 阿里云国际开户与代开：<https://www.yundaxian.com/open/aliyun>
- 腾讯云国际开户与代开：<https://www.yundaxian.com/open/tencent>
- 已有账号充值：<https://www.yundaxian.com/recharge>
- 阿里云国际版官方文档：<https://www.alibabacloud.com/help/en/>
- 腾讯云国际版官方文档：<https://www.tencentcloud.com/document/product>

## 发布 GitHub Pages

`docs/` 是一个零依赖的 Jekyll 站点，由 GitHub Pages 在线构建，本地不需要安装 Ruby 或任何构建工具。

1. 推送本仓库到 GitHub（公开仓库）。
2. 打开仓库 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 `main`，目录选 `/docs`，保存。
3. 编辑 [`docs/_config.yml`](docs/_config.yml)：
   - `url` 改成 `https://<你的 GitHub 用户名>.github.io`
   - `baseurl` 改成 `/<仓库名>`（例如 `/cloud-account-guide`）；如果仓库名是 `<用户名>.github.io` 或绑定了自定义域名，改成 `""`
4. 等一两分钟，访问 `https://<用户名>.github.io/<仓库名>/`。站点地图在 `sitemap.xml`，robots 在 `robots.txt`，均自动生成。

## 许可与免责声明

本仓库文字内容采用 [CC BY 4.0](LICENSE) 许可，转载请注明出处并附原链接。

本仓库仅为经验整理，不构成任何云厂商的官方说明或报价。云厂商规则、价格和可用地域随时可能调整，请以官方文档和控制台为准。使用任何云服务都须遵守其服务条款与当地法律法规。
