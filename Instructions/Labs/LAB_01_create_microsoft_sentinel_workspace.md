---
lab:
  title: 练习 01：部署 Microsoft Sentinel
  module: Guided Project - Create and configure a Microsoft Sentinel workspace
---

>**注意**：为了完成此实验室，你将需要一个 [Azure 订阅](https://azure.microsoft.com/en-us/free/?azure-portal=true)。 在其中具有管理访问权限。

## 一般指南

- 创建对象时，请使用默认设置，除非有不同的配置要求。
- 只有创建、删除或修改对象才能达到上述要求。 对环境进行不必要的更改可能会对您的最终成绩产生不利影响。
- 如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

我们目前正在评估公司环境的现有安全状况。 公司需要你帮助设置安全信息和事件管理 (SIEM) 解决方案，以帮助识别未来的和正在发生的网络攻击。

## 体系结构关系图

![日志分析工作区图。](../Media/apl-5001-lab-diagrams-01.png)

## 技能任务

你需要部署 Microsoft Sentinel 工作区。 该解决方案必须满足以下要求：

- 确保 Sentinel 数据存储在美国西部 Azure 区域。
- 确保所有 Sentinel 分析日志都保留 180 天。
- 将角色分配给 Operator1，以确保 Operator1 可以管理事件并运行 sentinel playbook。 解决方案必须符合最低权限原则。

## 练习说明

### 任务 1：创建 Log Analytics 工作区

创建日志分析工作区，包括区域选项。 详细了解 [Microsoft Sentinel 中的角色](https://learn.microsoft.com/azure/sentinel/quickstart-onboard)。

  1. 在 Azure 门户中，搜索并选择`Microsoft Sentinel`。
  1. 选择 **+ 新建**。
  1. 选择**新建工作区**。
  1. 选择`RG2`为资源组
  1. 为日志分析工作区输入有效名称
  1. 选择`West US`作为工作区的区域。
  1. 选择**查看 + 创建**以创建工作区。
  1. 选择**创建**以创建工作区。

### 任务 2：部署 Microsoft Sentinel 到工作区

向工作区部署 Microsoft Sentinel

  1. `workspace`部署完成后，选择**刷新**以显示新`workspace`内容。
  1. `workspace`选择要添加 Sentinel 的工作区（在任务 1 中创建）。
  1. 选择**添加**。

### 任务 3 - 向用户分配 Microsoft Sentinel 角色

将 Microsoft Sentinel 角色分配给使用。 了解有关在 [Microsoft Sentinel 中工作的角色和权限](https://learn.microsoft.com/azure/sentinel/roles)的更多信息

  1. 转到资源组 RG2
  1. 选择**访问控制 (IAM)**。
  1. 选择**添加**并`Add role assignment`。
  1. 在搜索栏中，搜索并选择`Microsoft Sentinel Contributor`角色。
  1. 选择**下一步**。
  1. 选择`User, group, or service principal`选项
  1. 选择 **+ 选择成员**。
  1. 在实验室说明中搜索 `Operator1` 分配的 `(operator1-XXXXXXXXX@LODSPRODMCA.onmicrosoft.com)`。
  1. 选择 `user icon`。
  1. 选择**选择**  。
  1. 选择“查看 + 分配”。
  1. 选择“查看 + 分配”。

### 任务 4：配置数据保留期

配置数据保留 [：了解有关数据保留的更多信息](https://learn.microsoft.com/azure/azure-monitor/logs/data-retention-archive)。

  1. 转到 `Log Analytics workspace` 任务 1 步骤 5 中创建的。
  1. 选择**使用情况和预估成本**。
  1. 请选择**数据保留**。
  1. 将数据保留期更改为 **180 天**。
  1. 选择“确定”****。

>**注意**：如需更多练习，请完成 [创建和管理 Microsoft Sentinel 工作区](https://learn.microsoft.com/training/modules/create-manage-azure-sentinel-workspaces/) 模块。
