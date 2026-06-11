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
- Affected area: name the file, folder, component, hook, composable, service, or store.
- Reason: explain why the requested change requires or avoids structural change.
- If yes, ask for user consent before changing structure.

**Verification Plan:**

- Type check, unit test, component test, local manual path, or focused command.
- Explain why this verification matches the changed boundary.
