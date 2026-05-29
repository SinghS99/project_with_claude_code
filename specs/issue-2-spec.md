# Technical Specification — Issue #2

## 1. Issue Overview

| Field | Value |
|-------|-------|
| Title | Privacy policy hover tooltips doesnt show any text when hovered. |
| Description | Hovering the "Privacy Policy" link in the footer shows no tooltip or descriptive text. Screenshot attached. |
| State | **Closed** (resolved by PR #5, merged 2026-05-29) |
| Reporter / Assignee | SinghS99 |
| Labels | none |
| Milestone | none |
| Priority | Low — cosmetic / UX polish, no functional regression |
| Related | Issue #1 (Cookie Policy, identical pattern) → fixed in PR #4 |

## 2. Problem Analysis

**Root cause.** In `src/components/Footer.jsx`, the "Privacy Policy" link was rendered as an `<a>` element with **no `href`** and **no `title` attribute**:

```jsx
<a className="group relative hover:text-white transition-colors duration-300">
  <span className="relative z-10">Privacy Policy</span>
  …
</a>
```

Because there was no `href`, the browser did not treat it as a navigable link (no pointer cursor, no underline by default). Because there was no `title`, no native browser tooltip surfaced on hover — so a user who hovered the link saw a faint background highlight (from the existing Tailwind `group-hover` overlay) but no descriptive text explaining what "Privacy Policy" was about.

This was a mirror of issue #1 (Cookie Policy) and a known footer pattern: three policy links (Privacy / Terms / Cookie) all built the same way and all had the same gap.

## 3. Solution That Shipped (PR #5)

Minimal, consistent with the Cookie Policy fix from PR #4:

- Added a `title` attribute with a one-line description of what the link covers.
- Added `cursor-pointer` so the element feels interactive (since `href` is still absent, browsers wouldn't show a pointer otherwise).
- No new components, hooks, libraries, or routing changes.

Trade-offs:
- **Native `title` tooltip vs. a custom React tooltip.** Native is zero-cost, accessible by default (screen readers expose `title`), and consistent across the three policy links. A custom tooltip would match the gradient styling better but would add weight for what is currently a static, non-clickable element. Native was the right call.
- **Leaving `href` empty.** The Privacy Policy page doesn't exist yet in the route table. Pointing `href="#"` would be misleading; pointing at a placeholder route would 404. Leaving `href` off and surfacing the description via `title` is the honest interim state.

## 4. Step-by-Step Implementation

1. **Locate the Privacy Policy `<a>`** in `src/components/Footer.jsx` (the bottom policy row, just above "Terms of Service" and "Cookie Policy").
2. **Add `title` attribute** with a concise description of what the policy covers.
3. **Add `cursor-pointer`** to the existing `className` so hover feels interactive.
4. **Open a PR** off `main` on a `fix/footer-privacy-policy-tooltip` branch with `Closes #2` in the body so the issue auto-closes on merge.

## 5. Verification Strategy

### Manual checks
- Run `npm run dev`, scroll to the footer, hover "Privacy Policy" → native browser tooltip appears with the description (after the OS-controlled delay, typically ~500ms).
- Cursor changes to a pointer on hover.
- Existing background-highlight hover effect still works (no visual regression).
- Other footer links (Cookie Policy, Terms of Service, Contact Us) behave as before.

### Unit / integration tests
- None added. The change is a single attribute on a static element; there is no test infrastructure for native tooltip behavior in this repo, and Vitest/React-Testing-Library wouldn't observe the browser's tooltip render anyway.

## 6. Files Modified

| File Path | Nature of Change |
|-----------|------------------|
| `src/components/Footer.jsx` | Added `title` + `cursor-pointer` to the Privacy Policy `<a>` (+4 / −1) |

## 7. New Files Created

None.

## 8. Existing Utilities Leveraged

| Utility | Benefit |
|---------|---------|
| Native HTML `title` attribute | Zero-cost, accessible browser tooltip — no new dependency |
| Existing Tailwind `cursor-pointer` utility | Reuses the project's styling system; no inline styles |

## 9. Acceptance Criteria (met by PR #5)

- ✅ Hovering "Privacy Policy" surfaces descriptive text via native tooltip.
- ✅ Cursor changes to a pointer on hover.
- ✅ No visual regression in adjacent footer links.
- ✅ Issue auto-closed via `Closes #2` in PR description.

## 10. Out of Scope

- **Terms of Service link.** Still has no `href` and no `title` — same problem as issue #1 / #2. Not in scope for PR #5; would be a follow-up (`fix/footer-terms-tooltip`).
- **Wiring up real `/privacy`, `/cookies`, `/terms` routes.** The link still has no `href` because those pages don't exist yet in this project. A future change should either add the pages or remove the policy links until they do.
- **Custom React tooltip component.** Native `title` was sufficient and matched the existing pattern (used by PR #4 for Cookie Policy).
