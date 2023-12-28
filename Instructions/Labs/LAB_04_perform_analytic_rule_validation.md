---
lab:
  title: 练习 04：执行模拟攻击
  module: Guided Project - Perform a simulated attack to validate Analytic and Automation rules
---

>**注意**：本实验室建立在实验室 01、02 和 03 的基础上。 为了完成此实验室，你将需要一个 [Azure 订阅](https://azure.microsoft.com/free/?azure-portal=true)。 在其中具有管理访问权限。

## 一般指南

- 创建对象时，请使用默认设置，除非有不同的配置要求。
- 只有创建、删除或修改对象才能达到上述要求。 对环境进行不必要的更改可能会对您的最终成绩产生不利影响。
- 如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

我们需要验证 Microsoft Sentinel 部署是否正在接收安全事件，并从运行 Windows 的虚拟机创建事件。

## 体系结构关系图

![模拟攻击示意图 ](../Media/apl-5001-lab-diagrams-lab04.png)

## 技能任务

你需要了解如何执行模拟攻击，以验证分析和自动化规则是否创建事件并将其分配给 `Operator1` 事件。 你将对该操作执行简单的 `Privilege Escalation` 攻击 `vm1`。

## 练习说明

### 任务 1 - 执行模拟权限升级攻击

使用模拟攻击测试 Microsoft Sentinel 中的分析规则。 详细了解 [权限升级攻击模拟](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1078.003/T1078.003.md)。

1. 找到并选择 **Azure 中的 vm1** 虚拟机，然后向下滚动到 **操作** 的菜单项，然后选择**运行命令**
1. 在**运行命令**窗格中，选择**RunPowerShellScript**
1. 将以下用于模拟管理员帐户创建的命令复制到 `PowerShell Script` 窗体中，然后选择**运行**

    ```CommandPrompt
    net user theusernametoadd /add
    net user theusernametoadd ThePassword1!
    net localgroup administrators theusernametoadd /add
    ```

>**注意：** 请确保每行只有一个命令，并且可以通过更改用户名重新运行命令。

1. 在 `Output` 窗口中，应会看到 `The command completed successfully` 三次

### 任务 2 - 验证是否创建了模拟攻击事件

验证是否已创建符合分析规则和自动化条件的事件。 了解有关 [Microsoft Sentinel 事件管理](https://learn.microsoft.com/azure/sentinel/incident-investigation)的更多信息。

1. 在 `Microsoft Sentinel` 中，转到 `Threat management` 菜单部分并选择“事件”****
1. 你应该会看到一个事件，该事件与你在创建的 `NRT` 规则中配置的 `Severity` 和 `Title` 匹配
1. 选择 `Incident`，`detail` 窗格随即打开
1. `Owner` 分配应为 Operator1（从 `Automation rule` 创建），`Tactics and techniques` 应是“特权提升”（来自 `NRT` 规则）********
1. 选择“查看完整的详细信息”，查看所有 `Incident management` 功能和 `Incident actions`****
