# feature-architect

`feature-architect` 是一个面向 Vue / React 前端项目的 Codex skill，用来帮助 AI 在写代码前根据需求和业务逻辑判断代码边界，避免把页面、状态、请求、表单、权限和工具函数一股脑塞进同一个文件。

它的核心目标不是“强行拆文件”，而是让 AI 在复杂度出现时先想清楚职责边界：哪些代码应该放在一起，哪些代码应该拆开，哪些既有代码需要重构，以及重构前是否需要征得用户同意。

## 适用场景

- 新增 Vue / React 页面、模块或业务功能。
- 编写复杂表单、列表详情、跨组件状态、多个 API 调用等前端逻辑。
- 判断组件、hook、composable、service、types、utils、store 的边界。
- 审查已有前端代码是否存在职责混杂的问题。
- 在可能涉及重构已有文件时，引导 AI 先向用户确认。

## 不适用场景

- 后端架构设计。
- 与当前需求无关的大规模目录重构。
- 纯机械的文件行数限制。
- 未经用户同意直接重构已有代码。

## 包结构

```text
feature-architect/
  SKILL.md
  README.md
  examples/
    react-feature-structure.md
    vue-feature-structure.md
  templates/
    completion-checklist.md
    refactor-consent.md
    split-plan.md
  docs/
    superpowers/
      plans/
      specs/
```

## 核心工作流

`SKILL.md` 是 skill 的运行入口。AI 触发该 skill 后会按以下思路工作：

1. 先查看当前项目结构，遵循已有项目约定。
2. 将任务基础复杂度判断为 `tiny`、`normal` 或 `complex`。
3. 再判断是否叠加 `refactor-involved` 标记。
4. 对复杂任务，先输出拆分方案，再写实现代码。
5. 对涉及已有代码重构的任务，先向用户确认。
6. 实现后按边界做验证，并用完成清单自检。

## 任务分级

`tiny`：局部文案、样式、字段映射、简单配置或 prop 调整。AI 可以直接修改，但不应引入新的架构。

`normal`：小组件、小 hook/composable、小 service 方法、局部 UI 交互。AI 应跟随现有目录模式，必要时创建职责明确的小文件。

`complex`：新增页面、复杂表单、列表详情、多 API、共享状态、路由、权限、缓存或跨组件协作。AI 必须先给出拆分方案，说明业务边界、文件职责、数据流、复用逻辑和验证方式。

`refactor-involved`：这是叠加标记，不是独立等级。只要需要移动既有逻辑、拆分已有组件、改 import 路径、重命名文件、抽公共 hook/composable/service 或调整 store/service 边界，都需要先征得用户同意。

## 模板说明

- `templates/split-plan.md`：复杂前端任务开始实现前使用，帮助 AI 输出拆分方案。
- `templates/refactor-consent.md`：发现已有代码需要重构时使用，帮助 AI 清楚说明原因、范围、风险和替代方案。
- `templates/completion-checklist.md`：完成前使用，检查文件职责、依赖方向、业务逻辑位置、重构授权和验证方式。

## 示例说明

- `examples/vue-feature-structure.md`：展示 Vue feature 目录如何组织页面、组件、composables、services 和 types。
- `examples/react-feature-structure.md`：展示 React feature 目录如何组织页面、组件、hooks、services 和 types。

两个示例都使用 `device-list` 作为演示业务，重点说明页面容器、展示组件、业务组件、状态逻辑和请求逻辑之间的边界。

## 使用方式

将整个目录作为 Codex skill 使用时，`SKILL.md` 会提供触发元数据和执行规则。README 仅供人类阅读，用来快速了解这个 skill 的目标、结构和维护方式。

典型触发请求：

```text
请实现一个 React 设备列表页面，包含筛选、分页、状态展示和接口请求。
```

```text
请改造这个 Vue 页面，但如果需要拆已有代码，请先告诉我为什么并征求确认。
```

## 维护原则

- 保持 `SKILL.md` 精简，让它只承载 AI 必须遵循的核心规则。
- 将示例和模板放在独立文件中，避免主 skill 过长。
- 新增规则时优先补充触发条件、边界判断或反模式，而不是加入机械行数阈值。
- 修改重构相关规则时，必须保留“先征得用户同意”的门禁。
- 修改模板后，应检查 `SKILL.md` 中的引用路径是否仍然正确。

## 相关文档

- 设计规格：[docs/superpowers/specs/2026-06-11-feature-architect-skill-design.md](docs/superpowers/specs/2026-06-11-feature-architect-skill-design.md)
- 实现计划：[docs/superpowers/plans/2026-06-11-feature-architect-skill.md](docs/superpowers/plans/2026-06-11-feature-architect-skill.md)
