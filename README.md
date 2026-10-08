[README.md](https://github.com/user-attachments/files/33204799/README.md)
# Maxwell automatic wwj

作者：抖音 萌猪过河

在唯一可见的 Ansys Maxwell 窗口中，通过官方 PyAEDT API 自动建立并求解**完整三维杯形电磁执行器的一个固定工况**：1 mm 气隙、1 A 电流、200 匝、精细网格。输出原生模型、网格、磁场、吸力、电感和可重新打开的工程。全过程不使用屏幕识别或鼠标操控。

**本版单工况已实测通过（2026-10-08）。** 真实 MCP 复跑的原生全流程约 **98.78 秒**，其中求解约 **75.91 秒**；吸力 **5.485874 N**，200 匝电感 **18.181846 mH**，网格 **51,922 个四面体**。六张原生图导出成功，保存重开后数值一致，MCP 退出后原窗口仍保留。其他电脑耗时会不同，完整证据见 [验证记录](references/validation.md)。

## 包含内容

- 单工况固定建模、求解、数值核验、保存重开和结果导出脚本。
- 单一可见桌面准备工具、会话守卫及本地 stdio MCP。
- MCP 工具：`inventory`、`start_actuator3d`、`job_status`。
- 模型假设、安装和智能体接入说明。

这是静磁场模型，材料为固定磁导率，空气域有限；不计算衔铁运动、饱和或温升。完整三维实体及真实网格可在 Maxwell 中查看，具体假设见 [模型说明](references/actuator3d.md)。

## 开始使用

1. 安装包含 Maxwell 的 Electronics Desktop 并准备可用许可；复制整个技能目录，在本机建立 Python 虚拟环境并安装 [requirements.txt](requirements.txt)。
2. 修改 Python、技能、AEDT 安装根和输出目录为本机路径，按 [接入说明](references/agent-setup.md) 使用本地 CLI 或配置 [mcp.example.json](mcp.example.json)。
3. **使用 MCP 前，先在 MCP 进程链外的独立本地终端运行 `prepare_desktop.py`，保留它准备好的唯一 Maxwell 窗口。** MCP 入口只附着现有桌面，没有窗口就拒绝启动并返回准备命令。
4. 给智能体指令：

   > 使用 Maxwell automatic wwj，在已准备的唯一可见 Maxwell 窗口中从头运行单工况三维电磁执行器，展示真实模型、网格和磁场，报告本次吸力、电感及验证结果，保留原生工程和窗口。

具备本地命令能力的智能体可以直接运行固定脚本。只支持 MCP 的智能体需要先由本机独立终端完成桌面准备。仅导入 skill、没有本地执行或连接能力，无法访问这台电脑上的 Maxwell。豆包客户端本身尚未实测。

## 仓库版与完整包

仓库版只保留本单工况案例的执行代码、技能说明、依赖和配置模板；安装 Maxwell 后可以重新运行。完整包只额外附带本案例已验收的原生工程和匹配结果，可放在 GitHub Release。源码压缩包不会自动包含 Release 附件，需要已有结果时下载完整包。

不打包 Ansys 安装程序、许可文件或 `.venv`。复制技能后在目标机器重建 Python 环境。更新时同步 `SKILL.md`、本 README 和 `agents/openai.yaml`；客户端显示旧简介时重新导入更新包或刷新客户端技能。

[安装](references/installation.md) · [智能体接入](references/agent-setup.md) · [单工况模型](references/actuator3d.md) · [验证记录](references/validation.md)
