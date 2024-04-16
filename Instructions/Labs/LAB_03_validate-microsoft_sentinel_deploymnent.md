---
lab:
  title: 练习 03：验证 Sentinel 部署
  module: 'Guided Project - Configure Microsoft Sentinel Data Collection rules, NRT Analytic rule and Automation'
---

>**注意**：本实验室建立在实验室 01 和实验室 02 的基础上。 为了完成此实验室，你将需要一个 [Azure 订阅](https://azure.microsoft.com/free/?azure-portal=true)。 在其中具有管理访问权限。

## 一般指南

- 创建对象时，请使用默认设置，除非有不同的配置要求。
- 只有创建、删除或修改对象才能达到上述要求。 对环境进行不必要的更改可能会对您的最终成绩产生不利影响。
- 如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

我们需要配置 Microsoft Sentinel，以便从运行 Windows 的虚拟机接收安全事件。

## 体系结构关系图

![通过 AMA 使用 DCR 的 Windows 安全事件示意图](../Media/apl-5001-lab-diagrams-lab03.png)

## 技能任务

需要验证 Microsoft Sentinel 部署是否满足以下要求：

- 通过 AMA 连接器配置 Windows 安全事件，以便仅从名为 VM1 的虚拟机收集所有安全事件。
- 创建近实时 (NRT) 查询规则，根据以下查询生成事件。

```KQL
SecurityEvent 
| where EventID == 4732
| where TargetAccount == "Builtin\\Administrators"
```

- 创建一个自动化规则，为 NRT 规则生成的事件分配 Operator1 所有者角色。

## 练习说明

>**注意**：在以下任务中，若要访问 `Microsoft Sentinel`，请选择 `workspace` 在实验室 01 中创建的。

### 任务 1 - 在 Microsoft Sentinel 中配置数据收集规则（DCR）

通过 AMA 连接器配置 Windows 安全事件。 了解有关通过 [AMA 连接器配置 Windows](https://learn.microsoft.com/azure/sentinel/data-connectors/windows-security-events-via-ama) 安全事件的更多信息。

 1. 在 `Microsoft Sentinel` 中，转到 `Configuration` 菜单部分并选择**数据连接器**
 1. 搜索并选择通过 **AMA 的 Windows 安全事件**
 1. 选择**打开连接器页面**
 1. 在`Configuration`区域中，选择 **+创建数据收集规则**。
 1. 在选项卡`Basics`上输入`Rule Name` 
 1. 在`Resources`选项卡上展开订阅和列中的`RG1``Scope`资源组
 1. 选择`VM1`，然后选择**下一步：收集** >。
 1. 在 `Collect` 选项卡上保留默认值 `All Security Events`
 1. 选择**下一步: 审查并创建**，然后选择**创建**。

### 任务 2 - 创建准实时 （NRT） 查询检测

在 Microsoft Sentinel 中使用准实时 (NRT) 分析规则检测威胁 了解有关 [Microsoft Sentinel 中的准实NRT规则](https://learn.microsoft.com/azure/sentinel/near-real-time-rules)的更多信息。

 1. 在 `Microsoft Sentinel` 中，转到 `Configuration` 菜单部分并选择**分析**
 1. 选择 **+ 创建**和** NRT 查询规则（预览）**
 1. 输入规则的一个`Name`，然后 `Tactics and techniques` 中选择**权限提升**。
 1. 选择**下一步：设置规则逻辑>**
 1. 在表单中 `Rule query`输入 KQL 查询

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    ```

 1. 选择**下一步：事件设置>**，然后选择**下一步：自动响应>**
 1. 选择**下一步：查看 + 创建**
 1. 完成验证后，选择**保存**。

### 任务 3 - 在 Microsoft Sentinel 中配置自动化 

配置 Microsoft Sentinel 中的自动化。 了解有关创建和使用 [Microsoft Sentinel 自动化规则](https://learn.microsoft.com/azure/sentinel/create-manage-use-automation-rules)的更多信息。

 1. 在 `Microsoft Sentinel` 中，转到 `Configuration` 菜单部分并选择**自动化**
 1. 选择 **+ 创建** ，然后选择自动化规则
 1. 输入一个 `Automation rule name`，然后从`Actions`中选择**分配所有者**
 1. 指定 **Operator1** 为所有者。
 1. 选择**应用**
