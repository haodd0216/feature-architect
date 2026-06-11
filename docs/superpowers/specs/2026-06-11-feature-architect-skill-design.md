# Feature Architect Skill Design

## Purpose

Create a Codex skill for Vue and React frontend projects that helps the AI split code according to requirements and business logic. The skill should prevent complex features from being placed into one oversized file, while still keeping small changes direct and readable.

The skill is not a generic architecture rulebook. It guides AI behavior during real frontend work: decide the task complexity, choose sensible boundaries, explain the split when needed, and avoid refactoring existing code without user consent.

## Scope

The skill covers:

- New Vue and React frontend feature implementation.
- Existing frontend code review when a requested change exposes unclear boundaries.
- Code organization around feature or domain folders.
- Responsibility-based splitting inside each feature.
- Vue composables, React hooks, services, types, utils, stores, and presentation components.
- Templates and examples that help the AI produce consistent split plans and completion checks.

The skill does not cover:

- Backend architecture.
- Global project scaffolding decisions unrelated to the requested feature.
- Mechanical file-size limits.
- Mandatory refactoring of existing code without user approval.

## Recommended Approach

Use a graded workflow: light checks for small tasks, explicit split plans for complex tasks, and user confirmation for refactor-involved tasks.

This approach balances discipline and speed. It keeps simple changes lightweight, but forces the AI to pause before implementing features that mix page structure, state, API calls, forms, routing, permissions, or reusable business logic.

## Skill Package Structure

The final skill should be a complete package:

```text
feature-architect/
  SKILL.md
  examples/
    vue-feature-structure.md
    react-feature-structure.md
  templates/
    split-plan.md
    refactor-consent.md
    completion-checklist.md
```

## Main Skill Behavior

`SKILL.md` should define the skill as a frontend architecture assistant for Vue and React projects. It should instruct the AI to:

1. Inspect the current project structure before deciding where code belongs.
2. Classify the request as `tiny`, `normal`, `complex`, or `refactor-involved`.
3. Follow existing project conventions first.
4. Prefer feature or domain organization by default.
5. Split files by responsibility inside the feature boundary.
6. Avoid empty architecture and premature abstraction.
7. Ask for user consent before refactoring existing code.
8. Complete work with a focused architecture self-check.

## Task Classification

### Tiny

Use for local text, style, mapping, prop, or config edits. The AI can implement directly, then verify it did not add unrelated responsibility to the touched file.

### Normal

Use for a small component, hook, composable, service method, type, or local UI interaction. The AI should follow existing folder patterns and briefly mention file placement when useful.

### Complex

Use for new pages, feature modules, complex forms, list-detail flows, multiple API calls, shared state, cross-component coordination, routing, permissions, caching, or business rules that affect more than one UI unit.

For complex work, the AI must present a split plan before implementation. The plan should include:

- Business boundary.
- Proposed files and directories.
- Responsibility of each file.
- State and data flow.
- Reusable logic candidates.
- Testing or verification boundary.
- Any existing code that may need refactoring.

### Refactor-Involved

Use when the requested change touches existing code whose current structure blocks a clean implementation. The AI may identify the problem and recommend a split, but must ask the user before changing the existing structure.

The consent prompt should be direct, for example:

> I found that this file currently mixes page composition, request logic, state handling, and presentation. I recommend splitting it into a page container, feature components, a service, and a composable or hook. Do you want me to include that refactor in this change?

## Splitting Strategy

The default strategy is:

1. Organize by feature or domain first.
2. Inside each feature, split by responsibility.
3. Extract logic only when it has a clear independent purpose.
4. Keep closely related code together when splitting would reduce clarity.

Common boundaries:

- Page or route container: orchestration, layout composition, route-level state.
- Business component: feature-specific UI with domain meaning.
- Presentation component: reusable UI with props and events only.
- Hook or composable: reusable stateful behavior and interaction logic.
- Service: API calls and transport-level concerns.
- Store: shared state that truly needs shared lifecycle.
- Types: public contracts, API DTOs, form models, component-facing models.
- Utils: pure helpers with no framework state.

## Anti-Patterns

The skill should explicitly prevent:

- Putting page layout, API calls, state transitions, form rules, table columns, modal logic, and helper functions into one large component.
- Creating many tiny files that do not have independent responsibility.
- Moving code into `utils` just because it is inconvenient inside the current file.
- Adding a global store for local state.
- Hiding business logic in presentation components.
- Refactoring existing files without user consent.
- Ignoring local project conventions in favor of a generic folder structure.

## Error Handling and States

The skill should guide the AI to place error handling and UI states at appropriate boundaries:

- API and transport errors belong near services or request wrappers when reusable.
- Business errors belong near hooks, composables, stores, or feature logic.
- Loading, empty, forbidden, and validation states should be represented clearly in the page or component tree.
- Page containers assemble states; they should not become the only place where all behavior lives.

## Testing and Verification

The skill should not force tests for every small change. It should require the AI to pick verification based on the split boundary:

- Pure utils: unit tests when behavior is non-trivial.
- Hooks and composables: test state changes, side effects, and error paths when practical.
- Components: test inputs, outputs, slots, emitted events, and key interactions.
- Pages or workflows: verify the main user path and important edge states.

For complex work, the AI should state what was tested or why a test was not added.

## Templates

### Split Plan Template

The split-plan template should help the AI answer:

- What is the business boundary?
- Which files will be created or changed?
- What is each file responsible for?
- What data flows between them?
- Which logic is reusable and which stays local?
- Is existing code being refactored?
- What verification will be performed?

### Refactor Consent Template

The refactor-consent template should help the AI ask before restructuring existing files:

- What problem was found?
- Why does it affect this change?
- What refactor is proposed?
- What is the risk of doing it now?
- What is the fallback if the user declines?

### Completion Checklist Template

The completion checklist should ask:

- Can each touched file be explained in one sentence?
- Does each extracted file have a clear owner and dependency direction?
- Did reusable logic leave page components when appropriate?
- Did presentation components stay free of business side effects?
- Did the AI avoid premature abstraction?
- Did any refactor receive user consent?
- Was verification appropriate for the changed boundary?

## Examples

### Vue Example

The Vue example should show a feature organized around a domain folder:

```text
features/device-list/
  DeviceListPage.vue
  components/
    DeviceTable.vue
    DeviceStatusTag.vue
    DeviceFilterBar.vue
  composables/
    useDeviceList.ts
  services/
    deviceListService.ts
  types.ts
```

It should explain that the page coordinates layout and feature state, the composable owns list behavior, the service owns requests, and components render focused UI.

### React Example

The React example should show the same idea in React terms:

```text
features/device-list/
  DeviceListPage.tsx
  components/
    DeviceTable.tsx
    DeviceStatusBadge.tsx
    DeviceFilterBar.tsx
  hooks/
    useDeviceList.ts
  services/
    deviceListService.ts
  types.ts
```

It should explain equivalent responsibilities and highlight that hooks should not become hidden page components.

## Success Criteria

The skill is successful when:

- AI-generated frontend code has clear boundaries before complex implementation begins.
- New code follows existing project structure instead of imposing a generic layout.
- Complex tasks produce a concise split plan.
- Existing code refactors require explicit user consent.
- Vue and React users can learn expected structure from examples.
- The skill remains practical for daily development instead of becoming a heavy ceremony.
