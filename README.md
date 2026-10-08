[README.md](https://github.com/user-attachments/files/33205675/README.md)
# Maxwell automatic wwj

作者：抖音 萌猪过河

**面向豆包及其他智能体的 Ansys Maxwell 电磁仿真自动化 skill。** 根据用户的模型、参数和分析目标，指导智能体通过官方 PyAEDT / AEDT API 完成建模、材料与激励设置、网格、求解、结果验证和原生工程交付。全过程保持一个可见 Maxwell 主窗口，通过 API 展示模型和结果，不使用屏幕识别、鼠标点击或键盘模拟。

本项目提供可复用的接入方法、单窗口会话管理和仿真工作流程，**目前附带一个已实测的三维电磁执行器示例**。该示例用于快速验证接入、学习流程和复现结果；用户提出其他 Maxwell 任务时，智能体应按新任务建立或修改相应模型。后续会逐步增加实测案例和流程。

## 如何使用

| 需求 | 执行方式 | 当前状态 |
|---|---|---|
| 从头复现内置执行器 | 直接运行固定脚本，或调用配套 MCP | 已完成本机全流程验证 |
| 建立自己的模型、调整已有工程或分析参数影响 | 具备本地执行能力的智能体按需求编写、运行和验证官方 API 脚本，复用本包接入及会话管理方法 | 需要针对具体任务验证 |
| 仅通过本包提供的 MCP 工具运行 | 使用 `inventory`、`start_actuator3d`、`job_status` | 当前仅封装内置固定示例，尚无通用建模工具 |

新任务可参考官方 [Maxwell 2D](https://aedt.docs.pyansys.com/version/stable/API/_autosummary/ansys.aedt.core.maxwell.Maxwell2d.html) 与 [Maxwell 3D](https://aedt.docs.pyansys.com/version/stable/API/_autosummary/ansys.aedt.core.maxwell.Maxwell3d.html) 接口，按实际安装版本、许可和分析目标选择实现。**一个案例跑通证明了该流程的接入和求解能力，其他分析类型仍需分别验证。** 安装 skill 本身不提供 Maxwell 软件或许可证。

## 给智能体的指令示例

**运行自己的仿真：**

> 使用 Maxwell automatic wwj，按我提供的尺寸和线圈参数建立电磁铁模型，计算工作气隙中的磁场和衔铁吸力。通过官方 API 完成建模、网格、求解和验证，在唯一可见的 Maxwell 窗口里展示过程，并交付原生工程和结果。

**分析已有工程：**

> 使用 Maxwell automatic wwj，读取我提供的 Maxwell 工程，检查材料、激励和求解设置，在新副本中按我的要求修改并重新求解，输出带单位的结果和验证记录。

**快速检查接入：**

> 使用 Maxwell automatic wwj，完整重跑内置的单工况三维电磁执行器示例，展示模型、网格和磁场，报告吸力与电感，保留原生工程和窗口。

新任务需要的几何、材料、工况或精度目标由用户提供，或由智能体说明合理假设后建立。已有示例的尺寸、材料假设和验收阈值只适用于该示例。

## 当前附带的已验证示例

**三维杯形电磁执行器，单工况：1 mm 气隙、1 A 电流、200 匝。** 采用完整 360° 三维实体、线性磁性材料和自适应四面体网格，计算静磁场、衔铁吸力及电感。

2026-10-08 本机真实 MCP 复跑结果：原生全流程约 **98.78 秒**，其中求解约 **75.91 秒**；吸力 **5.485874 N**，电感 **18.181846 mH**，网格 **51,922 个四面体**。六张原生图导出成功，保存重开后数值一致，MCP 退出后原窗口仍保留。其他电脑耗时会不同。

该示例的静态位置、固定磁导率及有限空气域假设见 [模型说明](references/actuator3d.md)，实测环境与证据见 [验证记录](references/validation.md)。本版只保留这一套示例模型和结果；豆包客户端本身尚未直接实测。

## 安装与接入

1. 准备包含 Maxwell 的 Electronics Desktop、可用许可和本地 Python 环境，安装 [requirements.txt](requirements.txt) 中的依赖。
2. 复制完整技能目录，按 [智能体接入说明](references/agent-setup.md) 设置本机 Python、技能、AEDT 和输出路径。本地命令可以执行定制 API 脚本；配套 MCP 按其已提供的工具范围使用。
3. **使用配套 MCP 前，在 MCP 进程链外的独立本地终端运行 `prepare_desktop.py`。** 准备并保留唯一可见 Maxwell 窗口后，MCP 再附着该桌面。配置模板见 [mcp.example.json](mcp.example.json)。

仅导入 skill、没有本地命令或相应 MCP 执行能力的智能体，无法控制本机 Maxwell。只连接本包三个 MCP 工具时，可复现内置示例；其他模型需要本地 API 脚本执行能力或另行实现并验证相应 MCP 工具。

## 仓库版与完整包

- **仓库版**：通用技能说明、接入及会话管理代码、内置示例脚本、依赖、配置模板和验证记录，可从头运行示例。
- **完整包 `maxwell automatic wwj.zip`**：额外附带这一示例已验收的 `.aedt` 工程、匹配的 `.aedtresults`、数值表和原生场图。

压缩包不包含 Ansys 安装程序、许可文件或 Python 虚拟环境。GitHub 源码下载不包含独立上传到 Release 的附件；需要现成工程结果时下载完整包。更新技能时同步本 README、`SKILL.md` 和 `agents/openai.yaml`；客户端显示旧简介时重新导入更新包或刷新技能。

[技能执行说明](SKILL.md) · [安装](references/installation.md) · [智能体接入](references/agent-setup.md) · [示例模型](references/actuator3d.md) · [验证记录](references/validation.md)
