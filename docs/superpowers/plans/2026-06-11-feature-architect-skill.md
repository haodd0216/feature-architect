# Feature Architect Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a complete `feature-architect` Codex skill package that guides AI to split Vue and React frontend code by business logic and responsibility without over-engineering.

**Architecture:** The repository root is the skill package. `SKILL.md` is the entry point, `examples/` documents Vue and React feature structures, and `templates/` provides reusable split-plan, refactor-consent, and completion-checklist prompts.

**Tech Stack:** Markdown skill documentation, Codex skill metadata, shell-based content verification with `rg`, `find`, and `git diff`.

---

## File Structure

- Create: `SKILL.md`
  - Defines the skill metadata, trigger rules, task classification, split workflow, refactor-consent gate, anti-patterns, and completion checks.
- Create: `examples/vue-feature-structure.md`
  - Shows Vue feature-folder structure with clear responsibility boundaries.
- Create: `examples/react-feature-structure.md`
  - Shows React feature-folder structure with equivalent hook/component/service boundaries.
- Create: `templates/split-plan.md`
  - Provides the output format for complex frontend work before implementation.
- Create: `templates/refactor-consent.md`
  - Provides the user-confirmation prompt format for refactor-involved work.
- Create: `templates/completion-checklist.md`
  - Provides the final architecture self-check for the AI.

## Task 1: Create Skill Entry Point

**Files:**
- Create: `SKILL.md`

- [ ] **Step 1: Write `SKILL.md`**

Create `SKILL.md` with this complete content:

````markdown
---
name: feature-architect
description: Use when implementing or modifying Vue/React frontend features to decide whether code should be split by feature, responsibility, hook/composable, service, type, or component boundary before writing code.
---

# Feature Architect

Guide frontend implementation so Vue and React code is split by business logic and responsibility instead of being placed into one large file.

Use this skill before implementing frontend changes in Vue or React projects when there is any chance that the request touches page structure, component boundaries, state, API calls, forms, routing, permissions, reusable logic, or existing code organization.

## Core Principle

Keep related business code close together, but keep responsibilities separate enough that each file can be understood, changed, and tested independently.

Do not split for decoration. Split when a boundary has a clear purpose.

## Workflow

1. Inspect the existing project structure before choosing file locations.
2. Classify the request as `tiny`, `normal`, `complex`, or `refactor-involved`.
3. Follow local project conventions before applying generic structure.
4. For `complex` work, present a split plan and wait for user approval before implementation.
5. For `refactor-involved` work, ask for explicit user consent before changing existing structure.
6. Implement within the approved boundary.
7. Run verification appropriate to the changed files.
8. Finish with the completion checklist.

## Task Classification

### tiny

Use for local text, style, mapping, prop, config, or simple field edits.

Behavior:
- Implement directly.
- Keep the edit local.
- Do not introduce new architecture.
- Before finishing, check that the touched file did not gain an unrelated responsibility.

### normal

Use for a small component, hook, composable, service method, type, or local UI interaction.

Behavior:
- Follow existing folder patterns.
- Create a focused file only when it has a clear responsibility.
- Briefly mention file placement when it matters.
- Avoid adding global state or shared utilities for local behavior.

### complex

Use for new pages, feature modules, complex forms, list-detail flows, multiple API calls, shared state, cross-component coordination, routing, permissions, caching, or business rules that affect more than one UI unit.

Behavior:
- Present a split plan before implementation.
- Include business boundary, proposed files, responsibility of each file, data flow, reusable logic candidates, verification plan, and possible refactor impact.
- Wait for user approval before writing code.

Use `templates/split-plan.md` as the output format.

### refactor-involved

Use when existing code structure blocks a clean implementation or when the requested change would be safer with a targeted split of existing files.

Behavior:
- Explain the structural problem.
- Explain why it affects this request.
- Propose the smallest useful refactor.
- Ask the user whether to include the refactor.
- If the user declines, implement the smallest local change that does not worsen the structure.

Use `templates/refactor-consent.md` as the consent format.

## Default Splitting Strategy

1. Organize by feature or domain first.
2. Inside the feature, split by responsibility.
3. Extract logic only when it has a clear independent purpose.
4. Keep closely related code together when splitting would reduce clarity.

Common boundaries:

- Page or route container: route-level orchestration, layout composition, query params, and high-level state assembly.
- Business component: feature-specific UI with domain meaning.
- Presentation component: reusable UI with props and events only.
- Hook or composable: reusable stateful behavior, derived state, side effects, and interaction logic.
- Service: API calls, request mapping, and transport-level concerns.
- Store: shared state that truly needs shared lifecycle across screens or distant components.
- Types: API DTOs, form models, component contracts, and domain-facing models.
- Utils: pure helpers with no framework state and no hidden side effects.

## Vue Guidance

Prefer feature folders for business modules when the project allows it.

Typical Vue boundaries:
- `*.vue` page files compose layout and connect feature units.
- `components/` contains feature components and presentation components.
- `composables/` contains reusable stateful behavior such as `useDeviceList`.
- `services/` contains request functions and API mapping.
- `types.ts` contains local feature contracts.
- `stores/` is used only for shared state that outlives one component tree.

Reference: `examples/vue-feature-structure.md`

## React Guidance

Prefer feature folders for business modules when the project allows it.

Typical React boundaries:
- `*Page.tsx` files compose layout and connect feature units.
- `components/` contains feature components and presentation components.
- `hooks/` contains reusable stateful behavior such as `useDeviceList`.
- `services/` contains request functions and API mapping.
- `types.ts` contains local feature contracts.
- `stores/` or context is used only when state must be shared beyond local component composition.

Reference: `examples/react-feature-structure.md`

## Error Handling and UI States

Place error handling and UI states at the boundary where they belong:

- API and transport errors belong near services or shared request wrappers when reusable.
- Business errors belong near hooks, composables, stores, or feature logic.
- Loading, empty, forbidden, and validation states should be represented explicitly.
- Page containers assemble states, but should not become the only place where all behavior lives.

## Anti-Patterns

Avoid:

- Putting page layout, API calls, state transitions, form rules, table columns, modal logic, and helper functions into one component.
- Creating many tiny files that do not have independent responsibility.
- Moving code into `utils` just because it is inconvenient inside the current file.
- Adding a global store for local state.
- Hiding business logic in presentation components.
- Refactoring existing files without user consent.
- Ignoring local project conventions in favor of a generic folder structure.
- Creating folders that contain only one unclear file and no stable responsibility.

## Completion Checklist

Before finishing, check:

- Can each touched file be explained in one sentence?
- Does each extracted file have a clear owner and dependency direction?
- Did reusable logic leave page components when appropriate?
- Did presentation components stay free of business side effects?
- Did the implementation avoid premature abstraction?
- Did any refactor receive user consent?
- Was verification appropriate for the changed boundary?

Use `templates/completion-checklist.md` for the full checklist.
````

- [ ] **Step 2: Verify metadata and required rules exist**

Run:

```bash
rg -n "name: feature-architect|description:|complex|refactor-involved|templates/split-plan.md|templates/refactor-consent.md|examples/vue-feature-structure.md|examples/react-feature-structure.md" SKILL.md
```

Expected:

```text
SKILL.md contains matches for skill metadata, complex classification, refactor-involved classification, both templates, and both examples.
```

- [ ] **Step 3: Commit**

Run:

```bash
git add SKILL.md
git commit -m "feat: add feature architect skill entry"
```

Expected:

```text
Commit succeeds and includes only SKILL.md.
```

## Task 2: Add Vue and React Examples

**Files:**
- Create: `examples/vue-feature-structure.md`
- Create: `examples/react-feature-structure.md`

- [ ] **Step 1: Write Vue example**

Create `examples/vue-feature-structure.md` with this complete content:

````markdown
# Vue Feature Structure Example

Use this example when a Vue feature has page composition, data loading, filters, table or card rendering, and business-specific display logic.

## Recommended Structure

```text
features/device-list/
  DeviceListPage.vue
  components/
    DeviceFilterBar.vue
    DeviceStatusTag.vue
    DeviceTable.vue
  composables/
    useDeviceList.ts
  services/
    deviceListService.ts
  types.ts
```

## Responsibilities

`DeviceListPage.vue`

- Owns route-level layout.
- Connects `useDeviceList` to feature components.
- Decides which high-level state is shown: loading, empty, error, or content.
- Does not contain request mapping, table row normalization, or status text rules.

`components/DeviceFilterBar.vue`

- Renders filter controls.
- Receives current filter values through props.
- Emits filter changes.
- Does not fetch data.

`components/DeviceTable.vue`

- Renders the list of devices.
- Receives rows and event handlers through props.
- Does not know request details.

`components/DeviceStatusTag.vue`

- Renders one status value.
- Encapsulates visual mapping from domain status to label and color.
- Does not mutate feature state.

`composables/useDeviceList.ts`

- Owns list state, filters, pagination, loading, and error state.
- Calls `deviceListService`.
- Exposes a small interface to the page.
- Keeps reusable behavior out of the page file.

`services/deviceListService.ts`

- Owns API calls and request or response mapping.
- Does not import Vue components.
- Does not control UI state.

`types.ts`

- Defines `DeviceListItem`, `DeviceListFilters`, request params, response models, and component-facing contracts.

## Bad Shape

Avoid a single `DeviceListPage.vue` that contains:

- Template for the whole page.
- API calls.
- Filter state.
- Pagination state.
- Table columns.
- Modal state.
- Status color mapping.
- Response normalization.
- Form validation.

That shape makes the page hard to scan and makes future changes risky.

## Good Split Signal

Split when a piece of code can answer all three questions:

- What does it do?
- How do other files use it?
- What does it depend on?

If those answers are unclear, the split probably needs a better boundary or should stay local.
````

- [ ] **Step 2: Write React example**

Create `examples/react-feature-structure.md` with this complete content:

````markdown
# React Feature Structure Example

Use this example when a React feature has page composition, data loading, filters, table or card rendering, and business-specific display logic.

## Recommended Structure

```text
features/device-list/
  DeviceListPage.tsx
  components/
    DeviceFilterBar.tsx
    DeviceStatusBadge.tsx
    DeviceTable.tsx
  hooks/
    useDeviceList.ts
  services/
    deviceListService.ts
  types.ts
```

## Responsibilities

`DeviceListPage.tsx`

- Owns route-level layout.
- Connects `useDeviceList` to feature components.
- Decides which high-level state is shown: loading, empty, error, or content.
- Does not contain request mapping, table row normalization, or status text rules.

`components/DeviceFilterBar.tsx`

- Renders filter controls.
- Receives current filter values through props.
- Calls callback props for filter changes.
- Does not fetch data.

`components/DeviceTable.tsx`

- Renders the list of devices.
- Receives rows and event handlers through props.
- Does not know request details.

`components/DeviceStatusBadge.tsx`

- Renders one status value.
- Encapsulates visual mapping from domain status to label and color.
- Does not mutate feature state.

`hooks/useDeviceList.ts`

- Owns list state, filters, pagination, loading, and error state.
- Calls `deviceListService`.
- Exposes a small interface to the page.
- Keeps reusable behavior out of the page file.

`services/deviceListService.ts`

- Owns API calls and request or response mapping.
- Does not import React components.
- Does not control UI state.

`types.ts`

- Defines `DeviceListItem`, `DeviceListFilters`, request params, response models, and component-facing contracts.

## Bad Shape

Avoid a single `DeviceListPage.tsx` that contains:

- JSX for the whole page.
- API calls.
- Filter state.
- Pagination state.
- Table columns.
- Modal state.
- Status color mapping.
- Response normalization.
- Form validation.

That shape makes the page hard to scan and makes future changes risky.

## Good Split Signal

Split when a piece of code can answer all three questions:

- What does it do?
- How do other files use it?
- What does it depend on?

If those answers are unclear, the split probably needs a better boundary or should stay local.
````

- [ ] **Step 3: Verify example coverage**

Run:

```bash
rg -n "features/device-list|useDeviceList|service|types.ts|Bad Shape|Good Split Signal" examples
```

Expected:

```text
Both example files include the recommended feature structure, responsibility sections, bad shape warning, and split signal.
```

- [ ] **Step 4: Commit**

Run:

```bash
git add examples/vue-feature-structure.md examples/react-feature-structure.md
git commit -m "docs: add frontend split examples"
```

Expected:

```text
Commit succeeds and includes only the two example files.
```

## Task 3: Add Reusable Templates

**Files:**
- Create: `templates/split-plan.md`
- Create: `templates/refactor-consent.md`
- Create: `templates/completion-checklist.md`

- [ ] **Step 1: Write split-plan template**

Create `templates/split-plan.md` with this complete content:

````markdown
# Split Plan Template

Use this before implementing `complex` Vue or React frontend work.

## Split Plan

**Task Level:** complex

**Business Boundary:**  
Describe the feature or domain this change belongs to in one or two sentences.

**Existing Pattern:**  
Name the local project pattern being followed, such as feature folders, route folders, colocated components, shared services, or module stores.

**Files To Create Or Change:**

| File | Responsibility | Notes |
| --- | --- | --- |
| `path/to/file` | One clear responsibility | Why this file belongs here |

**State And Data Flow:**

1. Describe where data is loaded.
2. Describe where state is stored.
3. Describe which components receive props or emit events.
4. Describe where errors, loading, empty, and forbidden states are handled.

**Reusable Logic Candidates:**

- Keep local: name logic that should remain near the page or component.
- Extract: name logic that has a stable independent purpose.
- Do not extract: name logic that would become premature abstraction.

**Refactor Impact:**

- Existing code refactor needed: yes or no.
- If yes, ask for user consent before changing structure.

**Verification Plan:**

- Type check, unit test, component test, local manual path, or focused command.
- Explain why this verification matches the changed boundary.
````

- [ ] **Step 2: Write refactor-consent template**

Create `templates/refactor-consent.md` with this complete content:

````markdown
# Refactor Consent Template

Use this when existing Vue or React code should be restructured to implement the requested change cleanly.

## Consent Prompt

I found that `[file or area]` currently mixes `[responsibility A]`, `[responsibility B]`, and `[responsibility C]`.

This affects the requested change because `[specific reason tied to the current task]`.

I recommend the smallest useful refactor:

- Move `[logic]` to `[target file]`.
- Keep `[logic]` in `[current file]`.
- Preserve existing behavior and public interfaces.

Risk:

- `[specific risk, such as touching a shared component, changing import paths, or increasing test scope]`.

Fallback if you do not want the refactor now:

- I can implement the requested change locally in `[file]` and avoid broad restructuring, but I will keep the new code isolated as much as possible.

Do you want me to include this refactor in the change?
````

- [ ] **Step 3: Write completion-checklist template**

Create `templates/completion-checklist.md` with this complete content:

````markdown
# Completion Checklist

Use this before finalizing Vue or React frontend work.

## Architecture Check

- [ ] Each touched file can be explained in one sentence.
- [ ] Each extracted file has a clear owner and dependency direction.
- [ ] Business logic is not hidden inside presentation components.
- [ ] Page or route containers do not own every detail of requests, state, rendering, and helpers.
- [ ] Services do not import UI components.
- [ ] Hooks or composables expose focused interfaces instead of becoming hidden page components.
- [ ] Shared stores are used only for state that truly needs shared lifecycle.
- [ ] `utils` contains pure helpers, not displaced business logic.
- [ ] The implementation follows local project conventions.
- [ ] The implementation avoids premature abstraction.

## Consent Check

- [ ] Existing code was not refactored without explicit user consent.
- [ ] If refactor was declined, the local change did not make the existing structure meaningfully worse.

## Verification Check

- [ ] Verification matches the changed boundary.
- [ ] Pure helpers with non-trivial behavior were tested or manually checked with concrete inputs.
- [ ] Hooks or composables with stateful behavior were tested or manually exercised through their consumers.
- [ ] Components were checked for key props, events, slots, callbacks, or interactions.
- [ ] Page workflows were checked for loading, empty, error, and success states when relevant.
````

- [ ] **Step 4: Verify template coverage**

Run:

```bash
rg -n "Business Boundary|Refactor Impact|Do you want me to include this refactor|Existing code was not refactored|Verification matches" templates
```

Expected:

```text
The templates include split planning, refactor consent, and completion verification language.
```

- [ ] **Step 5: Commit**

Run:

```bash
git add templates/split-plan.md templates/refactor-consent.md templates/completion-checklist.md
git commit -m "docs: add feature architect templates"
```

Expected:

```text
Commit succeeds and includes only the three template files.
```

## Task 4: Verify Skill Package Consistency

**Files:**
- Modify: `SKILL.md` only if verification finds an inconsistent reference.
- Modify: `examples/vue-feature-structure.md` only if verification finds an inconsistent term.
- Modify: `examples/react-feature-structure.md` only if verification finds an inconsistent term.
- Modify: `templates/split-plan.md` only if verification finds an unclear required field.
- Modify: `templates/refactor-consent.md` only if verification finds an unclear consent gate.
- Modify: `templates/completion-checklist.md` only if verification finds a missing completion check.

- [ ] **Step 1: Verify package files exist**

Run:

```bash
find . -maxdepth 3 -type f | sort
```

Expected:

```text
./README.md
./SKILL.md
./docs/superpowers/plans/2026-06-11-feature-architect-skill.md
./docs/superpowers/specs/2026-06-11-feature-architect-skill-design.md
./examples/react-feature-structure.md
./examples/vue-feature-structure.md
./templates/completion-checklist.md
./templates/refactor-consent.md
./templates/split-plan.md
```

- [ ] **Step 2: Verify no unfinished markers remain**

Run:

```bash
rg -n "TB[D]|TO[D]O|FIX[M]E|待[定]|未[定]|placeholde[r]|implement late[r]" SKILL.md examples templates docs/superpowers/plans/2026-06-11-feature-architect-skill.md
```

Expected:

```text
No matches. `rg` exits with code 1 because no unfinished-marker text is present.
```

- [ ] **Step 3: Verify consent gate is present**

Run:

```bash
rg -n "user consent|explicit user consent|Do you want me to include this refactor|Refactor Consent" SKILL.md templates/refactor-consent.md templates/completion-checklist.md
```

Expected:

```text
Matches appear in SKILL.md and both relevant templates, proving refactor-involved work requires confirmation.
```

- [ ] **Step 4: Verify Vue and React coverage**

Run:

```bash
rg -n "Vue|React|composables|hooks|DeviceListPage|useDeviceList" SKILL.md examples
```

Expected:

```text
Matches appear for Vue, React, composables, hooks, page examples, and useDeviceList examples.
```

- [ ] **Step 5: Review final diff**

Run:

```bash
git diff --stat
git diff -- SKILL.md examples templates
```

Expected:

```text
Diff contains only the skill entry, examples, and templates. Existing unrelated files are unchanged.
```

- [ ] **Step 6: Commit consistency fixes if any were needed**

If Step 1 through Step 5 required edits, run:

```bash
git add SKILL.md examples templates
git commit -m "docs: align feature architect skill package"
```

Expected:

```text
Commit succeeds only if consistency edits were made. If no edits were needed, skip this commit.
```

## Task 5: Final Delivery Check

**Files:**
- Read: `SKILL.md`
- Read: `examples/vue-feature-structure.md`
- Read: `examples/react-feature-structure.md`
- Read: `templates/split-plan.md`
- Read: `templates/refactor-consent.md`
- Read: `templates/completion-checklist.md`

- [ ] **Step 1: Check git status**

Run:

```bash
git status --short
```

Expected:

```text
Only user-owned unrelated files may remain untracked, such as .DS_Store. Skill package files should be committed or intentionally staged for final commit.
```

- [ ] **Step 2: Summarize created package**

Prepare a concise final summary that names:

```text
SKILL.md
examples/vue-feature-structure.md
examples/react-feature-structure.md
templates/split-plan.md
templates/refactor-consent.md
templates/completion-checklist.md
```

- [ ] **Step 3: Report verification**

Report the verification commands that passed:

```text
rg metadata and rule coverage in SKILL.md
rg example coverage in examples/
rg template coverage in templates/
find package structure
rg unfinished-marker scan
rg consent gate
rg Vue/React coverage
git diff review
```

- [ ] **Step 4: Mention unrelated working tree state**

If `.DS_Store` is still untracked, mention:

```text
Untracked .DS_Store was present before implementation and was left untouched.
```
