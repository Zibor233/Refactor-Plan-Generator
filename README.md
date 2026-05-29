# Refactor Plan Generator

一个面向老项目和历史代码的 `Skill`，用于生成 `分阶段、可执行、低风险` 的重构计划。

它不会一上来直接改代码，而是先帮助开发者识别问题、拆分阶段、评估风险，并给出验证与回滚建议，适合在真正动手重构前先把路线想清楚。

## Why

在真实开发里，重构最难的部分往往不是“怎么写新代码”，而是：

- 先改哪里
- 哪些地方不能碰
- 如何拆成几次小改动
- 每一步如何验证
- 出问题后如何快速回滚

很多工具擅长“直接生成代码”，但对老代码来说，直接重写往往风险很高。

这个 Skill 的目标是先生成一份靠谱的重构计划，再帮助你更稳妥地推进实现。

## Features

- 输出结构化的重构目标、问题诊断和整体策略
- 生成分阶段执行计划，而不是一次性大改
- 提供可直接落地的 checklist
- 显式分析风险、验证方案和回滚思路
- 支持快速规划、完整规划和方案复审三种模式
- 默认强调行为保持不变和渐进式重构

## Best For

适合这些场景：

- 维护老项目或历史代码
- 模块过大、职责混杂、耦合严重
- 想重构，但缺乏清晰执行顺序
- 测试不完善，不敢直接大改
- 独立开发者希望把风险控制在可接受范围内

## Repository Structure

```text
.trae/
  skills/
    refactor-plan-generator/
      SKILL.md
README.md
```

核心 Skill 文件：

- `.trae/skills/refactor-plan-generator/SKILL.md`

## Skill Overview

这个 Skill 会优先做以下事情：

1. 分析当前代码中的主要问题
2. 明确重构目标、边界和不变约束
3. 选择适合的整体策略
4. 将重构工作拆成多个安全阶段
5. 为每个阶段补充验证方法
6. 标出高风险点和对应缓解措施
7. 给出必要时的回滚方案

默认输出内容包括：

- Refactor Objective
- Current Problems
- Refactor Strategy
- Phased Execution Plan
- Task Checklist
- Risks And Mitigations
- Validation Plan
- Rollback Plan

## Modes

### Quick Mode

适合快速判断是否值得重构，输出：

- 主要问题
- 3 到 5 个阶段
- 关键风险
- 第一步建议

### Full Planning Mode

适合准备真正实施重构时使用，输出完整计划：

- 诊断
- 分阶段方案
- 任务清单
- 验证方法
- 回滚方案

### Review Mode

适合你已经有一个重构方案，想让 Skill 帮你补充：

- 风险遗漏
- 顺序问题
- 不现实步骤
- 测试缺口

## Example Prompts

### 1. 为老模块生成完整重构计划

```text
请帮我为这个 service 类生成一个分阶段重构计划。
目标是降低耦合、提升可测试性，但不要改变现有对外行为。
当前测试不完整，调用方较多，请优先考虑低风险方案。
```

### 2. 只要快速规划

```text
不要直接改代码，先给我一个 3 到 5 个阶段的重构方案，
并标出最高风险点和第一步应该做什么。
```

### 3. 审查已有重构方案

```text
这是我现有的重构计划，请帮我 review：
1. 先拆 service
2. 再抽数据库层
3. 再迁移调用方

请重点看有没有顺序问题、遗漏风险和验证不足。
```

## Example Output

面对一个过大的 service 类，这个 Skill 可能会输出类似这样的计划：

### Phase 1: Add Safety Nets

- 补充当前核心行为的 characterization tests
- 确保重构前后关键行为一致

### Phase 2: Isolate Responsibilities

- 分离数据访问、业务规则和格式转换逻辑
- 优先抽出边界清晰、副作用明显的部分

### Phase 3: Extract Abstractions

- 为数据库访问或外部服务引入接口层
- 降低核心逻辑和底层实现的直接耦合

### Phase 4: Migrate Callers

- 按调用方分批迁移
- 必要时保留兼容层

### Phase 5: Cleanup

- 删除废弃逻辑
- 收敛命名和模块边界

## Design Principles

这个 Skill 在设计上坚持几个原则：

- `先诊断，再行动`
- `优先渐进式重构，不做大爆炸式重写`
- `默认保持当前行为不变`
- `优先输出可执行计划，而不是泛泛建议`
- `把测试、验证和回滚作为计划的一部分`

## Screenshots

你可以在这里补充演示截图：

- Skill 页面截图
- 输入示例截图
- 输出的 phased plan 截图
- 风险与验证方案截图

如果你准备参加 Skill 创作赛，建议至少展示：

1. 输入内容
2. 输出的分阶段计划
3. checklist / 风险 / 验证结果

## How To Use

1. 将 `SKILL.md` 放到你的 TRAE SOLO Skill 目录中
2. 在需要重构规划时调用 `refactor-plan-generator`
3. 提供目标代码、问题描述和约束条件
4. 根据输出的 phased plan 分阶段实施
5. 每完成一个阶段后进行验证，再进入下一阶段

## Who This Is For

- 独立开发者
- 维护存量系统的工程师
- 经常接手历史代码的人
- 想在重构前先理清执行路径的人

## Future Improvements

后续计划继续增强：

- 针对前端组件、后端服务、数据库层提供更细粒度策略
- 增加不同技术栈下的重构建议风格
- 让输出结果更容易直接转换为 issue list 或任务清单
- 增强对低测试覆盖项目的保护性建议

