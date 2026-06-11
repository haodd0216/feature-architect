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
