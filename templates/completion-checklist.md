# Completion Checklist

Use this before finalizing Vue or React frontend work.

## Architecture Check

- [ ] Each touched file can be explained in one sentence.
- [ ] Each extracted file has a clear owner and dependency direction.
- [ ] Business logic is not hidden inside presentation components.
- [ ] Page or route containers do not own every detail of requests, state, rendering, and helpers for complex workflows unless the local project pattern clearly calls for it.
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
