# Project Capability Inventory Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend the `feature-architect` skill so AI must inspect existing frontend dependencies, UI frameworks, and local abstractions before writing Vue or React implementation code.

**Architecture:** Update the existing skill package in place. `SKILL.md` receives the mandatory project capability inventory gate, templates gain reuse-decision fields and checks, and `README.md` gets a concise human-facing explanation.

**Tech Stack:** Markdown skill documentation, Codex skill metadata, shell verification with `rg`, `sed`, `git diff`, and `git status`.

---

## File Structure

- Modify: `SKILL.md`
  - Add `Project Capability Inventory`, update workflow, add reuse rules, add UI framework priority, and extend anti-patterns.
- Modify: `templates/split-plan.md`
  - Add `Existing Capabilities And Reuse Decision` so complex plans record reuse choices.
- Modify: `templates/completion-checklist.md`
  - Add final capability reuse checks.
- Modify: `templates/refactor-consent.md`
  - Add fields for replacing duplicated local code with existing project capability.
- Modify: `README.md`
  - Add a human-facing section about avoiding repeated wheel-building.

## Task 1: Add Project Capability Inventory To Skill

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Update workflow in `SKILL.md`**

Replace the `## Workflow` list with this content:

```markdown
## Workflow

1. Inspect the existing project structure before choosing file locations.
2. Inventory existing project capabilities before writing implementation code.
3. Classify the base request size as `tiny`, `normal`, or `complex`.
4. Then decide whether `refactor-involved` applies as an additive marker. A task can be both `complex` and `refactor-involved`.
5. Follow local project conventions and existing project capabilities before applying generic structure.
6. Approval gates allow read-only exploration, listing files, checking dependencies, and checking existing structure, but require approval before writing implementation code.
7. For `complex` work, present a split plan and wait for user approval before writing implementation code. If the user already explicitly approved the split plan, execute that approved plan.
8. For `refactor-involved` work, ask for explicit user consent before changing existing structure. For `complex` + `refactor-involved` work, present the split plan and include the consent gate before implementation.
9. Implement within the approved boundary and reuse existing project capabilities where appropriate.
10. Run verification appropriate to the changed files.
11. Finish with the completion checklist.
```

- [ ] **Step 2: Add `Project Capability Inventory` section**

Insert this section after `## Workflow` and before `## Task Classification`:

```markdown
## Project Capability Inventory

Before writing frontend implementation code, inspect what the project already provides.

Check:

- `package.json` and lock files for installed libraries.
- Existing imports in related source files.
- Existing UI framework or design-system usage.
- Existing shared components.
- Existing local utilities, hooks, composables, services, stores, validators, request wrappers, and formatters.

Look for:

- Utility libraries such as lodash, lodash-es, remeda, radash, classnames, and clsx.
- Date libraries such as dayjs, date-fns, luxon, and moment.
- UI frameworks such as Ant Design, Ant Design Vue, Element Plus, MUI, Chakra UI, Naive UI, and Vuetify.
- State libraries such as Pinia, Vuex, Redux Toolkit, Zustand, Jotai, and Recoil.
- Request and data libraries such as axios, ky, fetch wrappers, React Query, TanStack Query, and SWR.
- Form and validation libraries such as antd Form, react-hook-form, Formik, vee-validate, zod, and yup.
- Local abstractions such as shared components, request clients, formatters, validators, hooks, composables, stores, and service modules.

This list is not exhaustive. Inspect the project instead of relying only on the examples above.

When the task involves utilities, UI components, requests, forms, validation, dates, state, or repeated business logic, explain the reuse decision before implementation:

- Which existing capability will be reused.
- Which existing capability will not be reused, and why.
- Whether any local implementation is intentionally simpler than reuse.
- Whether a new dependency is proposed.

Do not suggest or assume a new dependency unless existing project capabilities are insufficient and hand-writing the behavior would create clear complexity, correctness risk, or maintenance cost. Ask the user before adding or relying on a new dependency.
```

- [ ] **Step 3: Add reuse rules**

Insert this section after `## Project Capability Inventory` and before `## Task Classification`:

```markdown
## Reuse Decision Rules

### Existing capability satisfies the need

Default to reuse.

Examples:

- Use UI framework primitives for Button, Modal, Drawer, Table, Form, Input, Select, DatePicker, Tabs, Upload, Tooltip, Message, Notification, Alert, and Toast when the project already uses a framework that provides them.
- Use lodash or lodash-es for debounce, throttle, cloneDeep, groupBy, sortBy, uniqBy, isEqual, merge, and similar utility behavior when already installed and appropriate.
- Use existing request clients instead of creating a new fetch wrapper.
- Use existing form and validation libraries instead of hand-writing parallel validation flows.

### Existing capability exists but is not appropriate

You may write local code, but explain why reuse is not appropriate.

Acceptable reasons include:

- The local need is simpler than importing a library helper.
- The existing component is a primitive, but the task needs a business-specific wrapper.
- The dependency exists but is not used in the relevant area and would conflict with local conventions.
- Reuse would create more coupling than a focused local implementation.

### No existing capability exists

Default to a simple local implementation.

Suggest a new dependency only when hand-writing the behavior would create clear complexity, correctness risk, or maintenance cost. Any new dependency suggestion requires user approval before installation or code changes that assume it exists.
```

- [ ] **Step 4: Add UI framework rule**

Insert this section after `## Reuse Decision Rules` and before `## Task Classification`:

```markdown
## UI Framework Rule

If the project uses a UI framework, prioritize that framework's basic components for standard UI interactions.

Do not hand-write replacements for framework-provided primitives such as:

- Button
- Modal or Drawer
- Table
- Form and form item
- Input, Select, Checkbox, Radio, Switch
- DatePicker or TimePicker
- Tabs
- Tooltip or Popover
- Upload
- Message, Notification, Alert, or Toast

Business components may still be custom. This rule targets basic interaction primitives and design-system consistency, not domain-specific composition.
```

- [ ] **Step 5: Extend anti-patterns**

Add these bullets to the existing `## Anti-Patterns` list:

```markdown
- Writing debounce, throttle, deep clone, groupBy, date formatting, class name merging, request clients, or validation helpers before checking existing utilities.
- Hand-writing Button, Modal, Table, Form, DatePicker, Select, Tabs, Upload, Tooltip, Message, or Notification when the project already uses a UI framework that provides them.
- Creating a new request wrapper while the project already has one.
- Creating a new local component that duplicates a shared component or UI framework primitive.
- Installing or assuming a new dependency without user approval.
- Using an installed dependency blindly when a simple local implementation is clearer and the trade-off has been stated.
```

- [ ] **Step 6: Verify `SKILL.md` coverage**

Run:

```bash
rg -n "Project Capability Inventory|Reuse Decision Rules|UI Framework Rule|package.json|Existing capability satisfies the need|new dependency|Hand-writing Button|Creating a new request wrapper" SKILL.md
```

Expected:

```text
Matches show the new inventory gate, reuse decision rules, UI framework rule, dependency approval rule, and anti-patterns in SKILL.md.
```

- [ ] **Step 7: Commit**

Run:

```bash
git add SKILL.md
git commit -m "docs: add project capability inventory gate"
```

Expected:

```text
Commit succeeds and includes only SKILL.md.
```

## Task 2: Add Capability Reuse Fields To Templates

**Files:**
- Modify: `templates/split-plan.md`
- Modify: `templates/completion-checklist.md`
- Modify: `templates/refactor-consent.md`

- [ ] **Step 1: Update `templates/split-plan.md`**

In `templates/split-plan.md`, insert this section after `**Existing Pattern:**` and before `**Files To Create Or Change:**`:

```markdown
**Existing Capabilities And Reuse Decision:**

- Dependencies or local abstractions found: name relevant libraries, UI frameworks, request clients, validators, formatters, hooks, composables, stores, or shared components.
- UI framework or design-system components to reuse: name the components.
- Utility, date, request, form, validation, or state capabilities to reuse: name the capabilities.
- Capabilities intentionally not reused: explain why.
- New dependency proposed: yes or no.
- If a new dependency is proposed, ask for user approval before installing it or writing code that assumes it exists.
```

- [ ] **Step 2: Update `templates/completion-checklist.md`**

In `templates/completion-checklist.md`, add this section after `## Architecture Check` and before `## Consent Check`:

```markdown
## Capability Reuse Check

- [ ] `package.json`, lock files, or existing imports were checked before implementation.
- [ ] Existing UI framework primitives were reused where applicable.
- [ ] Existing request, form, validation, date, state, and utility libraries were reused where appropriate.
- [ ] No duplicate helper or component was created when an existing project capability fits.
- [ ] Any decision to hand-write logic instead of reusing existing capability was explained.
- [ ] No new dependency was added or assumed without user approval.
```

- [ ] **Step 3: Update `templates/refactor-consent.md`**

In `templates/refactor-consent.md`, insert this section after the `Risk:` block and before `Fallback if you do not want the refactor now:`:

```markdown
Existing project capability to reuse:

- `[library, UI framework component, shared component, request client, validator, formatter, hook, composable, service, or store]`.

Code being replaced:

- `[duplicated local helper, hand-written primitive, request wrapper, validation flow, or component]`.

Behavior or styling risks:

- `[specific risk, such as changed UI behavior, design-system differences, import path churn, or test scope]`.
```

- [ ] **Step 4: Verify template coverage**

Run:

```bash
rg -n "Existing Capabilities And Reuse Decision|Capability Reuse Check|package.json|No duplicate helper|Existing project capability to reuse|Code being replaced|Behavior or styling risks" templates
```

Expected:

```text
Matches show split-plan reuse decisions, completion reuse checks, and refactor-consent replacement fields.
```

- [ ] **Step 5: Commit**

Run:

```bash
git add templates/split-plan.md templates/completion-checklist.md templates/refactor-consent.md
git commit -m "docs: add capability reuse template checks"
```

Expected:

```text
Commit succeeds and includes only the three template files.
```

## Task 3: Document Capability Inventory In README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Add README section**

In `README.md`, insert this section after `## 适用场景` and before `## 不适用场景`:

```markdown
## 避免重复造轮子

这个 skill 会要求 AI 在写前端实现代码前先盘点项目已有能力，避免项目已经有 lodash、dayjs、classnames、请求封装、表单校验、UI 框架或本地共享组件时，仍然重复手写工具函数和基础组件。

如果项目已经使用 Ant Design、Ant Design Vue、Element Plus、MUI 等 UI 框架，AI 应优先复用 Button、Modal、Table、Form、DatePicker、Select、Tabs、Upload、Tooltip、Message/Notification 等基础交互组件。

如果 AI 选择自写逻辑而不是复用已有能力，需要先说明取舍；如果建议新增依赖，必须先征得用户同意。
```

- [ ] **Step 2: Add README maintenance note**

In the `## 维护原则` list, add this bullet:

```markdown
- 修改依赖复用规则时，应同时检查 `SKILL.md`、`templates/split-plan.md` 和 `templates/completion-checklist.md` 是否保持一致。
```

- [ ] **Step 3: Verify README coverage**

Run:

```bash
rg -n "避免重复造轮子|lodash|Ant Design|Message/Notification|新增依赖|依赖复用规则" README.md
```

Expected:

```text
Matches show the new human-facing capability inventory section and maintenance note.
```

- [ ] **Step 4: Commit**

Run:

```bash
git add README.md
git commit -m "docs: document capability reuse behavior"
```

Expected:

```text
Commit succeeds and includes only README.md.
```

## Task 4: Verify Package Consistency

**Files:**
- Read: `SKILL.md`
- Read: `templates/split-plan.md`
- Read: `templates/completion-checklist.md`
- Read: `templates/refactor-consent.md`
- Read: `README.md`

- [ ] **Step 1: Verify package coverage**

Run:

```bash
rg -n "Project Capability Inventory|Existing Capabilities And Reuse Decision|Capability Reuse Check|Existing project capability to reuse|避免重复造轮子" SKILL.md templates README.md
```

Expected:

```text
Matches appear across SKILL.md, templates/split-plan.md, templates/completion-checklist.md, templates/refactor-consent.md, and README.md.
```

- [ ] **Step 2: Verify no unfinished markers remain**

Run:

```bash
rg -n "TB[D]|TO[D]O|FIX[M]E|待[定]|未[定]|placeholde[r]|implement late[r]" SKILL.md templates README.md docs/superpowers/plans/2026-06-12-project-capability-inventory.md
```

Expected:

```text
No matches. `rg` exits with code 1 because no unfinished-marker text is present.
```

- [ ] **Step 3: Verify no unrelated tracked files changed**

Run:

```bash
git diff --stat
git status --short
```

Expected:

```text
No tracked changes remain after commits. Untracked .DS_Store or .idea files may still appear and should remain untouched.
```

- [ ] **Step 4: Commit consistency fixes if needed**

If the verification steps reveal inconsistencies that require edits, run:

```bash
git add SKILL.md templates/split-plan.md templates/completion-checklist.md templates/refactor-consent.md README.md
git commit -m "docs: align capability inventory references"
```

Expected:

```text
Commit succeeds only if consistency edits were needed. If no edits were needed, skip this commit.
```

## Task 5: Final Delivery Check

**Files:**
- Read: `SKILL.md`
- Read: `templates/split-plan.md`
- Read: `templates/completion-checklist.md`
- Read: `templates/refactor-consent.md`
- Read: `README.md`

- [ ] **Step 1: Check final git status**

Run:

```bash
git status --short --branch
```

Expected:

```text
Current branch is main. No tracked changes remain. Untracked .DS_Store or .idea files may remain untouched.
```

- [ ] **Step 2: Summarize implementation**

Prepare a final summary naming these updated files:

```text
SKILL.md
templates/split-plan.md
templates/completion-checklist.md
templates/refactor-consent.md
README.md
```

- [ ] **Step 3: Report verification evidence**

Report these verification commands:

```text
rg SKILL.md coverage for Project Capability Inventory
rg templates coverage for reuse fields
rg README coverage for avoiding repeated wheel-building
rg package consistency across SKILL.md/templates/README.md
rg unfinished-marker scan
git status --short --branch
```

- [ ] **Step 4: Mention untouched untracked files**

If still present, mention:

```text
Untracked .DS_Store and .idea files were present before implementation and were left untouched.
```
