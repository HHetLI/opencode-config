# Opencode Global Config

## 1. Setup

**还没有装opencode**

```sh
git clone https://github.com/yandy/agent-config.git ~/.config/opencode
npm install -g opencode-ai
```

**已经装了opencode**

```sh
cd ~/.config/opencode

git init
git branch -M main
git remote add origin https://github.com/yandy/agent-config.git
git fetch -p origin
git reset --hard origin/main
```

**add a fish abbr**

`~/.config/fish/conf.d/opencode.fish`

```fish
function opencode_ext
  echo "env OPENCODE_ENABLE_EXA=1 OPENCODE_EXPERIMENTAL_LSP_TOOL=1 opencode"
end

abbr -a opencode -f opencode_ext
```

## 2. Plugins

### 2.1 [superpowers](https://github.com/obra/superpowers)

## 3. Skills Management

### 3.1 Add skill

```sh
npx skills add <package> --skill <skills> -a opencode -y
# eg.
npx skills add anthropics/skills --skill pdf docx -a opencode -y
```

### 3.2 Update skills

```sh
npx skills update -a opencode
```

### 3.3 Remove skills

```sh
npx skills remove <skills> -a opencode
```

### 3.4 List skills

```sh
npx skills ls -a opencode
```

### 3.5 Current skills

- skill-creator

```sh
# npx skills add anthropics/skills --skill skill-creator -a opencode -y
```

- office

```sh
# npx skills add anthropics/skills --skill pdf docx xlsx pptx -a opencode -y
```

- ui/ux

```sh
# npx skills add nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max -a opencode -y
# npx skills add alchaincyf/huashu-design --skill huashu-design -a opencode -y
```

- browser automation

```sh
npm install -g playwright @playwright/cli
playwright install chromium firefox
# npx skills add microsoft/playwright-cli --skill playwright-cli -a opencode -y
```

- [context7](https://github.com/upstash/context7)

```sh
npx ctx7 login
# npx ctx7 setup --opencode
```
