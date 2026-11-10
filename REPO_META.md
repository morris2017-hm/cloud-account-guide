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

- 2026-11-10：新增《系统盘和数据盘怎么选：云盘类型、实际 IOPS 与释放后还扣不扣费》`docs/cloud-disk-system-data-guide.md`。既有文章写过快照、轻量套餐的磁盘限制，以及关机后云盘继续计费，没有比较 ECS/CVM 的系统盘和数据盘，也没有写云盘性能如何随容量变化、释放实例时云盘是否一起释放。本文按国际站块存储文档整理选型、扩容不能缩小，以及两边释放规则不一致时以控制台为准的核对方法。
- 2026-11-10：新增《包年包月、按量付费还是抢占式：关机之后还扣哪些费》`docs/cloud-billing-mode-stop-charging-guide.md`。既有文章写过按量欠费时间线、账单入口、CDT 流量和轻量套餐，没有比较 ECS/CVM 的包年包月、按量付费与抢占式，也没有写节省停机和关机不收费实际停掉哪些费用。本文按国际站计费与停机文档整理选型、继续出账的云盘和公网 IP，以及两边文档不一致时以控制台和账单为准的核对方法。
- 2026-11-09：新增《阿里云国际版账号身份验证怎么做：个人、企业与内地资源》`docs/aliyun-international-account-verification.md`。既有文章只在购买中国内地地域时一笔带过验证要求，本文专写国际站个人/企业验证的适用场景、证件、审核与不可撤销规则。
- 2026-11-09：新增《轻量应用服务器还是云服务器：阿里云 ECS、腾讯云 CVM 怎么选》`docs/lightweight-server-vs-ecs-cvm-guide.md`。既有文章只在购买流程里提到轻量和 Lighthouse 的名字，并链到站外选型文；本文按国际站官方对比、限制和计费说明，写套餐与云服务器的适用场景、流量超额、磁盘与快照配额，以及阿里云轻量迁到 ECS 的边界。
- 2026-11-09：新增《腾讯云国际版身份验证怎么做：注册地、证件与变更限制》`docs/tencent-cloud-international-identity-verification.md`。既有购买和代开文章只写了内地资源要验证、验证不可撤销、变更间隔 30 天。本文按国际站账号中心现行说明，区分绑卡与身份验证，写个人/企业证件、3 个与 10 个名额、提交与审核，以及不能解除但可在满 30 天后变更的操作路径。
