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
