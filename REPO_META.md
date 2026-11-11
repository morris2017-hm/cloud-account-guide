# 仓库元信息建议（发布时填写到 GitHub，不影响内容）

## 仓库名
- 首选：`cloud-account-guide`
- 备选：`intl-cloud-account-guide`、`aliyun-tencent-cloud-guide`

## Description（About 栏，建议 ≤ 120 字）
阿里云 / 腾讯云国际版账号购买、代开与 USDT 充值中文指南：站点区别、来源对比、验收与安全清单，兼顾 AWS、Azure、GCP。

## Website（About 栏）
https://www.yundaxian.com/

## Topics
alibaba-cloud, aliyun, tencent-cloud, cloud-account, alibaba-cloud-international, tencent-cloud-international, aws, azure, google-cloud, usdt, cloud-computing, devops, chinese, guide, awesome-cloud

## GitHub Pages
Settings → Pages → Deploy from a branch → `main` / `/docs`，然后按 README「发布 GitHub Pages」修改 `docs/_config.yml` 中的 `url` 与 `baseurl`。

注：项目站点（`<用户名>.github.io/<仓库名>/`）下的 robots.txt 不在域名根目录，搜索引擎不会读取它；站点地图 `sitemap.xml` 仍然可以被抓取。若绑定自定义子域名（例如 guide.example.com），robots.txt 与 sitemap 都会在根目录生效，届时把 `baseurl` 设为 ""。

## 发文记录

- 2026-11-11：新增《按量能转包年包月吗：公网跟不跟，腾讯云转不回去》`docs/cloud-billing-method-switch-guide.md`。既有文章写过三种计费方式怎么选、关机后还扣哪些费，以及腾讯云「按量可以转包年包月、包年包月可以转包销」这一句。没有写阿里云互转时系统盘、数据盘、固定公网 IP 和 EIP 分别跟着谁走，也没有写转到按量的每月退款额度，以及已用不满 30 天已消耗按 1.5 倍计。腾讯云那边没有写付清才生效、新周期从转换成功时起算，也没有把接口示例里的 `POSTPAID_BY_HOUR` 和参数说明、计费模式页对上。本文按国际站转换说明、退款规则和 CVM 计费模式页整理。
- 2026-11-11：新增《自动续费哪天扣钱：开了不会马上续上》`docs/cloud-subscription-auto-renewal-guide.md`。既有文章写过「余额不等于已经续费」、腾讯云自动续费扣款失败后的停服窗口，以及包年包月到期日按自然月计算。没有写阿里云国际站的五个扣款时点、设置次日生效、手动续费后不再叠加自动扣款，也没有写腾讯云到期日当天扣款后的按天重试、CVM 自动续费按月一次，以及「到期不续费」只减少提醒。本文按国际站续费管理、ECS 自动续费和 CVM 续费标记接口整理扣款日、时长和两套文档不一致时以续费页为准的核对方法。
- 2026-11-11：新增《私钥丢了还能登录吗：密钥对、重置密码，以及 VNC》`docs/cloud-ssh-keypair-password-vnc-guide.md`。既有文章写过安全组放行 22/3389、重装时要重设登录方式，以及账号 MFA 和 AccessKey。没有写实例 SSH 私钥只在创建时给出、绑定后密码登录会关掉、解绑不生成新密码，也没有写 VNC 用系统密码而且不走安全组。本文按国际站登录凭证、密钥对接口和 VNC 说明整理地域限制、用户名差异，以及重置密码时要不要重启。
- 2026-11-10：新增《升降配会停机吗：同可用区改规格、跨可用区，以及降配退不退钱》`docs/cloud-instance-type-change-guide.md`。既有文章写过抢占式不能升降配、轻量套餐只能升或可迁到 ECS，以及换地域要用自定义镜像。没有写 ECS/CVM 变更规格必须重启、vCPU 和内存不能分开改、腾讯云包年包月最多降 5 次，也没有把阿里云同可用区变更和跨可用区迁移的 IP 规则分开。本文按国际站变更规格、跨可用区迁移和降配退款说明整理停机、库存、磁盘与 NVMe 前置条件，以及两套退款公式不一致时以订单页为准。
- 2026-11-10：新增《重装系统会清掉什么：更换操作系统、系统盘快照，以及香港能不能 Linux 换 Windows》`docs/cloud-reinstall-os-guide.md`。既有文章写过数据盘用来躲重装、重装不能把系统盘缩小、腾讯云关机不收费期间不能重装，以及用自定义镜像换地域。没有分开写阿里云「更换系统盘」和「操作系统迁移」，也没有写两边在中国内地以外不能 Linux 与 Windows 互转，以及原系统盘快照不能回滚到新盘。本文按国际站更换系统盘、操作系统迁移和 CVM 重装说明整理数据边界、地域限制，以及重装后的挂载与登录。
- 2026-11-10：新增《公网 IP 和弹性公网 IP 怎么选：按流量还是按带宽，闲置还扣不扣费》`docs/cloud-public-ip-eip-bandwidth-guide.md`。既有文章写过 CDT 免费额度、节省停机时系统公网 IP 会被释放、轻量公网 IP 不能更换，以及释放实例后空闲 EIP 可能继续计费。没有比较普通公网 IP 和 EIP 的生命周期，也没有写按流量峰值只是上限、按带宽超过 5 Mbps 后的阶梯单价，以及地址占用费在什么条件下免除。本文按国际站 EIP 与公网计费文档整理选型、闲置扣费，以及 EIP 欠费释放和实例不是同一套时钟。
- 2026-11-10：新增《系统盘和数据盘怎么选：云盘类型、实际 IOPS 与释放后还扣不扣费》`docs/cloud-disk-system-data-guide.md`。既有文章写过快照、轻量套餐的磁盘限制，以及关机后云盘继续计费，没有比较 ECS/CVM 的系统盘和数据盘，也没有写云盘性能如何随容量变化、释放实例时云盘是否一起释放。本文按国际站块存储文档整理选型、扩容不能缩小，以及两边释放规则不一致时以控制台为准的核对方法。
- 2026-11-10：新增《包年包月、按量付费还是抢占式：关机之后还扣哪些费》`docs/cloud-billing-mode-stop-charging-guide.md`。既有文章写过按量欠费时间线、账单入口、CDT 流量和轻量套餐，没有比较 ECS/CVM 的包年包月、按量付费与抢占式，也没有写节省停机和关机不收费实际停掉哪些费用。本文按国际站计费与停机文档整理选型、继续出账的云盘和公网 IP，以及两边文档不一致时以控制台和账单为准的核对方法。
- 2026-11-09：新增《阿里云国际版账号身份验证怎么做：个人、企业与内地资源》`docs/aliyun-international-account-verification.md`。既有文章只在购买中国内地地域时一笔带过验证要求，本文专写国际站个人/企业验证的适用场景、证件、审核与不可撤销规则。
- 2026-11-09：新增《轻量应用服务器还是云服务器：阿里云 ECS、腾讯云 CVM 怎么选》`docs/lightweight-server-vs-ecs-cvm-guide.md`。既有文章只在购买流程里提到轻量和 Lighthouse 的名字，并链到站外选型文；本文按国际站官方对比、限制和计费说明，写套餐与云服务器的适用场景、流量超额、磁盘与快照配额，以及阿里云轻量迁到 ECS 的边界。
- 2026-11-09：新增《腾讯云国际版身份验证怎么做：注册地、证件与变更限制》`docs/tencent-cloud-international-identity-verification.md`。既有购买和代开文章只写了内地资源要验证、验证不可撤销、变更间隔 30 天。本文按国际站账号中心现行说明，区分绑卡与身份验证，写个人/企业证件、3 个与 10 个名额、提交与审核，以及不能解除但可在满 30 天后变更的操作路径。
