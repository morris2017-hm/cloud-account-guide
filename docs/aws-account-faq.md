---
title: AWS 账号开通与充值常见问题：Account ID、根用户安全、配额与续费
description: AWS 账号购买、新开和按 Account ID 充值时最常问的问题，包括账号标识、根用户 MFA、IAM、服务配额、账单核对和续费注意事项。
permalink: /aws-account-faq/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md)

# AWS 账号开通与充值常见问题

阿里云和腾讯云之外，AWS 是出海团队用得最多的云。AWS 的账号体系和计费方式跟前两家不太一样，这篇按问答形式整理开通和充值时最常遇到的问题。

## 开通前

**AWS 账号怎么识别？**
每个 AWS 账号都有一个 12 位数字的 Account ID，在控制台右上角的账户菜单里可以看到。充值、核对账单、排查问题都以它为准，见官方 [查看账号标识](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-identifiers.html)。

**自己注册和渠道新开有什么区别？**
自己注册需要绑定可用的付款卡并通过验证，账单直接由 AWS 收取；渠道新开的账号由渠道结算，不需要你自备卡。两种方式交付的都应是可以登录 AWS 官方控制台的独立账号，区别主要在付款关系。

**渠道新开的账号能放正式业务吗？**
可以，但接手后要像对待自己注册的账号一样做安全加固（见下文），并确认以后由谁付款、怎么续费。

**起充金额是多少？**
以服务方页面为准。云大仙公开页面写明 AWS 为 $200 起充，折扣档位以 [产品与价格](https://www.yundaxian.com/products) 和下单页实时显示为准。

## 接手后

**拿到账号第一件事做什么？**
按顺序：

1. 用根用户（root user）登录，修改密码；
2. 给根用户启用 MFA，见 [为根用户启用 MFA](https://docs.aws.amazon.com/IAM/latest/UserGuide/enable-mfa-for-root.html)；
3. 创建 IAM 用户或使用 IAM Identity Center 做日常管理，根用户只留给少数必须的操作，见 [根用户最佳实践](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)；
4. 检查目标区域的服务配额。

**为什么新账号开不了大规格实例？**
新账号的部分服务配额可能较低，开大规格或大量实例前，先在 Service Quotas 里查看当前配额，不够就提交提额申请，见 [Service Quotas 介绍](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)。

**怎么避免账单失控？**
用 AWS Budgets 设置预算和提醒，见 [用 Budgets 管理成本](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)。AWS 很多服务按用量计费，流量和存储费用容易被低估。

## 充值与续费

**已有 AWS 账号怎么充值？**
渠道结算的账号，提交 12 位 Account ID 完成校验后，按订单金额和网络付 USDT 即可原位充值，账号里的 EC2、S3 等资源不受影响。入口见 [国际云充值](https://www.yundaxian.com/recharge)，逐步说明见 [AWS 按 Account ID 充值实操](https://www.yundaxian.com/article/aws-account-top-up-guide)。

**自己绑卡的账号也能走渠道充值吗？**
不一定。先确认账号的账单付款方和结算关系，自己绑卡、由 AWS 直接收费的账号，付款应在官方账务控制台完成，见 [管理付款](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-payments.html)。

**渠道余额等于 AWS 官方余额吗？**
不能直接等同。渠道订单和 AWS 官方账单要分开核对，到账的是什么以订单约定和账单为准。

**付款前要核对什么？**
Account ID 是否正确（12 位逐位核对）、实付金额、付款网络和收款地址。USDT 网络的区别见 [USDT 充值云服务说明](usdt-cloud-top-up.md)。

**付完款要做什么？**
保存 TxID 和订单号，回到 AWS 账务控制台查看账单状态，确认资源没有因为欠费受影响。

## 其他

**Azure 和 Google Cloud 也能自助办理吗？**
这两家的订阅类型、配额和区域限制差异较大，目前不支持自助下单，需要逐单确认后办理，说明见 [产品与价格](https://www.yundaxian.com/products)。

**AWS 和阿里云、腾讯云怎么选？**
看团队熟悉哪家、目标区域和要用的服务。对比见 [阿里云国际版还是腾讯云国际版](aliyun-vs-tencent-cloud-international.md) 和 [多云选型对比](https://www.yundaxian.com/article/alibaba-cloud-vs-tencent-cloud-vs-aws)。

返回 [指南首页](index.md)
