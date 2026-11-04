---
title: 腾讯云代开流程与验收：注册地确认、付款、交付加固与续费
description: 腾讯云代开从准备到上线的完整步骤，附交付验收表和账号加固清单，以及续费时容易忽略的问题。
permalink: /tencent-cloud-agency-opening/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md)

# 腾讯云代开流程与验收

腾讯云代开和阿里云代开的骨架一样：准备、下单、付款、验收。不同的是，腾讯云多了一个必须在**下单前**定下来的东西：注册地。

## 第一步：准备

- 一个从未在 tencentcloud.com 注册过的邮箱，它会用于登录保护和账号通知；
- **和服务方确认注册地**：企业账号应选企业主体所在地，个人账号应选本人所在地，注册后不可更改；
- 想好产品和地域（Lighthouse / CVM / COS / CDN，中国香港、新加坡、东京、硅谷等）；
- 确认你的 USDT 网络（TRC20 / ERC20 / BEP20）。

## 第二步：下单与付款

1. 在服务方页面选择腾讯云国际新开 / 代开，云大仙的入口见 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)（公开起充 $30）；
2. 在结算页核对实付金额、入账面值、付款网络；
3. 按订单网络和地址转账，ERC20 与 BEP20 都是 `0x` 地址，转账前再核对一次；
4. 保存 TxID 和订单号。

## 第三步：交付验收

| 验收项 | 通过标准 |
| :--- | :--- |
| 登录 | 能登录 tencentcloud.com 官方控制台 |
| 账号身份 | 是主账号而不是 CAM 子用户，记下账号 ID |
| 注册地 | 与下单时确认的一致 |
| 余额 | 费用中心可用余额与订单入账一致 |
| 可用性 | 目标地域能创建一台最小规格实例 |

## 第四步：加固

- 改成只有你知道的强密码，换绑你自己的邮箱；
- 绑定虚拟 MFA，开启登录保护与操作保护；
- 日常运维使用 CAM 子用户，按最小权限授权，子用户也可以开启安全保护；
- 设置余额告警。

逐项操作说明见 [腾讯云代开全流程](https://www.yundaxian.com/article/tencent-cloud-agency-opening-process-and-acceptance)。

## 关于身份验证

只用海外地域时，是否需要身份验证以控制台实际要求为准；要用中国内地地域、申请合作伙伴或在线开票，就必须完成验证。验证完成后不能撤销，两次修改之间要间隔 30 天，审核一般需要几个工作日（[身份验证 FAQ](https://www.tencentcloud.com/document/product/378/3629)）。

## 续费

提交账号 ID 或注册邮箱即可原位充值，不用迁移服务器，入口见 [国际云充值](https://www.yundaxian.com/recharge)。充值后单独检查包年包月资源的续费状态，余额充足不等于已经续费。

## 常见疑问

- **代开账号能自己绑信用卡吗？** 代开账号走渠道结算，付款设置不要自行改动，先咨询服务方。
- **交付后服务方还能登录吗？** 改密、换绑邮箱、绑定 MFA、开启登录保护之后，只有你能登录。

下一篇：[USDT 充值云服务说明](usdt-cloud-top-up.md)
