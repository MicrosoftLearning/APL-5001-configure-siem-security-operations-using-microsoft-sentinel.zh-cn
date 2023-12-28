---
lab:
  title: 练习 02：引入Windows 安全事件数据
  module: Guided Project - Deploy Microsoft Sentinel Content Hub solutions and data connectors
---

>**注意**：本实验室建立在实验室 01 基础上。 为了完成此实验室，你将需要一个 [Azure 订阅](https://azure.microsoft.com/free/?azure-portal=true)。 其中您拥有管理权限。

## 一般指南

- 创建对象时，请使用默认设置，除非有不同的配置要求。
- 只有创建、删除或修改对象才能实现既定要求。 对环境进行不必要的更改可能会对您的最终成绩产生不利影响。
- 如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

我们需要使用 Microsoft Sentinel 解决方案配置 Microsoft Sentinel 以引入数据。

## 体系结构关系图

![内容中心数据连接器示意图](../Media/apl-5001-lab-diagrams-lab02.png)

## 技能任务

需要在 Microsoft Sentinel 工作区中部署内容中心解决方案，并满足以下要求：

- 安装以下解决方案：
  - Windows 安全事件。
  - Azure 活动连接器。
  - Microsoft Defender for Cloud。
- 配置 Azure Activity 的数据连接器，以应用订阅中的所有新资源和现有资源。
- 配置 Microsoft Defender for Cloud 的数据连接器，以连接到 Azure 订阅，并确保只启用双向同步。
- 根据资源创建或部署活动模板的可疑数量启用分析规则。 该规则应每小时运行一次，并只查询最后一小时的数据。
- 确保 Azure 活动工作簿在我的工作簿中可用。

## 练习说明

>**注意**：在以下任务中，若要访问 `Microsoft Sentinel`，请选择在实验室 01 中创建的`workspace`。

### 任务 1 - 部署 Microsoft Sentinel 内容中心解决方案

部署内容中心解决方案并配置数据连接器。 了解有关内容[中心解决方案](https://learn.microsoft.com/azure/sentinel/sentinel-solutions)的更多信息。

1. 在 `Microsoft Sentinel` 中，转到 `Content management` 菜单部分并选择**内容中心**
1. 搜索并选择 **Windows 安全事件**
1. 选择**查看详细信息**链接
1. 选择 Windows 安全事件计划，然后选择**创建**
1. 选择包含 Microsoft Sentinel 工作区的资源组`RG2`，然后选择`Workspace`。
1. 选择**下一步**至数据连接器选项卡（解决方案将部署 2 个数据连接器）
1. 选择**下一步**至工作簿选项卡（解决方案将安装工作簿）
1. 选择**下一步**至分析选项卡（解决方案将安装分析规则）
1. 选择**下一步**至猎取查询选项卡（解决方案安装猎取查询）
1. 选择**查看 + 创建**
1. 选择**创建**

1. 对`Azure Activity`和`Microsoft Defender for Cloud`解决方案重复这些步骤。

### 任务 2 - 为 Azure 活动设置数据连接器

配置 Azure Activity 的数据连接器，以应用订阅中的所有新资源和现有资源。 详细了解[ Microsoft Sentinel 数据连接器](https://learn.microsoft.com/azure/sentinel/connect-data-sources)。

  1. 在 `Microsoft Sentinel` 中，转到 `Content management` 菜单部分并选择**内容中心**
  1. 在 `Content hub` 中，筛选`Status`已安装的解决方案。
  1. 选择 `Azure Activity` 解决方案，然后选择**管理**。
  1. 选择`Azure Activity`，然后选择**打开连接器页面**。
  1. 在`Instructions`选项卡下的`Configuration`区域中，向下滚动到 `2. Connect your subscriptions...`，然后选择**启动 Azure 策略分配向导>。**
  1. 在**基本信息**选项卡中，选择**范围**下的省略号按钮 (...)，然后从下拉列表中选择你的“订阅”，然后单击**选择**。
  1. 选择“参数”选项卡，从“主要 Log Analytics 工作区”下拉列表中选择你的工作区********。
  1. 选择**修正**选项卡，然后选择**创建修正任务**复选框 。
  1. 选择**查看 + 创建**按钮，检查配置。
  1. 选择**创建**以完成操作。
  
### 任务 3 - 为 Defender for Cloud 设置数据连接器

配置 Microsoft Defender for Cloud 的数据连接器，并确保仅配置事件管理。

  1. 在 `Microsoft Sentinel` 中，转到 `Content management` 菜单部分并选择**内容中心**
  1. 在 `Content hub` 中，筛选`Status`已安装的解决方案。
  1. 选择 `Microsoft Defender for Cloud` 解决方案，然后选择**管理**。
  1. 选择`Subscription-based Microsoft Defender for Cloud (Legacy)`，然后选择**打开连接器页面**。
  1. 在`Configuration`选项卡下的`Instructions`区域中，向下滚动到订阅，并将列中的`Status`滑块移动到**连接**。
  1. 确保`Bi-directional sync`**已启用**。

### 任务 4：创建分析规则

根据资源创建或部署活动模板的可疑数量创建分析规则。 该规则应每小时运行一次，并只查询最后一小时的数据。 了解有关使用 [Microsoft Sentinel 分析规则模板](https://learn.microsoft.com/azure/sentinel/detect-threats-built-in)的更多信息。

  1. 在 `Microsoft Sentinel` 中，转到 `Configuration` 菜单部分并选择**分析**。
  1. 在`Rule templates`选项卡中，搜索**资源创建或部署活动的可疑数量**。
  1. 选择**资源创建或部署活动的可疑数量**，然后选择**创建规则**。
  1. 保留`General`选项卡上的默认值，然后选择**下一步：设置规则逻辑 >**。
  1. 保留默认值`Rule query`并使用表格进行配置`Query scheduling`：

     |设置 |值|
     |---|---|
     |运行查询的间隔|1 小时|
     |查找上次的数据|1 小时|

  1. 选择**下一步: 事件设置>**。
  1. 保留默认值并选择**下一步：自动回复 >**。
  1. 保留默认值并选择**下一步：审查并创建 >**。
  1. 选择**保存**。

### 任务 5 - 确保 Azure 活动工作簿在我的工作簿中可用

  1. 在 `Microsoft Sentinel` 中，转到 `Content management` 菜单部分并选择**内容中心**
  1. 在 `Content hub` 中，筛选`Status`已安装的解决方案。
  1. 选择 `Azure Activity` 解决方案，然后选择**管理**。
  1. 选择`Azure Activity`工作簿`checkbox` ，然后选择**配置**。
  1. 选择`Azure Activity`工作簿，然后选择**保存**。
  1. 为`Microsoft Sentinel`工作区选择`Azure Region`  
