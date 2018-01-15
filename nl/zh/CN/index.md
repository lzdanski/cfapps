---

copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 创建 Cloud Foundry 应用程序
{: #creating_cloud_foundry_apps}

通过 {{site.data.keyword.Bluemix}}，您可以在 {{site.data.keyword.Bluemix_notm}} 控制台中创建应用程序。然后，可以决定是要继续使用控制台、使用 cf 命令行界面还是使用 {{site.data.keyword.jazzhub_title}} 来开发、跟踪、规划和部署应用程序。
{:shortdesc}

在 {{site.data.keyword.Bluemix_notm}} 中创建应用程序时，可以首先使用入门模板。*入门模板*是一种模板，其中包含预定义的服务以及使用特定 buildpack 配置的应用程序代码。入门模板有两种类型：样板和运行时。

*样板*是一种容器，其中包含一个应用程序及其关联的运行时环境，以及为特定域预定义的服务。例如，“移动云”样板包含 Node.js 运行时，以及 Mobile Data、Mobile Application Security 和 Push 服务。该样板还包含一个 SDK 和一些样本应用程序，您可以基于它们来开发用于访问这些服务的移动应用程序。

*运行时*是用于运行应用程序的一组资源。{{site.data.keyword.Bluemix_notm}} 提供了多种运行时环境，作为不同类型的应用程序的容器。运行时环境以 buildpack 的形式集成到 {{site.data.keyword.Bluemix_notm}} 中，由系统自动配置使用，几乎甚至完全不需要维护。

要开始创建应用程序，请执行以下步骤：
  1. 单击 IBM Cloud 工具栏中的**目录**。
  2. 单击 **Cloud Foundry 应用程序**，然后选择运行时。按照指导经验来指定名称以及选择编码方式。单击**创建**。
  3. 按照指导经验完成操作后，单击**概述**。
  5. 在仪表板中，通过单击应用程序的“概述”上的**创建连接**，可以向该应用程序添加服务。浏览目录并从中选择服务，或者滚动到目录结尾并单击 **{{site.data.keyword.Bluemix_notm}} 试验性服务**以浏览试验性服务。也可以使用 cf 命令行界面。请参阅“用于处理应用程序的选项”。
  6. 在“概述”页面上，滚动到“Continuous Delivery”卡，然后单击**查看工具链**。应用程序的源代码将保存在 Bluemix 上托管的存储库中。此外，还将创建开放工具链，此工具链使用该存储库和 Delivery Pipeline 来开发和部署应用程序。有关 Continuous Delivery 服务的更多信息，请参阅 <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Continuous Delivery 入门</a>。

**注：**如果绑定到应用程序的服务崩溃，那么该应用程序可能会停止运行或发生错误。{{site.data.keyword.Bluemix_notm}} 不会自动重新启动应用程序以从这些问题恢复。请考虑对应用程序进行编码，使其能够识别中断、异常和连接失败状况并从中进行恢复。有关更多信息，请参阅“应用程序不会自动重新启动”故障诊断主题。

## 用于处理应用程序的选项

创建应用程序后，您可以通过一些选项继续向应用程序中添加服务，以及构建和部署应用程序：

<dl><dt>cf 命令行界面</dt>
<dd>使用 <a href="https://github.com/cloudfoundry/cli#getting-started">cf 命令行界面</a>可更新应用程序，创建服务实例或将服务绑定到应用程序。您还可以使用 cloud-cli 命令行界面来创建、更新和删除服务产品。</dd>
<dt>{{site.data.keyword.Bluemix_notm}} 用户界面</dt>
<dd>使用 {{site.data.keyword.Bluemix_notm}} <a href="https://console.bluemix.net/dashboard/apps">用户界面</a>可构建应用程序，包括选择将哪些服务和运行时组合在一起来解决您的业务问题。</dd>
<dt>{{site.data.keyword.contdelivery_full}}</dt>
<dd>使用 {{site.data.keyword.contdelivery_short}} 可自动执行构建、单元测试、部署等操作。通过丰富的基于 Web 的 IDE 来编辑和推送代码。创建工具链以允许支持开发、部署和操作任务的工具集成。Continuous Delivery 服务包含 Delivery Pipeline、Eclipse Orion Web IDE 和 Git Repos and Issue Tracking。有关更多信息，请参阅 <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Continuous Delivery 入门</a>。</dd>
</dl>

## 提示

开发 Web 应用程序时，请参考以下提示：

<dl><dt>持久性</dt>
<dd>不要为应用程序指定任何本地存储器。应用程序的每个实例随时都可能会重新启动或移动到其他虚拟机（通常是为了实现负载均衡），即使只有一个实例在运行也不例外。移动或删除应用程序时，会擦除本地存储器中存储的所有内容。请使用某个 {{site.data.keyword.Bluemix_notm}} 数据存储服务来实现持久性。</dd>
<dt>资源限制</dt>
<dd>请注意，试用帐户可以使用的资源数量存在限制。这些限制如下：<table style="width:100%">
<caption>表 1. 试用帐户的 {{site.data.keyword.Bluemix_notm}} 资源限制</caption>
  <th>资源类型</th>	<th>数量限制</th>
<tr><td>在所有应用程序上使用的服务数</td> <td>10</td>
<tr><td>在所有应用程序上使用的内存</td> <td>	2 G</td>
<tr><td>路径数</td> <td>500</td>
</table>
</dd>
</dl>
