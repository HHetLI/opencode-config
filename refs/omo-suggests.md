# Oh-My-OpenCode 模型配置建议（国产模型版）

基于你提供的6个模型：**GLM 5、Kimi K2.5、Qwen3.5-plus、MiniMax-M2.5、Doubao2.0-code、Doubao2.0-pro**，结合 Oh-My-OpenCode 的架构设计和各模型的能力特点，以下是最优的 Agent 和 Category 配置方案。

## 一、模型能力总览

| 模型 | 核心优势 | 适用场景 | 能力等级 |
|------|----------|----------|----------|
| **Doubao2.0-pro** | 深度推理、长链路任务、多模态理解、数学能力顶尖（AIME 98.3分），价格仅为GPT-5.2的1/6 | 架构决策、复杂问题分析、规划类任务 | S+ |
| **GLM 5** | 逻辑推理、知识可靠性、数学能力强（AIME 92.7分），国产芯片适配好 | 代码逻辑分析、复杂推理、架构评审 | S |
| **Kimi K2.5** | 超长文本处理、智能体集群调度、多模态能力强，支持截图生成代码 | 项目管理、任务编排、长文档分析 | S |
| **Qwen3.5-plus** | 高性价比、多模态能力均衡、代理任务表现优秀 | 通用任务、多模态处理、成本敏感场景 | A+ |
| **Doubao2.0-code** | 专为编程场景优化，与TRAE深度集成，代码生成质量高 | 代码编写、调试、重构 | A+ |
| **MiniMax-M2.5** | 编码能力强（SWE-Bench 80.2%）、工具调用优秀、速度快价格低 | 代码搜索、快速实现、工具调用类任务 | A |

## 二、Agent 配置建议

### 核心 Agents
| Agent | 推荐模型 |  fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Sisyphus（主编排器）** | Kimi K2.5 | Kimi K2.5 → GLM 5 → Doubao2.0-pro | Kimi擅长智能体调度和多任务协调，完美匹配Sisyphus的编排器角色，支持上百个子智能体并行工作 |
| **Hephaestus（深度工作者）** | Doubao2.0-code | Doubao2.0-code → MiniMax-M2.5 → Qwen3.5-plus | 豆包Code专为编程优化，适合自主完成复杂编码任务，无需过多干预 |
| **Oracle（架构顾问）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Qwen3.5-plus | 豆包Pro推理能力顶尖，对标GPT-5.2，适合做架构决策和代码评审 |
| **Librarian（文档专家）** | MiniMax-M2.5 | MiniMax-M2.5 → Qwen3.5-plus → Doubao2.0-code | MiniMax速度快成本低，擅长文档检索和工具调用，适合知识库查询 |
| **Explore（代码探索）** | MiniMax-M2.5 | MiniMax-M2.5 → Doubao2.0-code → Qwen3.5-plus | 优先速度和低成本，MiniMax Lightning版本可达100 TPS，适合并行代码搜索 |
| **Multimodal-Looker（多模态分析）** | Kimi K2.5 | Kimi K2.5 → Qwen3.5-plus → Doubao2.0-pro | Kimi原生多模态能力强，支持截图直接生成代码，适合UI分析和视觉调试 |

### 规划 Agents
| Agent | 推荐模型 | fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Prometheus（战略规划）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Kimi K2.5 | 豆包Pro长链路推理能力强，适合做复杂任务规划和需求访谈 |
| **Metis（计划分析）** | GLM 5 | GLM 5 → Doubao2.0-pro → Kimi K2.5 | GLM逻辑严谨，擅长发现计划中的漏洞和模糊点 |
| **Momus（计划评审）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Kimi K2.5 | 豆包Pro推理能力顶尖，能严格验证计划的完整性和可行性 |

### 编排 Agents
| Agent | 推荐模型 | fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Atlas（Todo编排器）** | Kimi K2.5 | Kimi K2.5 → Qwen3.5-plus → MiniMax-M2.5 | Kimi擅长任务管理和流程跟踪，适合按计划系统地执行任务 |

## 三、Category 配置建议

| Category | 推荐模型 | temperature | 适用场景 | 配置理由 |
|----------|----------|-------------|----------|----------|
| **visual-engineering** | Qwen3.5-plus | 0.8 | 前端开发、UI/UX设计、样式调整 | Qwen多模态能力均衡，性价比高，适合视觉类开发任务 |
| **ultrabrain** | Doubao2.0-pro | 0.2 | 深度逻辑推理、复杂架构设计、高难度问题分析 | 豆包Pro推理能力最强，对标GPT-5.2，适合需要极致思考的场景 |
| **deep** | Doubao2.0-code | 0.3 | 深度编码、复杂功能实现、系统重构 | 豆包Code专为编程优化，适合自主完成复杂开发任务 |
| **artistry** | Qwen3.5-plus | 0.9 | 创意类任务、创新方案设计、UI创意 | Qwen创造力较好，适合需要发散思维的场景 |
| **quick** | MiniMax-M2.5 | 0.1 | 简单修改、bug修复、 typo 调整 | MiniMax速度快成本低，适合快速完成 trivial 任务 |
| **unspecified-low** | Qwen3.5-plus | 0.3 | 普通复杂度任务、日常开发 | Qwen性价比高，综合能力均衡 |
| **unspecified-high** | Doubao2.0-pro | 0.4 | 高复杂度通用任务、跨模块开发 | 豆包Pro综合能力最强，适合处理复杂的未知任务 |
| **writing** | Qwen3.5-plus | 0.6 | 文档编写、技术写作、注释生成 | Qwen文本生成能力均衡，成本较低 |

## 四、完整配置文件示例

```json
{
  "$schema": "https://raw.githubusercontent.com/code-yeongyu/oh-my-opencode/dev/assets/oh-my-opencode.schema.json",

  "agents": {
    "sisyphus": {
      "model": "moonshotai/kimi-k2.5",
      "ultrawork": { "model": "bytedance/doubao-seed-2.0-pro", "variant": "max" },
      "prompt_append": "优先使用Kimi的智能体集群能力，并行调度子任务提升效率。"
    },
    
    "hephaestus": {
      "model": "bytedance/doubao-seed-2.0-code",
      "prompt_append": "专注于深度编码，充分利用Doubao Code的编程优化能力，优先保证代码质量。"
    },
    
    "oracle": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "variant": "high",
      "prompt_append": "作为架构顾问，严格审查代码设计和方案合理性，提供专业的架构建议。"
    },
    
    "librarian": {
      "model": "minimax/minimax-m2.5",
      "prompt_append": "快速检索文档和代码库，优先返回准确的参考信息和实现案例。"
    },
    
    "explore": {
      "model": "minimax/minimax-m2.5",
      "prompt_append": "高速完成代码搜索和上下文分析，优先速度和准确性。"
    },
    
    "multimodal-looker": {
      "model": "moonshotai/kimi-k2.5",
      "prompt_append": "充分利用多模态能力，精准解析图像、截图、PDF等视觉内容，提取关键信息。"
    },
    
    "prometheus": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "prompt_append": "深入理解用户需求，制定详细、可执行的工作计划，识别潜在风险点。"
    },
    
    "metis": {
      "model": "zhipuai/glm-5",
      "prompt_append": "严谨分析计划中的漏洞和模糊点，确保方案的完整性和可行性。"
    },
    
    "momus": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "prompt_append": "严格评审计划和代码，确保符合最佳实践和质量标准。"
    },
    
    "atlas": {
      "model": "moonshotai/kimi-k2.5",
      "prompt_append": "系统地管理和执行任务列表，跟踪进度，确保按时完成所有工作项。"
    }
  },

  "categories": {
    "visual-engineering": {
      "model": "alibaba/qwen-3.5-plus",
      "temperature": 0.8,
      "prompt_append": "注重UI/UX质量，实现美观、响应式的界面设计，符合现代设计标准。"
    },
    
    "ultrabrain": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.2,
      "thinking": {
        "type": "enabled",
        "budgetTokens": 32000
      },
      "reasoningEffort": "high"
    },
    
    "deep": {
      "model": "bytedance/doubao-seed-2.0-code",
      "temperature": 0.3,
      "reasoningEffort": "medium",
      "prompt_append": "深入理解代码逻辑，编写高质量、可维护的代码，遵循项目规范。"
    },
    
    "artistry": {
      "model": "alibaba/qwen-3.5-plus",
      "temperature": 0.9,
      "prompt_append": "发挥创意，提供创新的解决方案和设计思路。"
    },
    
    "quick": {
      "model": "minimax/minimax-m2.5",
      "temperature": 0.1,
      "maxTokens": 1024,
      "prompt_append": "快速完成简单任务，确保准确性，避免过度思考。"
    },
    
    "unspecified-low": {
      "model": "alibaba/qwen-3.5-plus",
      "temperature": 0.3
    },
    
    "unspecified-high": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.4,
      "reasoningEffort": "medium"
    },
    
    "writing": {
      "model": "alibaba/qwen-3.5-plus",
      "temperature": 0.6,
      "prompt_append": "编写清晰、准确、专业的技术文档和注释。"
    }
  },

  "background_task": {
    "providerConcurrency": {
      "bytedance": 4,
      "moonshotai": 3,
      "zhipuai": 2,
      "alibaba": 5,
      "minimax": 10
    },
    "modelConcurrency": {
      "bytedance/doubao-seed-2.0-pro": 2,
      "bytedance/doubao-seed-2.0-code": 3,
      "moonshotai/kimi-k2.5": 2,
      "minimax/minimax-m2.5": 15,
      "alibaba/qwen-3.5-plus": 8
    }
  },

  "model_fallback": {
    "enabled": true,
    "global_fallback_chain": [
      "bytedance/doubao-seed-2.0-pro",
      "zhipuai/glm-5",
      "moonshotai/kimi-k2.5",
      "alibaba/qwen-3.5-plus",
      "bytedance/doubao-seed-2.0-code",
      "minimax/minimax-m2.5"
    ]
  }
}
```

## 五、使用建议

1. **成本优化**：MiniMax-M2.5和Qwen3.5-plus作为低成本主力，承担80%的日常任务，将昂贵的Doubao2.0-pro和GLM 5保留给高价值的推理和架构任务。

2. **性能优先**：对于编码任务，优先使用Doubao2.0-code，其专门针对编程场景优化，代码质量更高。

3. **多模态任务**：涉及UI截图、设计图转代码等场景，使用Kimi K2.5效果最佳。

4. **并发配置**：MiniMax-M2.5并发数可以设置较高（10-15），用于并行搜索和批量处理任务。

5. ** fallback 机制**：配置全局 fallback 链，确保在某个模型服务不可用时自动切换到备用模型，保证系统稳定性。

## 六、模型ID参考（根据实际提供商调整）

| 模型 | 常见提供商ID示例 |
|------|------------------|
| GLM 5 | `zhipuai/glm-5`、`siliconflow/zhipuai-glm-5` |
| Kimi K2.5 | `moonshotai/kimi-k2.5`、`siliconflow/moonshotai-kimi-k2.5` |
| Qwen3.5-plus | `alibaba/qwen-3.5-plus`、`siliconflow/alibaba-qwen-3.5-plus` |
| MiniMax-M2.5 | `minimax/minimax-m2.5`、`siliconflow/minimax-minimax-m2.5` |
| Doubao2.0-code | `bytedance/doubao-seed-2.0-code`、`volcengine/doubao-seed-2.0-code` |
| Doubao2.0-pro | `bytedance/doubao-seed-2.0-pro`、`volcengine/doubao-seed-2.0-pro` |

请根据你实际使用的模型提供商（如硅基流动、火山引擎、阿里云百炼等）调整配置中的模型ID。
