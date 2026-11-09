---
title: 阿里云国际版账号身份验证怎么做：个人、企业与内地资源
description: 阿里云国际版什么时候必须做身份验证，个人护照或驾驶证与企业证件有何不同，审核大概要多久，以及验证完成后为什么不能在控制台自行解绑。
permalink: /aliyun-international-account-verification/
---

[指南首页](index.md) · [阿里云账号购买](aliyun-international-account-purchase.md) · [阿里云代开](aliyun-agency-opening.md) · [腾讯云账号购买](tencent-cloud-international-account-purchase.md) · [腾讯云代开](tencent-cloud-agency-opening.md) · [USDT 充值](usdt-cloud-top-up.md) · [阿里云充值方式](aliyun-international-top-up.md) · [阿里云还是腾讯云](aliyun-vs-tencent-cloud-international.md) · [防封清单](cloud-account-risk-control.md) · [AWS 常见问题](aws-account-faq.md) · [CDT 免费流量](aliyun-international-cdt-free-traffic.md) · [地域怎么选](cloud-region-selection-guide.md) · [子用户与权限](ram-cam-subuser-permission-guide.md) · [欠费停机与恢复](cloud-overdue-payment-suspension-guide.md) · [账单与用量](cloud-international-billing-usage-guide.md) · [快照与备份](cloud-snapshot-backup-guide.md) · [安全组怎么配](cloud-security-group-guide.md) · [身份验证](aliyun-international-account-verification.md)

# 阿里云国际版账号身份验证怎么做

做完阿里云账号购买或阿里云代开之后，只使用中国香港或海外地域时，通常不会因为地域本身被要求先做身份验证；一旦改买中国内地节点，购买前就会被拦住。国际版和中国站在这件事上不是同一套规则：中国站买任何资源之前都要实名，国际站只在一部分场景才要求。这篇按国际站账号中心的现行说明，整理「要不要做、做个人还是企业、提交时核对什么、做完还能不能改」。审核时长、入口名称和可购产品都会调整，请以官方当前页面和控制台显示为准。

## 什么时候必须做，什么时候可以先不做

官方对两个站点的对比写得很清楚：中国站在购买或使用任何云资源之前，必须完成个人或企业实名；国际站的实名是**特定场景**才需要，其中包括购买或使用位于中国内地的云资源，以及申请信用额度。详见 [China Website vs International Website](https://www.alibabacloud.com/help/en/account/aliyun-vs-alibaba-cloud)。

国际站身份验证总览把场景拆得更细，见 [Identity verification overview](https://www.alibabacloud.com/help/en/account/account-verification-overview)：

- 购买部署在中国内地节点的特定云服务，或开通**包含中国内地**的加速地域，需要先做身份验证，这是合规要求。
- 下面四件事需要的是**企业**身份验证：与阿里云签在线合同、申请信用额度、加入阿里云合作伙伴网络（ACPN）、绑定一个与账号 Country/Region 不在同一国家或地区的手机号。

只用中国香港和海外地域、也不做上面这些事，官方并没有要求先完成身份验证。个人验证和企业验证的常见问题里都写了同一条出路：如果暂时完不成验证、又要服务中国内地用户，可以用中国（香港）地域，该地域不要求这项身份验证。地域本身怎么选，见 [地域怎么选](cloud-region-selection-guide.md)。

另外两件容易混在一起的事：

- **账号身份验证和域名实名不是一回事。** 前者确认这个阿里云账号属于哪个法律主体，一个账号只能挂一个主体。后者确认某个域名归谁，组织注册、地域又在中国内地的域名才会涉及。同一个账号可以管理多个域名，域名上的验证主体和账号上的验证主体可以不同，这不代表你还有另一个阿里云账号。
- **客服工单里的「核实身份」也不是这篇说的证件审核。** 官方另有一篇 [Identity verification FAQ](https://www.alibabacloud.com/help/en/account/support/how-to-verify-your-account-identity)，适用情形是：停用账号前要删除默认付款方式、自己改不了安全手机、或要确认账号是否被盗。那种流程是提交工单，并提供银行交易记录，或卡号前 6 位加后 4 位和持卡人姓名。它不代替下面的护照、驾驶证或商业登记文件审核。

国际站个人验证只接受护照和驾驶证。官方举例说明，巴西 RG 这类国民身份证和居留许可都不在接受范围内；只有中国内地居民身份证，或者企业注册在中国内地的，应到中国站（aliyun.com）注册和购买。两个站点的账号也不能互相升级，见 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md)。

## 个人还是企业：提交前定下来

注册并完善资料时，账号类型已经选过 Personal（个人）或 Enterprise（企业）。进入验证阶段后，能选的类型取决于这个设定：

- 个人账号可以做个人验证，也可以做企业验证。企业验证一旦通过，账号类型会从个人变成企业。
- 已经是企业类型的账号，只能做企业验证。

| 对比项 | 个人身份验证 | 企业身份验证 |
| :--- | :--- | :--- |
| 证件 | 护照或驾驶证，只接受这两种 | 账号 Country/Region 所在地政府签发的有效商业登记文件 |
| 通过后能做的事 | 验证通过并提交购买意向后，可购买中国内地的特定云服务，或开通包含中国内地的加速地域；产品是否可售以当时页面为准 | 在个人验证的范围之外，还可以签在线合同、申请信用额度、加入 ACPN、绑定与账号国家或地区不同的手机号 |
| 对账号的影响 | 类型锁定为个人，不能再改；账号上的姓名改为与证件一致 | 类型锁定为企业，不能再改回个人；公司名称改为与商业文件一致 |
| 审核 | 人工审核，大约 3 个工作日，结果发到邮箱 | 同样大约 3 个工作日，结果发到邮箱 |

几个做完就收不回来的限制，总览页写在 Limitations 里：

1. **一个账号只能有一种验证**，个人或企业，不能两个都挂。
2. **不能在控制台自行解绑。** 要改已经通过的验证信息，需要提交工单联系客服；账号不再使用，可以直接关闭账号。
3. **证件签发国家或地区必须和账号里的 Country/Region 一致。** 这个字段在账号中心的 Basic Information（基本信息）里可以先核对。
4. **验证成功后账号类型锁定。** 个人验证通过之后，不能再改成企业；企业验证通过之后，也不能改回个人。

因此，如果后面可能要签合同、要信用额度，或者手机号所在国家与账号国家不一致，应在第一次提交时就走企业验证，而不是先用护照「临时过一下」。

同一个手机号最多可以注册 6 个阿里云账号，这些账号可以分别做个人验证或企业验证。这是官方对手机号的上限说明，不表示多开账号就没有其他风控或使用政策上的约束。

## 材料怎么准备才不容易被打回

个人和企业的文件要求基本相同，差别在证件种类。下面这些都来自官方提交说明，不满足就容易变成 Failed。

- 格式为 JPG、JPEG、PNG 或 PDF。最多上传两个文件，每个不超过 5 MB。证件本身至少占画面的 40%。
- 彩色、完整、四个角都在画面里，文字能读清，没有反光和阴影。正着拍，不要旋转 90 度或 180 度。
- 不要交截图、复印件或后期修改过的图片，不要交过期或无效证件，也不要交不属于你本人或你所在机构的证件。
- 页面上填写的姓名、公司名、证件号必须和文件逐字一致。个人证件没有姓时，官方说明是把名再填一次到姓的位置。企业名称有多种语言时，用英文；公司名支持搜索并可能自动带出登记号，但美国、阿联酋、德国的企业搜索不可用，这些国家要改成手动填写。

个人账号在提交前确认 Account Type 仍是 Personal，并备好与 Country/Region 一致的护照或驾驶证。企业账号（含政府、公共机构、团体）准备商业登记文件的正反面；如果当前仍是个人账号、但打算直接做企业验证，入口文案是升级为企业账号，而不是个人验证那一栏。

## 在哪里提交，状态怎么读

操作入口在阿里云国际站账号中心（可从 `account.alibabacloud.com` 进入）的 Identity Verification（身份验证）页面。菜单文字以你账号里的实际语言为准。

1. 个人验证：在 Verify Your Identity - Individual Account 一栏点击 Verify Now。证件类型选护照或驾驶证，上传文件，填写与证件一致的名、姓和证件号。
2. 企业验证：账号类型已是 Enterprise/Government 时，在 Verify Your Identity - Enterprise Account 一栏点击 Verify Now；账号类型仍是 Personal 时，在 Upgrade to an Enterprise Account 一栏进入。上传商业登记文件正反面，填写公司名称、登记号和注册地址。
3. 页面上有一项要单独选择：是否购买中国内地云资源，或使用包含中国内地的加速地域。选 Yes，系统会**另外**发起一项合规审核，这项审核的状态和账号验证状态分开显示。选 No，表示当前不打算买内地资源；账号验证成功之后，仍可以回到该页面，阅读条款并确认，再补上这项合规审核。
4. 勾选信息真实、准确、完整，然后提交。

提交后进入人工审核。同一页可以看到三种状态：

| 状态 | 含义 |
| :--- | :--- |
| Under Review | 已提交，处理中。官方写的处理时间大约是 3 个工作日 |
| Verification Successful | 已通过，可以购买当时页面允许的产品和服务 |
| Failed | 未通过。页面上会给原因，改正后可以重新提交 |

大约 3 个工作日是文档里的常见耗时，不是到时必出结果。到期没有邮件时，先看垃圾箱；仍然没有，或认为 Failed 有误，再按页面提示联系技术支持。验证通过也不等于内地网站已经可以对外提供服务：对外网站通常还要单独办 ICP 备案，国际站和中国站的备案路径不一样，是否需要备案见 [地域怎么选](cloud-region-selection-guide.md)。

## 和账号购买、代开怎么配合

身份验证挂在这个国际版账号上，通过之后不能在控制台自己拆掉，账号上的姓名或公司名也会改成证件上的名字。因此：

- 如果成品号已经做过身份验证，验证主体是当时提交证件的人，控制台不能自行解绑，要改信息得提交工单。这类账号不适合作为你自己的正式业务账号。来源上的差别见 [阿里云国际版账号购买指南](aliyun-international-account-purchase.md)。
- 渠道代开交付的独立主账号，这项验证应由你用自己的护照、驾驶证或本公司的商业登记文件提交。不要使用不属于你的证件，官方提交说明明确禁止这样做。
- 交付后先确认自己能改密码、换绑邮箱、管理 MFA，再决定要不要提交验证。验收顺序见 [阿里云代开流程与验收](aliyun-agency-opening.md)。
- 只用香港及海外地域时，可以先不验证；一旦确定要买内地节点或要开包含内地的加速，再按上面的个人或企业流程提交。

## 账号从哪里来

身份验证的前提，是这个 alibabacloud.com 账号由你自己长期掌控。没有合适的国际信用卡时，可以通过 [云大仙](https://www.yundaxian.com/) 办理阿里云国际版账号。账号到手后的加固见 [国际云账号风控与防封清单](cloud-account-risk-control.md)。

## 官方参考

- [Identity verification overview](https://www.alibabacloud.com/help/en/account/account-verification-overview)
- [Individual identity verification](https://www.alibabacloud.com/help/en/account/verify-your-identity-individual-account)
- [Enterprise identity verification](https://www.alibabacloud.com/help/en/account/verify-your-identity-enterprise-account/)
- [China Website vs International Website](https://www.alibabacloud.com/help/en/account/aliyun-vs-alibaba-cloud)
- [Identity verification FAQ](https://www.alibabacloud.com/help/en/account/support/how-to-verify-your-account-identity)（客服核实账号归属，不是证件实名流程）

## 一句话总结

国际站不是买任何资源都要实名：内地节点和包含内地的加速要做身份验证，合同、信用额度、合作伙伴和跨国绑定手机号要做企业验证。证件国家必须和账号 Country/Region 一致，个人只收护照或驾驶证，通过后类型锁定且不能自行解绑，所以提交前先定个人还是企业。

返回 [指南首页](index.md)
