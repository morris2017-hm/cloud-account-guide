---
title: 阿里云、腾讯云国际版安全组怎么配：SSH/RDP/HTTP/HTTPS 规则与常见坑
description: 阿里云 ECS 与腾讯云 CVM 安全组怎么配：默认拒绝入站、按最小权限开放 22/3389/80/443、控制台路径、多网卡与多安全组、常见误配，以及和账号风控的关系；适合刚完成账号购买或代开、要加固第一台机器的人。
permalink: /cloud-security-group-guide/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md)

# 阿里云、腾讯云国际版安全组怎么配

很多人刚做完阿里云账号购买或腾讯云账号购买，第一台 ECS / CVM 一开就上线，安全组却还停在「全开」或默认模板。安全组是实例级的虚拟防火墙：入站默认拒绝，出站策略因产品类型而异。配错了，要么 SSH 连不上，要么整网扫端口扫到你机器上。

本文对照两家国际版官方文档，说明安全组是什么、怎么安全地开放常用端口、控制台去哪找、多网卡与多组怎么处理，以及最常见的误配。规则会调整，文中附了官方出处，请以官方最新说明为准。

## 安全组是什么

安全组是有状态的包过滤虚拟防火墙，用来控制实例的入站和出站流量。

- **阿里云 ECS**：每个实例至少属于一个安全组；规则立即生效；安全组按地域、按 VPC 隔离，交换机与安全组必须同属一个 VPC。见 [Security group overview](https://www.alibabacloud.com/help/en/ecs/user-guide/overview-44)。
- **腾讯云 CVM**：同样是有状态过滤；无规则时默认拒绝全部流量，需要主动加允许规则；规则按从上到下优先级匹配，命中后不再往下看。见 [Security Group Overview](https://www.tencentcloud.com/document/product/213/12452)。

「有状态」的意思是：入站允许的连接，其回程出站包会自动放行（阿里云文档还写明会话最长约 910 秒），不必为回包单独写对称规则。见 [Security group rules](https://www.alibabacloud.com/help/en/ecs/user-guide/security-group-rules)。

## 默认策略：入站先拒绝

两家的默认思路都是**白名单**：入站先拒绝，只加你真正需要的允许规则。

| 对比项 | 阿里云国际版 ECS | 腾讯云国际版 CVM |
| :--- | :--- | :--- |
| 入站默认 | 基础 / 企业安全组均默认拒绝入站；基础组组内互通默认允许 | 无自定义规则时拒绝全部流量 |
| 出站默认 | 基础组默认允许全部出站；企业（高级）组默认拒绝出站 | 视模板与规则而定；「开放全部端口」模板会放开出入站 |
| 默认安全组风险 | 默认组常放行 `0.0.0.0/0` 的 22、3389 与 ICMP，官方明确提示有爆破风险 | 默认安全组规则常允许全部 IPv4，官方建议改成只开必要端口 |
| 常用模板 | 自定义规则；控制台创建时可带示例规则 | 「开放全部端口」「开放主要端口（22/80/443/3389 + ICMP）」等 |

阿里云默认安全组的风险说明见 [Default security groups](https://www.alibabacloud.com/help/en/ecs/user-guide/default-security-groups)；腾讯云创建与模板说明见 [Creating a Security Group](https://www.tencentcloud.com/document/product/213/34271)。

## 怎么安全地开 22 / 3389 / 80 / 443

官方建议按**最小权限**：远程管理口只给自己的公网 IP，Web 口才考虑对公网开放。

1. **SSH（TCP 22）**：授权对象写成你当前出口公网 IP，例如 `x.x.x.x/32`，不要长期留 `0.0.0.0/0`。阿里云用例见 [Security group guidelines and use cases](https://www.alibabacloud.com/help/en/ecs/user-guide/security-groups-for-different-use-cases)。
2. **RDP（TCP 3389）**：同样只放管理员 IP；Windows 机器尤其不要对全网开放。
3. **HTTP 80 / HTTPS 443**：面向公网的网站可以授权 `0.0.0.0/0`，但**只开这两个端口**，数据库、Redis、后台管理端口不要跟 Web 同组对公网开放。
4. **ICMP**：需要 `ping` 排查时再临时放开，排完可收紧。
5. **改端口也要改规则**：系统里把 SSH 改到 2222，安全组仍只放 22，照样连不上。

临时排障可以短暂放开再立刻改回；「先开全网、以后再说」是最常见的事故起点。

## 控制台路径与规则字段

**阿里云 ECS（国际版控制台）**

1. 登录 ECS 控制台，选对**地域**。
2. 左侧进入「网络与安全」→「安全组」，或从实例详情页进「安全组」。
3. 新建或选择安全组，添加规则。常见字段：方向（入站/出站）、协议、端口范围、授权对象（CIDR / IP / 安全组 ID / 前缀列表）、策略（允许/拒绝）、优先级（1–100，数字越小越高）。

**腾讯云 CVM（国际版控制台）**

1. 登录 CVM 控制台，左侧点「安全组」，先选对**地域**。
2. 创建时可选手动模板或自定义；创建后「设置规则」。
3. 常见字段：类型（入站/出站）、来源或目标、协议端口、策略（允许/拒绝）。多组绑定时按组优先级从上到下执行。

安全组按地域生效：新加坡的组管不了香港的机器，选错地域是「规则明明加了却不生效」的第一大原因。

## 多网卡、多安全组怎么处理

- 一台实例可以绑多个安全组；阿里云会把各组规则汇总后再匹配，同优先级时拒绝优先于允许。
- 阿里云辅助弹性网卡（ENI）可以绑与主网卡不同的安全组，适合把公网入口和内网管理拆开；同一 ENI 上的组须同类型（都是基础组或都是高级组）。
- 腾讯云官方建议：相同防护需求的实例进同一组，一台实例不要绑过多组，以免规则冲突导致断网。
- 换组时（尤其阿里云基础组），官方建议先加入新组、等约 10 秒再移出旧组，减少对已有会话的冲击。

数据库、缓存放无公网 IP 的内网组，只允许应用层安全组访问，比写一堆公网 CIDR 更稳。

## 常见误配

1. **开放全部端口 / `0.0.0.0/0` 管所有端口**：扫端口、爆破、挖矿脚本会第一时间找到你。
2. **出站被收成白名单后忘了放行**：企业安全组或自定义拒绝全部出站后，若不加允许规则，实例会连不上软件源、证书吊销列表、元数据等，表现为「装不了包、更新失败」。
3. **选错地域或 VPC**：规则加在别的地域，或组和交换机不在同一 VPC。
4. **只改了系统防火墙、没改安全组**（或反过来）：两层都要通。
5. **生产组直接在线乱改**：阿里云建议先克隆到测试环境验证再改生产。

这些问题也常和账号风控叠加：全网暴露的管理口更容易被滥用，进而触发异常登录或滥用告警，可对照 [防封清单](cloud-account-risk-control.md) 一起做完加固。

## 一套够用的第一台机器规则

1. 新建自定义安全组，不要长期依赖「开放全部端口」模板或未改过的默认组。
2. 入站只放：你的 IP → 22 或 3389；`0.0.0.0/0` → 80 与 443（若确有公网 Web）。
3. 数据库、面板、Redis 端口不对公网开放。
4. 出口 IP 会变时，用堡垒机、Workbench / Session Manager，或及时更新 `/32` 规则。
5. 改完规则后从外网测一次 SSH/HTTPS，并确认不相关端口已关闭。

## 账号从哪里来

安全组再严，也要先有一个真正属于自己、能续费的国际版账号。没有合适国际信用卡的团队，可通过 [云大仙](https://www.yundaxian.com/) 办理 [阿里云账号购买与阿里云代开](https://www.yundaxian.com/open/aliyun) 或 [腾讯云账号购买与腾讯云代开](https://www.yundaxian.com/open/tencent)，已有账号可在 [国际云充值](https://www.yundaxian.com/recharge) 用 USDT 续费，最低 30 USDT 起充。Azure 和 Google Cloud 的充值不支持网站自助办理，产品说明见 [产品与价格](https://www.yundaxian.com/products)。

## 一句话总结

安全组默认拒绝入站。管理口只放你的 IP，Web 只开 80/443，数据库不对公网；选对地域，少用「全开」模板，改完立刻自测。

返回 [指南首页](index.md)
