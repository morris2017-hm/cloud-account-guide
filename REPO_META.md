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

- 2026-11-09：新增《阿里云国际版账号身份验证怎么做：个人、企业与内地资源》`docs/aliyun-international-account-verification.md`。既有文章只在购买中国内地地域时一笔带过验证要求，本文专写国际站个人/企业验证的适用场景、证件、审核与不可撤销规则。
- 2026-11-09：新增《轻量应用服务器还是云服务器：阿里云 ECS、腾讯云 CVM 怎么选》`docs/lightweight-server-vs-ecs-cvm-guide.md`。既有文章只在购买流程里提到轻量和 Lighthouse 的名字，并链到站外选型文；本文按国际站官方对比、限制和计费说明，写套餐与云服务器的适用场景、流量超额、磁盘与快照配额，以及阿里云轻量迁到 ECS 的边界。
