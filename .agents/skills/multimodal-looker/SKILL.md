---
name: multimodal-looker
description: When the current model cannot directly "see" media files (images, videos, screenshots, diagrams), delegate visual analysis to a multimodal subagent. First try Read tool yourself — only invoke this skill as fallback if the model cannot interpret the visual content.
---

# multimodal-looker

当任务需要理解媒体文件的**视觉内容**，但当前模型无法直接"看到"图片/视频时，将分析任务委托给具备多模态能力的 `multimodal-looker` 子代理。

## 核心原则：有条件调用

### 第一步：尝试自己看

先用 Read 工具读取媒体文件。读取后会出现两种情况：

- **能看懂** → 直接基于视觉内容回答用户，**无需**使用本技能
- **看不懂** → 模型只会看到文件的元数据/编码数据，无法给出有意义的视觉描述 → 此时启动本技能委托给子代理

### 第二步：仅在"看不懂"时才委托

只有当确认自己无法从视觉上理解文件内容时，才使用 Task 工具将任务委托给 `multimodal-looker` 子代理。

## 适用场景

以下类型的媒体文件通常需要图/视频理解能力，如果当前模型不具备，就委托子代理：

- 图片（`.png` `.jpg` `.jpeg` `.gif` `.webp` `.svg` `.bmp` `.heic` `.tiff` 等）
- 视频（`.mp4` `.mov` `.avi` `.webm` `.mkv` 等）
- 需要分析的视觉内容：UI 截图、图表、架构图、白板图、照片、手绘草稿等

## 使用流程

```
收到涉及媒体文件的请求
        │
        ▼
用 Read 工具读取该文件
        │
   ┌────┴────┐
   ▼         ▼
 能看懂    看不懂（只有元数据/blob）
   │         │
   ▼         ▼
直接回答   Task(subagent_type="multimodal-looker")
            │
            ▼
          拿到子代理文本分析结果 → 融合回答用户
```

## 子代理调用方式

```
Task(
  description: "简短描述（3-8个中文字）",
  prompt: "详细的分析指令...",
  subagent_type: "multimodal-looker"
)
```

### Prompt 编写规范

子代理的 prompt 必须包含以下要素：

1. **文件路径** — 使用绝对路径（如 `/home/user/project/screenshot.png`），确保子代理可以访问
2. **分析目标** — 明确说明需要提取/关注什么信息
3. **任务上下文** — 用户当前在做什么，为什么需要分析该文件
4. **输出要求** — 期望的描述格式和粒度

### 子代理返回后的处理

- 子代理返回的是**文本描述**，不是原始媒体数据
- 你收到文本后，结合用户原始需求进行解读、推理和回答
- 如果一次分析不够（多个文件或复杂场景），可以分批调用子代理

## 场景示例

### 1. UI 截图分析

用户说："看看这个截图里的布局有什么问题"

```
Task(
  description: "分析UI截图布局",
  prompt: "请分析 /home/user/project/ui-screenshot.png 的UI界面。
重点关注：
1. 整体布局结构（header、sidebar、content 等区域划分）
2. 元素对齐和间距是否一致
3. 颜色、字体使用的规范性问题
4. 交互元素（按钮、输入框）的状态和可发现性
5. 任何明显的设计缺陷或可用性问题

返回结构化的分析报告。",
  subagent_type: "multimodal-looker"
)
```

### 2. 图表解读

用户说："解读一下这个数据趋势图"

```
Task(
  description: "解读数据图表",
  prompt: "请分析 /home/user/project/trend-chart.png 中的图表。
提取以下信息：
1. 图表类型（折线图/柱状图/散点图等）
2. X轴和Y轴的含义及单位
3. 数据中的关键趋势（上升/下降/拐点/异常值）
4. 图例说明（多条数据线分别代表什么）
5. 整体结论和洞察

返回结构化的分析。",
  subagent_type: "multimodal-looker"
)
```

### 3. 架构图/流程图理解

用户说："帮我理解一下这个系统架构图"

```
Task(
  description: "理解系统架构图",
  prompt: "请分析 /home/user/project/architecture.png 中的系统架构图。
重点关注：
1. 图中所有文字标注（组件、服务、模块名称）
2. 组件之间的连接关系（箭头、连线表示的数据流/调用关系）
3. 分层结构（如果有）
4. 整体架构模式和设计意图
5. 不清晰或无法确定的内容请标注为 AMBIGUOUS

返回详细的架构描述。",
  subagent_type: "multimodal-looker"
)
```

### 4. 视频内容分析

用户说："这个操作演示视频里展示了什么步骤"

```
Task(
  description: "分析操作演示视频",
  prompt: "请分析视频 /home/user/project/demo.mp4 中展示的操作流程。
描述以下内容：
1. 视频中出现的关键界面/屏幕
2. 操作者的点击/输入步骤（按时间顺序）
3. 界面状态的变化（弹窗出现、数据更新等）
4. 视频中显示的任何代码、文字或错误信息
5. 流程的最终结果

按时间线描述，标注关键时间节点。",
  subagent_type: "multimodal-looker"
)
```

### 5. 照片内容识别

用户说："这照片里是什么东西"

```
Task(
  description: "识别照片内容",
  prompt: "请详细描述 /home/user/photo.jpg 这张照片的内容。
包括：
1. 照片中的主体对象（人、物、场景）
2. 可以识别的文字、标志、标签
3. 环境/背景信息
4. 照片可能的使用场景或来源
5. 任何值得注意的细节",
  subagent_type: "multimodal-looker"
)
```

### 6. 对比多张图片

用户说："比较这两张设计稿的差异"

```
Task(
  description: "对比设计稿",
  prompt: "请对比分析以下两张设计稿的差异：
- 文件1: /home/user/project/design-v1.png
- 文件2: /home/user/project/design-v2.png

重点比较：
1. 布局变化（新增/删除/移动的元素）
2. 颜色和字体差异
3. 功能区域的变化
4. 整体视觉风格的差异
5. 哪个版本更优及原因

返回逐项对比结果。",
  subagent_type: "multimodal-looker"
)
```

## 注意事项

1. **始终使用绝对路径** — 避免子代理找不到文件
2. **一次分析一个批次** — 如果媒体文件很多（>5 个），分批调用，每批 3-5 个文件
3. **提供充分上下文** — 子代理不共享你的对话历史，需要在 prompt 中提供背景
4. **不对结果二次猜测** — 子代理的描述是其最佳判断，直接使用即可
5. **等待结果再回复** — 在子代理返回前，不要向用户展示任何分析结论
6. **将描述融入回答** — 拿到子代理的文本描述后，结合原始用户问题，用自然语言回答用户
