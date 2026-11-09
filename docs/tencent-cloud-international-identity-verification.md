---
title: 腾讯云国际版身份验证怎么做：注册地、证件与变更限制
description: 腾讯云国际版什么时候必须做身份验证，个人证件和企业登记文件有何不同，同一身份能认证几个主账号，以及通过后为什么不能解除、距上次完成满 30 天才能变更。
permalink: /tencent-cloud-international-identity-verification/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [阿里云身份验证](aliyun-international-account-verification.md) · [轻量还是云服务器](lightweight-server-vs-ecs-cvm-guide.md) · [腾讯云身份验证](tencent-cloud-international-identity-verification.md)

# 腾讯云国际版身份验证怎么做

腾讯云账号购买或腾讯云代开完成之后，控制台里会同时出现两道关：一道是付款，一道是身份验证。只买中国香港或海外地域时，先挡住购买的通常是付款绑卡，而不是证件；一旦要买中国内地节点、线上开票或申请合作伙伴，身份验证又会变成硬条件。购买说明里只写了「不能撤销、内地资源要先验证」。这篇按国际站账号中心 2026 年 6 月和 7 月更新的说明，把「要不要做、交什么证件、名额有多少、通过后还能不能改」拆开。入口名称和审核时长会调整，以官方当前页面和控制台显示为准。

阿里云国际版是另一套规则：个人验证通过后类型会锁定，控制台不能自行解绑。不要把那套步骤套到腾讯云上。对照见 [阿里云国际版账号身份验证怎么做](aliyun-international-account-verification.md)。

## 先把绑卡和身份验证分开

[账号概述](https://www.tencentcloud.com/document/product/378/80429) 写明，国际站账号要购买和使用云产品，需要已经完成银行卡绑定或信用授权。同一页的站点对比表把「交易准备」写成：已绑定信用卡，或已申请信用额度。这是付款门槛。

身份验证确认的是账号和资源归谁。未完成时，[身份验证常见问题](https://www.tencentcloud.com/document/product/378/3629) 列出的直接影响是三条：

- 不能购买或使用中国内地地域的云服务；
- 不能申请成为腾讯云合作伙伴；
- 不能申请线上自助开票。

中国香港、澳门、台湾在这句里被明确排除在「中国内地」之外。只使用香港或海外地域、也不开票、不申请合作伙伴时，上述 FAQ 没有把身份验证写成购买前的必做项。中文版 [实名认证概述](https://intl.cloud.tencent.com/zh/document/product/378/80678) 在这三项后面写了「等」，所以不要把清单理解成永远只有这三件事；某个产品页面若另外要求认证，以该页面的提示为准。

渠道结算的账号不要为了「完成验证」去自己绑卡或改付款方式。付款设置怎么处理，见 [腾讯云代开流程与验收](tencent-cloud-agency-opening.md)。

## 个人还是企业：提交前定下来

认证直接决定账号和资源的归属。官方在总览里特别提醒：企业若用个人证件完成认证，之后人员变动或账号争议可能影响业务，甚至造成资金损失。先确认这批资源是个人用还是公司用，再选类型。总览见 [Identity Verification Overview](https://www.tencentcloud.com/document/product/378/80678)。

| 对比项 | 个人身份验证 | 企业身份验证 |
| :--- | :--- | :--- |
| 适合谁 | 中国内地以外的个人 | 中国内地以外的企业、个体工商户、政府、事业单位、学校、组织等 |
| 账号归属 | 个人 | 企业 |
| 证件 | 账号注册地政府签发的有效身份证（ID Card）、护照、驾照或工作许可证 | 账号注册地政府签发的有效商业登记文件 |
| 通过后官方写明可以做的事 | 参加个人类运营活动；申请线上开票 | 参加企业类运营活动；申请成为合作伙伴；申请授信额度；申请线上开票 |

线上开票两类都能申请。合作伙伴和授信额度写在企业验证一侧。授信额度的入口和能否批下来，以控制台和审核结果为准，本文不写额度和费率。

账号概述还说明：国际站面向中国内地以外的个人、组织、企业，以及中国企业的海外分支；中国站面向内地企业（不含港澳台）以及内地、港澳台的个人。两个站点的账号资源隔离，不能互转或合并。只持有中国居民身份证或内地营业执照的，应到中国站注册，国际站不收这些证件。

## 证件必须和注册地对上

注册地是注册时选定的国家或地区，提交页会显示它，选定后不能修改。个人和企业材料都必须来自同一个国家或地区。见 [个人身份验证](https://www.tencentcloud.com/document/product/378/10495) 和 [企业身份验证](https://www.tencentcloud.com/document/product/378/10496)。

因此代开或自己注册之前，就要先问清注册地，再核对你手里的证件是不是这个地方发的。注册地不能改成证件所在国。对不上时，要么改用注册地的有效证件，要么这个账号先不做认证（仅当你不需要内地资源、在线开票和合作伙伴）。购买前要核对的其他项见 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。

材料本身有四条硬要求：

- 首次提交接受 JPG、PNG 或 PDF，单个文件不超过 10 MB。
- 画面完整、文字清楚，没有涂改或伪造。
- 填写的全名、证件号，或企业名称、注册号、注册地址，与文件逐字一致。企业名称有多种语言时，用英文。
- 不要提交不属于你本人或你所在机构的证件。

企业名称还有一个容易忽略的限制：主账号上的企业名称至少 3 个字符。短于这个长度时，官方要求提交工单并附上登记文件，而不是在页面上硬提交。见 [账号限制](https://www.tencentcloud.com/document/product/378/80431)。

## 一个身份能认证几个主账号

同一份主体信息能挂的主账号数量有上限，满了就不能再给新账号提交：

| 主体 | 主账号上限 | 满了以后 |
| :--- | :--- | :--- |
| 个人 | 3 个 | 先注销已认证账号，或把其中某个账号变更成其他主体，释放名额后再提交 |
| 企业 | 默认 10 个 | 默认额度不够时，官方写的是联系客户经理处理 |

这是认证名额，不是注册名额。同一限制页还写着：一个邮箱只能注册 1 个主账号，一个手机号最多绑定 5 个主账号。换一个邮箱可以再开主账号，但同一份个人证件仍然最多认证 3 个。

主账号完成认证后，它创建的子账号继承这份认证信息，不需要单独再认证。日常操作仍应使用 CAM 子用户，见 [阿里云 RAM、腾讯云 CAM 子用户怎么设](ram-cam-subuser-permission-guide.md)。

## 在哪里提交，审核要多久

操作在腾讯云国际站控制台（tencentcloud.com）的账号信息里。菜单语言以账号实际显示为准。

1. 进入 Account Information（账号信息），打开 Basic Information（基本信息）里的 Verification Status（认证状态）。
2. 尚未认证时，点击 Submit for Verification（提交认证）。已经认证的账号不要重复提交，同一页可以查看状态。
3. 个人：选择 Individual Verification（个人认证），点击 Verify Now。证件类型选注册地的身份证（ID Card）、护照、驾照或工作许可证，上传文件，填写与证件一致的全名和证件号。中国居民身份证不在此列。
4. 企业：选择 Enterprise Verification（企业认证）。上传登记文件，填写与文件一致的企业名称、注册号和注册地址。
5. 勾选信息完整、真实、有效，然后提交。

提交后由人工审核，官方写的时间是 2 到 4 个工作日。结果通过站内信、邮件和短信通知。超过这个时间还没有结果，按页面上的在线客服入口询问，不要把证件图片发到无关的聊天工具里。

未通过时，回到认证页面看具体原因，改完再提交。已经通过的不需要因为「等得久」再交一次。

## 通过之后：不能解除，满 30 天可以变更

「不能撤销」容易理解成什么都不能改。账号限制页和 FAQ 把两件事分开写了：

- **不能解除。** 认证状态不能退回未认证。要换主体，只有两条路：变更认证，或注销账号（名额释放后，用新主体重新认证）。
- **可以变更，但要等。** 变更次数没有总上限；每一次都要等上一次认证完成满 30 天。FAQ 把同一条规则写成 30 天内最多变更一次。

个人可以改成另一个个人，也可以改成企业；企业可以改成另一个企业，也可以改成个人。变更说明建议不要随意修改，因为归属会跟着变。步骤见 [修改个人认证信息](https://www.tencentcloud.com/document/product/378/37276) 和 [修改企业认证信息](https://www.tencentcloud.com/document/product/378/80680)：

1. 仍在认证状态里进入 View/Change Verification（查看/变更认证），点击 Modify verification（修改认证）。
2. 选择目标类型。从个人认证改出时，页面按钮是 Verify Now；从企业认证改出时，按钮是 Next。后面要填的字段和首次提交相同。变更企业材料时，上传格式写明还包括 JPEG，单个仍不超过 10 MB，且要求原始、有效、清晰、未编辑的登记文件。
3. 再次勾选确认并提交。审核仍是 2 到 4 个工作日，通知渠道同样是站内信、邮件和短信。

审核还没出结果时，可以提交工单撤回。撤回后，认证主体恢复成原来的那个。在新申请被批准之前，资源和已经产生的费用仍算在原认证主体名下。批准之后，账号类型会自动更新成新的认证类型。

所以：成品号如果已经用别人的证件认证通过，你不能把它退回未认证。距离上次认证完成不满 30 天时，官方也不允许变更。即使已经满 30 天，仍要有与该账号注册地一致的本人或本公司证件，才能换成你的主体。这条和「注册地不可改」叠在一起，就是成品号不适合做正式业务的原因之一。

## 和账号购买、代开怎么配合

- 只用香港及海外地域、暂时不需要线上开票和合作伙伴：可以先不提交认证，把时间用在改密码、换绑邮箱和绑定 MFA 上。
- 已经确定要买中国内地地域：先完成认证，再购买。认证通过不等于内地网站可以马上对外提供服务，网站备案是另一件事，见 [地域怎么选](cloud-region-selection-guide.md)。
- 公司资源、可能要授信额度或合作伙伴：第一次就提交企业认证，不要先用个人证件占住主体。
- 渠道交付的新主账号：用你自己的、与注册地一致的证件提交。验收时先确认你能改密、换绑邮箱、管理 MFA，再上传证件。顺序见 [腾讯云代开流程与验收](tencent-cloud-agency-opening.md)。

官方 FAQ 还有一条和购买来源直接相关的提醒：不要在腾讯云官网及其授权代理以外的平台购买或注册账号，以免个人或企业信息被拿去给其他账号做认证。发现证件已被他人用于认证时，官方给出的办法是使用账号找回工具尝试找回，登录后再自行注销不需要的账号。

## 账号从哪里来

身份验证挂在这个 tencentcloud.com 主账号上，注册地又不能改。还没有独立主账号时，先看 [腾讯云国际版账号购买指南](tencent-cloud-international-account-purchase.md)。没有合适的国际信用卡，可以通过 [云大仙](https://www.yundaxian.com/) 办理 [腾讯云账号购买与代开](https://www.yundaxian.com/open/tencent)。拿到账号后，先确认注册地和登录邮箱是不是你能长期控制的，再决定要不要提交证件。

## 官方参考

- [Identity Verification Overview](https://www.tencentcloud.com/document/product/378/80678)
- [Individual Identity Verification](https://www.tencentcloud.com/document/product/378/10495)
- [Enterprise Identity Verification](https://www.tencentcloud.com/document/product/378/10496)
- [Tencent Cloud Account Limits](https://www.tencentcloud.com/document/product/378/80431)
- [Identity verification FAQ](https://www.tencentcloud.com/document/product/378/3629)
- [Modifying Individual Verification Information](https://www.tencentcloud.com/document/product/378/37276)
- [Modifying Enterprise Verification Information](https://www.tencentcloud.com/document/product/378/80680)
- [Tencent Cloud Account Overview](https://www.tencentcloud.com/document/product/378/80429)

## 一句话总结

国际站不是买任何地域都要先交证件：中国内地资源、线上开票和合作伙伴申请被写成必须认证，中文总览在这三项后还留了「等」。证件必须来自账号注册地，个人同一身份最多 3 个主账号，企业默认 10 个。通过后不能解除认证，变更要等满 30 天，所以提交前先定个人还是企业，也不要接一个已经用别人证件认证过的成品号。

返回 [指南首页](index.md)
