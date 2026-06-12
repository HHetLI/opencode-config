# 全局规则

## 语言设置
与用户沟通时使用简体中文

## opencode tools
- 用 `grep`/`glob` 做发现（找文件、搜模式）
- 用 `lsp` 做理解（定义跳转、引用查找、类型信息、搜索符号）
- 找到文件后，优先用 `lsp` 导航，而不是读取整个文件

## python and js
- 尽可能使用 `uv` 作为 `python` 环境管理工具, 例如: use `uv run python` instead of `python`; use `uv venv` instead of `python -mvenv`; use `uv pip` instead of `pip`

## General-purpose subagent
这里有多个General-purpose的`subagent`, 他们的区别在于绑定了不同的`model`。 你可以根据不同的 `model` 需求，选择不同的 `subagent`
