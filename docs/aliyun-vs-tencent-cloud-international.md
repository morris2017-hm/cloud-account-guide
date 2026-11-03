---
title: 阿里云国际版还是腾讯云国际版：账号购买与代开前怎么选
description: 在阿里云账号购买和腾讯云账号购买之间犹豫时，从产品、地域、注册规则、代开流程和续费方式几个角度对比，帮你先选对平台再下单。
permalink: /aliyun-vs-tencent-cloud-international/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md)

# 阿里云国际版还是腾讯云国际版

准备出海部署时，很多人会同时搜「阿里云账号购买」和「腾讯云账号购买」，然后卡在二选一。两家都是成熟的国际云，没有绝对的好坏，关键看你的业务放在哪、用什么产品、谁来维护。这篇按下单前真正需要考虑的几件事做对比。

## 一张表看主要差别

| 对比项 | 阿里云国际版 | 腾讯云国际版 |
| :--- | :--- | :--- |
| 官网 / 控制台 | alibabacloud.com | tencentcloud.com |
| 常用入门产品 | ECS、轻量应用服务器、OSS、CDN | CVM、Lighthouse 轻量应用服务器、COS、CDN |
| 常用地域 | 中国香港、新加坡、日本、美国、欧洲等 | 中国香港、新加坡、东京、首尔、硅谷等 |
| 注册时的硬规则 | 购买中国内地地域资源前需完成身份验证 | 注册地选定后不可更改，买资源前需绑卡或信用授权 |
| 子账号体系 | RAM | CAM |
| 账号标识 | 16 位 UID | 账号 ID |
| 与中国站关系 | 与 aliyun.com 不互通 | 与 cloud.tencent.com 不互通 |

具体可用产品和地域以控制台实际显示为准。

## 按场景怎么选

- **团队已经熟悉阿里云中国站**：优先选阿里云国际版，控制台和产品命名相近，迁移和运维成本最低。
- **团队熟悉腾讯云中国站，或者业务和腾讯生态关联多**：优先选腾讯云国际版。
- **只要一台小机器建站、做跳板**：两家的轻量应用服务器都够用，先看目标地域有没有你要的机房，再比较线路。
- **要做多地域、对可用性要求高**：两家可以都开，各放一部分业务，互为备份。两个账号分别开户、分别续费，互不影响。

地域细节可以参考 [阿里云国际站地域怎么选](https://www.yundaxian.com/article/aliyun-international-region-selection-guide)，产品选择可以参考 [Lighthouse 和 CVM 怎么选](https://www.yundaxian.com/article/tencent-cloud-lighthouse-vs-cvm-guide) 和 [轻量应用服务器和 ECS 怎么选](https://www.yundaxian.com/article/aliyun-international-simple-application-server-vs-ecs)。

## 代开时的差别

阿里云代开和腾讯云代开的骨架一样：准备干净邮箱、下单核对金额、按订单网络付 USDT、登录官方控制台验收。差别主要有两点：

1. **腾讯云要在下单前定好注册地。** 注册地一经选择不能修改，它关系到税率和后续身份验证，见腾讯云 [注册说明](https://www.tencentcloud.com/document/product/378/17985)。阿里云没有这一步，但要用中国内地地域就要先做身份验证，见 [Account verification overview](https://www.alibabacloud.com/help/en/account/account-verification-overview)。
2. **验收时看的标识不同。** 阿里云核对 16 位 UID 和主账号权限，腾讯云核对账号 ID、注册地和余额。

两家的办理入口：[阿里云账号购买与代开](https://www.yundaxian.com/open/aliyun) · [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。完整流程见本站的 [阿里云代开流程与验收](aliyun-agency-opening.md) 和 [腾讯云代开流程与验收](tencent-cloud-agency-opening.md)。

## 续费方式

两家都可以按账号标识原位充值：阿里云提交 UID 或注册邮箱，腾讯云提交账号 ID 或注册邮箱，服务器和 IP 都不变，入口统一在 [国际云充值](https://www.yundaxian.com/recharge)。

两家共同的提醒：余额充足不等于包年包月资源已经续费，到期前要单独检查续费状态。

## 价格怎么比

两家的折扣档位和起充金额都会调整，本文不写具体数字。比较价格时建议：

- 按同一地域、同一规格、同一计费方式比较实例价格；
- 渠道费用分开看：实付金额、入账面值、折扣档位；
- 以 [产品与价格](https://www.yundaxian.com/products) 页面和下单页实时显示为准。

还想把 AWS、Google Cloud、Azure 一起比较，可以看 [多云选型对比](https://www.yundaxian.com/article/alibaba-cloud-vs-tencent-cloud-vs-aws)。

## 一句话结论

先按团队熟悉度和目标地域选平台，再走代开流程。选不出来时，先在其中一家开一个小账号，在目标地域开一台最小规格实例实测线路，比看任何对比表都准。

下一篇：[国际云账号风控与防封清单](cloud-account-risk-control.md)
