# fuck-that-report

从实验题目解析、代码实现与结果验证，到填写原始DOCX实验报告的Codex skill。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

仓库名为`fuck-that-report`，安装目录和调用名称为`lab-experiment-report`。

## 快速开始

### 1. 安装

**方式一：让Codex安装**

在Codex中发送：

```text
$skill-installer 请从 https://github.com/mumingg/fuck-that-report 安装仓库根目录的skill，安装名称使用lab-experiment-report。
```

**方式二：手动安装（Windows PowerShell）**

需要先安装Git。在PowerShell中执行：

```powershell
$skillDir = Join-Path $env:USERPROFILE '.agents\skills\lab-experiment-report'
New-Item -ItemType Directory -Path (Split-Path $skillDir -Parent) -Force | Out-Null
git clone https://github.com/mumingg/fuck-that-report.git $skillDir
```

这是用户级安装，可在不同实验目录中使用。macOS或Linux可以执行：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/mumingg/fuck-that-report.git ~/.agents/skills/lab-experiment-report
```

如果目标目录已存在，请先检查已有skill；不要重复克隆或直接覆盖。Codex会自动检测skill变更；未出现时可重启Codex。如果此前已经安装在`~/.codex/skills/lab-experiment-report`且可以调用，无需重复安装。

### 2. 准备实验目录

在Codex中打开本次实验所在的文件夹，例如：

```text
实验三/
├── 实验题目.docx       # 也支持PDF、Markdown或TXT题目
├── 实验报告.docx       # 需要填写的原始报告模板
└── main.py             # 可选：已有代码，可被检查和复用
```

将题目要求的数据、环境说明和已有代码也放入工作目录。有多个题目或报告候选时，skill会请你选择确切文件。

### 3. 明确调用

在Codex对话中发送：

```text
$lab-experiment-report
请根据当前文件夹中的实验题目完成实验，验证代码和结果，并填写实验报告.docx。
提交材料只需要原始实验报告和源代码。
```

也可以明确指定输入和提交要求：

```text
$lab-experiment-report
题目是“实验题目.pdf”，目标报告是“实验报告.docx”。
请先检查已有main.py，复用正确实现，完成缺失内容后再填写报告。
按题目要求提交报告、源代码和结果图片。
```

该skill要求显式调用，不会仅凭“帮我写报告”等普通请求自动触发。skill会在填写过程中保留原报告；最终替换前请关闭Word中打开的目标报告。

## 它会做什么

1. 识别题目、目标报告、参考报告、现有代码和结果。
2. 读取完整题目，列出执行计划与可验证的验收条件。
3. 检查和复用已有代码，补齐缺失部分并实际运行实验。
4. 根据需要补充可核验的参数对比、过程图、指标表或公式说明。
5. 在工作副本中填写报告，检查结构并渲染所有页面进行视觉检查。
6. 通过验收后替换原始报告，并清理本次任务的内部工作目录。

完整执行约束见[references/report-workflow.md](references/report-workflow.md)。

## 环境与依赖

本仓库包含指令和配置，不包含实验脚本、报告模板或数据集。所需语言、算法库和数据由具体题目决定。

- 支持本地skill的Codex环境。
- 处理DOCX或PDF时，需要可用的相应文档/PDF工具与工作流；在Codex中对应`documents`和`pdf`等skill。
- 完成DOCX视觉验收时，需要文档渲染工具和LibreOffice。工作流会检查PATH中的`soffice`，或Windows常见安装路径。
- 如果无法运行实验或渲染报告，skill会说明失败原因，并保留原报告。

安装skill不会自动安装这些外部依赖。

## 当前固定规则

以下规则已经写入skill，使用前请确认符合自己的实验要求：

- 实验目的从题目中逐字复制，保留原文标点和空格。
- 实验地点固定为`实训楼508`；如需其他地点，请同时修改[SKILL.md](SKILL.md)和[执行契约](references/report-workflow.md)中的相关规则。
- 新写的中文说明不在中文与相邻英文或数字之间插入空格。
- 只使用实际运行验证过的结果，不编造日期、成绩或实验事实。
- 报告最终写回原始DOCX路径，不另存用户可见的“最终版”。
- 默认不生成ZIP；只有当前请求明确覆盖此规则时才生成。
- 内部工作副本、渲染图和QA文件位于工作目录的`.lab-work/lab-experiment-report`，验收通过并替换成功后清理；提交文件由题目或用户要求决定。

## 更新

对于通过Git克隆安装且没有本地修改的用户：

```powershell
$skillDir = Join-Path $env:USERPROFILE '.agents\skills\lab-experiment-report'
git -C $skillDir pull --ff-only
```

如果你使用其他安装目录，请换成实际路径。更新前先检查并保留自己对固定规则的修改。

## 仓库结构

```text
fuck-that-report/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── report-workflow.md
├── README.md
└── LICENSE
```
