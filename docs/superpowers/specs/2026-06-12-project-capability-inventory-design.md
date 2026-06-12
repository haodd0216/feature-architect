# Project Capability Inventory Design

## Purpose

Extend the `feature-architect` skill so AI must inspect existing frontend project capabilities before writing Vue or React implementation code. The goal is to prevent repeated wheel-building: hand-written utility functions, duplicated request helpers, and custom basic UI components when the project already provides libraries, UI frameworks, or local abstractions.

This extension should keep the existing architecture-splitting behavior, but add a stronger pre-implementation gate: understand what the project already has before deciding what to build.

## Problem

AI often writes its own helper functions or basic UI elements even when a project already has tools such as lodash, dayjs, classnames, Ant Design, Ant Design Vue, Element Plus, MUI, local request wrappers, validators, or shared components.

That creates several risks:

- Duplicate utilities with inconsistent behavior.
- UI that does not match the project's design system.
- Extra maintenance burden for code the project already solved.
- More bugs in date handling, deep cloning, debounce, form validation, table behavior, modal behavior, and request handling.
- Architecture plans that split files well but still ignore existing dependencies.

## Scope

Update the existing skill package without adding new top-level resources.

Files to modify:

- `SKILL.md`
- `templates/split-plan.md`
- `templates/completion-checklist.md`
- `templates/refactor-consent.md`
- `README.md`

Files not required:

- No new template file.
- No scripts.
- No package installation.

## Design Summary

Add a mandatory `Project Capability Inventory` step to the skill workflow.

Before writing frontend implementation code, AI must inspect:

- `package.json` and lock files.
- Existing imports in related source files.
- Existing UI framework usage.
- Existing local components.
- Existing local utilities, hooks, composables, services, stores, validators, request wrappers, and formatters.

AI must then explain reuse decisions before implementation when the task involves utilities, UI components, requests, forms, validation, dates, state, or repeated business logic.

## Workflow Changes

The `SKILL.md` workflow should be updated so project capability inventory happens before task classification or before selecting implementation boundaries.

Recommended workflow:

1. Inspect current project structure.
2. Inventory existing project capabilities.
3. Classify the base request size as `tiny`, `normal`, or `complex`.
4. Decide whether `refactor-involved` applies as an additive marker.
5. Follow local project conventions before applying generic structure.
6. Present split plans and consent gates where required.
7. Implement within approved and existing-capability-aware boundaries.
8. Verify architecture and reuse decisions before finishing.

## Inventory Targets

The skill should name common capability categories so AI knows what to look for:

- Utility libraries: lodash, lodash-es, remeda, radash, classnames, clsx.
- Date libraries: dayjs, date-fns, luxon, moment.
- UI frameworks: Ant Design, Ant Design Vue, Element Plus, MUI, Chakra UI, Naive UI, Vuetify.
- State libraries: Pinia, Vuex, Redux Toolkit, Zustand, Jotai, Recoil.
- Request and data libraries: axios, ky, fetch wrappers, React Query, TanStack Query, SWR.
- Form and validation libraries: antd Form, react-hook-form, Formik, vee-validate, zod, yup.
- Local abstractions: shared components, local request clients, formatters, validators, hooks, composables, stores, and service modules.

This list is not exhaustive. AI should inspect the project instead of relying only on this list.

## Reuse Decision Rules

### Existing capability satisfies the need

Default to reuse.

Examples:

- Use Ant Design or Ant Design Vue Button, Modal, Table, Form, DatePicker, Select, Tabs, Upload, Tooltip, and Message/Notification instead of hand-writing basic UI primitives.
- Use lodash or lodash-es for debounce, throttle, cloneDeep, groupBy, sortBy, uniqBy, isEqual, merge, and similar utility behavior when already installed and appropriate.
- Use existing request clients instead of creating a new fetch wrapper.
- Use existing form and validation libraries instead of hand-writing parallel validation flows.

### Existing capability exists but is not appropriate

AI may write local code, but must explain why reuse is not appropriate.

Acceptable reasons include:

- The local need is simpler than importing a library helper.
- The existing component is a primitive, but the task needs a business-specific wrapper.
- The project dependency exists but is not used in the relevant area and would conflict with local conventions.
- Reuse would create more coupling than a focused local implementation.

### No existing capability exists

Default to a simple local implementation.

AI may suggest a new dependency only when hand-writing the behavior would create clear complexity, correctness risk, or maintenance cost. Any new dependency suggestion requires user approval before installation or code changes that assume it exists.

## UI Framework Rule

If the project uses a UI framework, AI must prioritize that framework's basic components for standard UI interactions.

AI should not hand-write replacements for framework-provided primitives such as:

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

Business components may still be custom. The rule targets basic interaction primitives and design-system consistency, not domain-specific composition.

## Template Changes

### split-plan.md

Add a section named `Existing Capabilities And Reuse Decision`.

It should require:

- Relevant dependencies or local abstractions found.
- UI framework or design-system components to reuse.
- Utility functions or libraries to reuse.
- Capabilities intentionally not reused and the reason.
- Whether a new dependency is proposed.
- If a new dependency is proposed, confirmation that user approval is required.

### completion-checklist.md

Add capability reuse checks:

- Checked `package.json`, lock files, or existing imports before implementation.
- Reused existing UI framework primitives where applicable.
- Reused existing request, form, validation, date, and utility libraries where appropriate.
- Did not create a duplicate helper or component when an existing project capability fits.
- Explained any decision to hand-write logic instead of reusing existing capability.
- Did not add or assume a new dependency without user approval.

### refactor-consent.md

Add a short prompt field for refactors that replace duplicated local code with existing project capability:

- Existing project capability to reuse.
- Code being replaced.
- Behavior and styling risks.
- Fallback if the user declines.

## README Changes

Add a short section explaining that the skill also prevents repeated wheel-building by requiring AI to inspect existing dependencies, UI frameworks, and local abstractions before implementation.

The README should stay human-facing. It should not duplicate all skill instructions.

## Anti-Patterns To Add

The skill should explicitly prevent:

- Writing debounce, throttle, deep clone, groupBy, date formatting, class name merging, request clients, or validation helpers before checking existing utilities.
- Hand-writing Button, Modal, Table, Form, DatePicker, Select, Tabs, Upload, Tooltip, Message, or Notification when the project already uses a UI framework that provides them.
- Creating a new request wrapper while the project already has one.
- Creating a new local component that duplicates a shared component or UI framework primitive.
- Installing or assuming a new dependency without user approval.
- Using an installed dependency blindly when a simple local implementation is clearer and the trade-off has been stated.

## Verification

After implementation, verify:

- `SKILL.md` includes the project capability inventory gate.
- `templates/split-plan.md` includes `Existing Capabilities And Reuse Decision`.
- `templates/completion-checklist.md` includes capability reuse checks.
- `templates/refactor-consent.md` supports replacing duplicated local code with existing project capability.
- `README.md` mentions avoiding repeated wheel-building.
- No unrelated files are changed.

## Success Criteria

The extension is successful when AI using the skill:

- Checks existing dependencies and local abstractions before writing frontend implementation code.
- Uses project UI framework primitives instead of hand-writing standard UI elements.
- Reuses existing tools such as lodash, date libraries, request clients, validators, and form libraries when appropriate.
- Explains why it chooses to self-implement instead of reuse.
- Suggests new dependencies only conservatively and only with user approval.
- Preserves the existing feature architecture and refactor-consent behavior.
