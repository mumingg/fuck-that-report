# lab-experiment-report

一个用于完成和验证实验任务、并填写原始DOCX实验报告的Codex skill。

## 使用

仅在明确调用以下 skill 时使用：

```text
$lab-experiment-report
```

该 skill 会先读取实验题目和报告模板，检查并运行实验代码，在结果通过验收后填写确认的原始报告文档。完整执行约束见 [references/report-workflow.md](references/report-workflow.md)。

## 目录

- `SKILL.md`：skill入口和阶段门禁
- `references/report-workflow.md`：实验到报告的完整执行契约
- `agents/openai.yaml`：Codex界面和调用策略配置

## 许可

本仓库当前未附加开源许可证。
