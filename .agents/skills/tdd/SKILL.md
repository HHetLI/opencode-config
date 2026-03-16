---
name: tdd
description: 采用测试驱动开发(TDD)模式生成代码，要求在代码实现阶段使用
---

# tdd

TDD 是一种开发模式，要求在编写功能代码之前先编写测试用例。

## When to use

从头新建项目、开发新功能、修复bug、优化代码结构、重构项目等场景。
build from scratch, develop new features, fix bugs, optimize code structure, refactor projects, etc.

要求在代码实现阶段使用

## Instructions

1. 第一步：编写失败的测试用例（红）：
   - 测试文件命名：test_<功能名>.ts/py/js（与功能文件同名，前缀test_）；
   - 测试用例覆盖：核心场景、异常场景（如参数错误）、边界场景（如空值/极限值）；
   - 测试框架：TypeScript用vitest，Python用pytest，JavaScript用jest。
2. 第二步：编写功能代码（绿）：
   - 仅实现让测试用例通过的最小逻辑，不做多余优化；
   - 功能文件命名与测试文件对应（如 test_fibonacci.ts → fibonacci.ts）。
3. 第三步：重构建议（重构）：
   - 优化代码结构（如抽离常量、拆分复杂函数）；
   - 不破坏现有功能，保证测试用例仍能通过。
