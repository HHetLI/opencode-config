# Oh-My-OpenCode 顶配模型配置建议（不考虑成本）

基于性能优先原则，从你提供的6个模型中选择每个场景下的最优解，充分发挥各模型的最强能力。

## 一、模型能力排名（纯性能维度）
| 排名 | 模型 | 综合能力 | 最强单项 |
|------|------|----------|----------|
| 1 | **Doubao2.0-pro** | S+ | 深度推理、数学能力、长链路任务、多模态理解 |
| 2 | **GLM 5** | S | 逻辑严谨性、知识可靠性、复杂推理 |
| 3 | **Kimi K2.5** | S | 智能体调度、超长文本、多模态转代码 |
| 4 | **Doubao2.0-code** | A+ | 编程专项、代码生成质量 |
| 5 | **Qwen3.5-plus** | A+ | 多模态均衡、高吞吐 |
| 6 | **MiniMax-M2.5** | A | 工具调用、快速编码 |

## 二、顶配 Agent 配置（性能优先）

### 核心 Agents
| Agent | 顶配模型 |  fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Sisyphus（主编排器）** | Kimi K2.5 | Kimi K2.5 → Doubao2.0-pro → GLM 5 | Kimi的智能体集群调度能力是不可替代的，支持上百个子智能体并行工作，是编排器的最佳选择 |
| **Hephaestus（深度工作者）** | Doubao2.0-code | Doubao2.0-code → GLM 5 → Kimi K2.5 | 豆包Code是专门优化的编程模型，编码质量最高，适合自主完成复杂开发任务 |
| **Oracle（架构顾问）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Kimi K2.5 | 豆包Pro推理能力最强（AIME 98.3分），对标GPT-5.2，架构决策准确性最高 |
| **Librarian（文档专家）** | GLM 5 | GLM 5 → Doubao2.0-pro → Kimi K2.5 | GLM知识可靠性最高，检索信息的准确性和权威性最好 |
| **Explore（代码探索）** | Doubao2.0-code | Doubao2.0-code → Kimi K2.5 → MiniMax-M2.5 | 代码搜索需要理解代码语义，编程专项模型理解更准确 |
| **Multimodal-Looker（多模态分析）** | Kimi K2.5 | Kimi K2.5 → Doubao2.0-pro → Qwen3.5-plus | Kimi原生多模态能力最强，截图生成代码准确率最高，视觉分析能力突出 |

### 规划 Agents
| Agent | 顶配模型 | fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Prometheus（战略规划）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Kimi K2.5 | 豆包Pro长链路推理能力最强，规划最全面，风险识别最准确 |
| **Metis（计划分析）** | GLM 5 | GLM 5 → Doubao2.0-pro → Kimi K2.5 | GLM逻辑最严谨，最擅长发现计划中的漏洞和逻辑缺陷 |
| **Momus（计划评审）** | Doubao2.0-pro | Doubao2.0-pro → GLM 5 → Kimi K2.5 | 豆包Pro综合能力最强，评审最严格，方案质量最高 |

### 编排 Agents
| Agent | 顶配模型 | fallback 链 | 配置理由 |
|-------|----------|--------------|----------|
| **Atlas（Todo编排器）** | Kimi K2.5 | Kimi K2.5 → Doubao2.0-pro → GLM 5 | Kimi任务管理能力最强，进度跟踪最准确，执行力最高 |

## 三、顶配 Category 配置（性能优先）

| Category | 顶配模型 | temperature | 配置理由 |
|----------|----------|-------------|----------|
| **visual-engineering** | Kimi K2.5 | 0.8 | Kimi多模态转代码能力最强，设计图还原度最高，UI实现效果最好 |
| **ultrabrain** | Doubao2.0-pro | 0.1 | 豆包Pro推理能力天花板，适合需要极致思考的复杂架构问题 |
| **deep** | Doubao2.0-code | 0.2 | 编程专项模型，深度编码能力最强，代码质量最高 |
| **artistry** | Doubao2.0-pro | 0.9 | 豆包Pro创造力最强，创意方案质量最高 |
| **quick** | Doubao2.0-code | 0.0 | 编程模型处理简单任务速度快、准确性高，零错误率 |
| **unspecified-low** | Qwen3.5-plus | 0.3 | 通用任务处理效率高，响应速度快 |
| **unspecified-high** | Doubao2.0-pro | 0.4 | 复杂未知任务交给能力最强的模型处理，成功率最高 |
| **writing** | Doubao2.0-pro | 0.6 | 文档生成质量最高，逻辑性和专业性最好 |

## 四、顶配配置文件示例

```json
{
  "$schema": "https://raw.githubusercontent.com/code-yeongyu/oh-my-opencode/dev/assets/oh-my-opencode.schema.json",

  "agents": {
    "sisyphus": {
      "model": "moonshotai/kimi-k2.5",
      "ultrawork": { "model": "bytedance/doubao-seed-2.0-pro", "variant": "max" },
      "thinking": {
        "type": "enabled",
        "budgetTokens": 64000
      },
      "prompt_append": "充分利用Kimi的智能体集群能力，最大程度并行调度子任务，极致提升工作效率。"
    },

    "hephaestus": {
      "model": "bytedance/doubao-seed-2.0-code",
      "thinking": {
        "type": "enabled",
        "budgetTokens": 32000
      },
      "reasoningEffort": "high",
      "prompt_append": "追求极致代码质量，充分利用Doubao Code的编程优化能力，写出最优、最简洁、最可维护的代码。"
    },

    "oracle": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "variant": "xhigh",
      "thinking": {
        "type": "enabled",
        "budgetTokens": 64000
      },
      "reasoningEffort": "high",
      "prompt_append": "作为顶级架构顾问，提供最专业、最严谨、最前沿的技术方案和架构建议。"
    },

    "librarian": {
      "model": "zhipuai/glm-5",
      "prompt_append": "提供最准确、最权威、最可靠的技术信息和文档参考，确保信息来源的正确性。"
    },

    "explore": {
      "model": "bytedance/doubao-seed-2.0-code",
      "prompt_append": "精准理解代码语义，快速定位相关代码实现，提供最相关的上下文信息。"
    },

    "multimodal-looker": {
      "model": "moonshotai/kimi-k2.5",
      "prompt_append": "精准解析视觉内容，最高质量地将设计图、截图转换为可运行代码，100%还原设计效果。"
    },

    "prometheus": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "thinking": {
        "type": "enabled",
        "budgetTokens": 64000
      },
      "reasoningEffort": "high",
      "prompt_append": "制定最全面、最详细、最具可执行性的工作计划，识别所有潜在风险，提前准备应对方案。"
    },

    "metis": {
      "model": "zhipuai/glm-5",
      "thinking": {
        "type": "enabled",
        "budgetTokens": 32000
      },
      "reasoningEffort": "high",
      "prompt_append": "最严格地审查计划的逻辑严谨性，发现所有潜在漏洞和模糊点，确保方案无懈可击。"
    },

    "momus": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "thinking": {
        "type": "enabled",
        "budgetTokens": 32000
      },
      "reasoningEffort": "high",
      "prompt_append": "最严格地评审方案和代码质量，确保符合最高行业标准和最佳实践。"
    },

    "atlas": {
      "model": "moonshotai/kimi-k2.5",
      "prompt_append": "最高效地管理和执行任务列表，精准跟踪进度，确保所有任务按时高质量完成。"
    }
  },

  "categories": {
    "visual-engineering": {
      "model": "moonshotai/kimi-k2.5",
      "temperature": 0.8,
      "reasoningEffort": "medium",
      "prompt_append": "实现最高质量的UI/UX设计，100%还原设计稿，达到像素级精准，交互体验顶级。"
    },

    "ultrabrain": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.1,
      "thinking": {
        "type": "enabled",
        "budgetTokens": 128000
      },
      "reasoningEffort": "high"
    },

    "deep": {
      "model": "bytedance/doubao-seed-2.0-code",
      "temperature": 0.2,
      "thinking": {
        "type": "enabled",
        "budgetTokens": 64000
      },
      "reasoningEffort": "high",
      "prompt_append": "编写最高质量的生产级代码，性能最优、bug最少、可维护性最强。"
    },

    "artistry": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.9,
      "prompt_append": "提供最具创意、最前沿、最惊艳的设计和解决方案。"
    },

    "quick": {
      "model": "bytedance/doubao-seed-2.0-code",
      "temperature": 0.0,
      "maxTokens": 2048,
      "prompt_append": "快速准确完成任务，零错误，最高效率。"
    },

    "unspecified-low": {
      "model": "alibaba/qwen-3.5-plus",
      "temperature": 0.3
    },

    "unspecified-high": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.4,
      "reasoningEffort": "high"
    },

    "writing": {
      "model": "bytedance/doubao-seed-2.0-pro",
      "temperature": 0.6,
      "prompt_append": "编写最专业、最清晰、最具逻辑性的技术文档和内容。"
    }
  },

  "background_task": {
    "providerConcurrency": {
      "bytedance": 10,
      "moonshotai": 8,
      "zhipuai": 5,
      "alibaba": 10,
      "minimax": 15
    },
    "modelConcurrency": {
      "bytedance/doubao-seed-2.0-pro": 5,
      "bytedance/doubao-seed-2.0-code": 8,
      "moonshotai/kimi-k2.5": 5,
      "zhipuai/glm-5": 3,
      "alibaba/qwen-3.5-plus": 10
    }
  },

  "model_fallback": {
    "enabled": true,
    "global_fallback_chain": [
      "bytedance/doubao-seed-2.0-pro",
      "zhipuai/glm-5",
      "moonshotai/kimi-k2.5",
      "bytedance/doubao-seed-2.0-code",
      "alibaba/qwen-3.5-plus",
      "minimax/minimax-m2.5"
    ]
  },

  "performance": {
    "prefer_lower_latency": false,
    "max_parallel_tasks": 50,
    "thinking_budget_multiplier": 2.0
  }
}
```

## 五、顶配方案核心优势

1. **能力最大化**：每个角色都使用该场景下能力最强的模型，没有任何性能妥协
2. **极致效率**：Kimi作为编排器，智能体集群能力拉满，支持50+并行任务
3. **最高质量**：Doubao2.0系列负责核心推理和编码，代码质量和方案准确性达到国内顶尖水平
4. **最强逻辑**：GLM5负责知识检索和逻辑审查，确保所有方案严谨无漏洞
5. **最佳多模态**：Kimi负责视觉类任务，设计图还原度和转代码准确率最高

这个配置方案代表了当前国产大模型能达到的最高性能水平，完全不考虑成本，只为追求最好的效果和最高的效率。
