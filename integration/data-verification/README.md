---
description: >-
  数据正确性和准确性是所有数据类产品的基础，没有这个基础，后续所有的分析功能都会大打折扣，甚至完全无法使用。另外，当您需要从原有系统迁移到易观方舟，或者您希望将易观方舟的数据同您原有的数据仓库的数据进行比对校验时，也常会遇到和自己所期望的数据结果不一致的情况。如果您遇到这些情况，请不要着急，这些问题非常正常，本文会提供一些步骤帮助您更快地找到问题的原因。
---

# 数据验证

{% hint style="info" %}
如果你正在使用易观方舟任何一个商业版本，并且遇到了数据不准的情况，请立即联系您的专属客户服务专家。我们非常重视客户数据的准确性问题，用户成功专家会全程协助您解决。
{% endhint %}

如果您倾向于自主验证数据，可参考以下5步来进行：

## **第一步：客户端埋点检查**

很多的数据问题都是因为客户端埋点不正确引起的。所以首先应该从源头触发进行检查。主要检查点为：

1. 是否使用最新版本SDK
2. 是否覆盖了所有涉及到的页面
3. 是否覆盖了位于不同页面的相同事件
4. 数据是否能正常发送
5. 是否因为页面跳转导致有些事件不能完全上报
6. 是否包含了更适合在服务端上报的事件

您可以通过下方文档详细了解客户端埋点验证的方法：

{% page-ref page="sdk-verification.md" %}

## **第二步：数据来源一致性检查**

在同原有数据分析平台或数据仓库对比数据时常会遇到某些指标偏差很大的情况。这时我们首先应检查两个数据平台的数据来源是否一致。大多数时候方舟的数据都来自于客户端的用户行为，如果其它平台的数据来源和易观方舟不一致，那么很可能两边得到的统计结果也不同。如果您正巧遇到了这种情况，建议您采用相同的数据上报方式。

## **第三步：统计口径检查**

不同的数据平台之间很可能存在指标口径的差异，口径不同会导致数据结果的偏差。故在对比数据一致性时，一定要先了解口径的差异。

另外，如果某指标是基于您自定义的事件或属性，还需要确认数据上报的正确以及来源的一致。

## **第四步：数据入库检查**

数据正确、完整入库才能被后续使用，所以如果前几步都确认无误，接下来需要检查数据是否正确入库。我们在产品内提供了数据接收情况的工具，目前您可以在产品中的如下位置：

> 管理 &gt; 服务集成配置 &gt; 接入更多数据 &gt; Debug 数据验证/错误数据日志

查看到哪些数据因为有误而没有进入系统。如果有过大比例的数据没有进入系统，那么一定会很大程度影响数据完整性。

数据可能因为以下原因导致出错：

1. 数据格式无效
2. 属性值类型与首次上报时不一致
3. 客户端上报时间是否超过服务端接收限制

您可以通过下面的文档了解数据入库检查的具体使用方法：

{% page-ref page="data\_debug.md" %}

## **第五步：基本指标检查**

如果前面几步检查均无误，我们建议您通过 UV 来比对易观方舟工作的状态。选用 UV 的原因是其口径在各个系统间通常会比较一致。

假设 UV 都偏差较大，建议您着重检查第一、二和四步。假设 UV 基本一致，而其它指标有偏差，建议您检查第三、四步。

{% hint style="info" %}
在查询指标的过程中，若发现异常，可以通过进一步细分维度，定位可能的原因。

部分指标异常是因为项目中包含了内部流量，内部流量和外部流量行为上通常会有较大差异，尤其是测试人员的强度测试会造成某些页面或者某个事件上报大量数据， 影响衡量真正用户的指标。

目前可以通过以下方式过滤：

1. 在分析模型中创建指标时，添加事件条件，选择 IP 不等于内部 IP；
2. 创建 [虚拟事件](../../features/project-manegement/merged-events.md)，增加过滤条件 IP 不等于内部 IP。

若测试集中在少数人员上，也可以选择用户ID、邮箱或者手机号来过滤具体的用户。

更便捷的 IP 过滤/数据视图我们正在开发中。
{% endhint %}
