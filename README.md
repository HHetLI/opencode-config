# Opencode Global Config

## 1.Setup

```sh
cd ~/.config/opencode

git init
git branch -M main
git remote add origin https://github.com/yandy/agent-config.git
git fetch -p origin
git reset --hard origin/main
```

## 2.OMO

#### 2.1 Enable OMO
```sh
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

#### 2.2 OMO config optimization

```
学习 [features](https://github.com/code-yeongyu/oh-my-opencode/blob/dev/docs/reference/features.md) , [agents-models](https://github.com/code-yeongyu/oh-my-opencode/blob/dev/docs/guide/agent-model-matching.md) 两篇文档。
如果我现在只有 GLM 5, Kimi K2.5, Qwen3.5-plus, MiniMax-M2.5, Doubao2.0-code, Doubao2.0-pro 6个模型可以选择，请结合网上关于这6个模型的公开资料，为我生成 agents 和 categories的模型选择建议和配置
```

## 3. Skills Management

### 3.1 Add skill

```
bunx skills add <package> --skill <skills> -a opencode -y
# eg.
bunx skills add anthropics/skills --skill pdf docx -a opencode -y
```

### 3.2 Update skills

```
bunx skills update
```

### 3.3 Current skills

- `anthropics/skills --skill pdf docx xlsx pptx`
- `nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max`
- `vercel-labs/agent-skills --skill vercel-react-best-practices`

## 4. Common Tools

```sh
bun install -g agent-browser playwright
playwright install chromium firefox
```
