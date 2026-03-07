# Opencode with OMO(Oh-My-Opencode)

## Setup

### 1.还没安装 `opencode`

```sh
git clone https://github.com/yandy/opencode-omo.git ~/.config/opencode
```

### 2.已经安装了 `opencode`
```sh
cd ~/.config/opencode

git init
git branch -M main
git remote add origin https://github.com/yandy/agent-config.git
git fetch -p origin
git reset --hard origin/main
```

## Enable OMO

```json
# 增加项目配置
cat > .opencode/opencode.json <<- 'EOM'
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "oh-my-opencode@latest"
  ]
}
EOM
```

## OMO config optimize

```
学习 [features](https://github.com/code-yeongyu/oh-my-opencode/blob/dev/docs/reference/features.md) , [agents-models](https://github.com/code-yeongyu/oh-my-opencode/blob/dev/docs/guide/agent-model-matching.md) 两篇文档。
如果我现在只有 GLM 5, Kimi K2.5, Qwen3.5-plus, MiniMax-M2.5, Doubao2.0-code, Doubao2.0-pro 6个模型可以选择，请结合网上关于这6个模型的公开资料，为我生成 agents 和 categories的模型选择建议和配置
```
