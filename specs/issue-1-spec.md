# Technical Specification — Issue #1

## 1. Issue Overview

| Field | Value |
|-------|-------|
| Title | Footer cookie policy hovering doesnt show any text or description |
| Description | Hovering the "Cookie Policy" link in the footer shows no tooltip or descriptive text. Screenshot attached. |
| State | **Closed** (resolved by PR #4, merged 2026-05-28) |
| Reporter / Assignee | SinghS99 |
| Labels | none |
| Milestone | none |
| Priority | Low — cosmetic / UX polish, no functional regression |
| Related | Issue #2 (Privacy Policy, identical pattern) → fixed in PR #5 |

## 2. Problem Analysis

**Root cause.** In `src/components/Footer.jsx`, the "Cookie Policy" link was rendered as an `<a>` element with **no `href`** and **no `title` attribute**:

```jsx
<a className="group relative hover:text-white transition-colors duration-300">
  <span className="relative z-10">Cookie Policy</span>
  …
</a>
```

Because there was no `href`, the browser did not treat it as a navigable link (no pointer cursor, no underline). Because there was no `title`, no native browser tooltip surfaced on hover. The existing Tailwind `group-hover` gradient overlay still painted a faint background, so the element *looked* responsive — but a hovering user got no descriptive text explaining what "Cookie Policy" was about.

This was the first instance of a footer-wide pattern: all three policy links (Privacy / Terms / Cookie) were built the same way and all had the same gap. Issue #1 surfaced it on Cookie Policy; issue #2 followed for Privacy Policy.

## 3. Solution That Shipped (PR #4)

Minimal patch:

- Added a `title` attribute with a one-line description of what the link covers.
- Added `cursor-pointer` so the element feels interactive (since `href` is still absent, browsers wouldn't show a pointer otherwise).
- No new components, hooks, libraries, or routing changes.

PR #4 also bundled an unrelated change: it created `.claude/settings.json` to enable the `commit-commands` Claude Code plugin. That was tooling, not part of the fix; it shipped in the same PR as a convenience. (See section 10.)

Trade-offs:
- **Native `title` tooltip vs. a custom React tooltip.** Native is zero-cost, accessible by default (screen readers expose `title`), and consistent. A custom tooltip would match the gradient styling more closely but adds weight for a static, non-clickable element. Native was the right call and set the precedent that PR #5 then followed for Privacy Policy.
- **Leaving `href` empty.** The Cookie Policy page doesn't exist yet in the route table. Adding `href="#"` would be misleading; pointing at a placeholder route would 404. Leaving `href` off and surfacing the description via `title` is the honest interim state.

## 4. Step-by-Step Implementation

1. **Locate the Cookie Policy `<a>`** in `src/components/Footer.jsx` (the third element in the bottom policy row, after Privacy Policy and Terms of Service).
2. **Add `title` attribute** with a concise description of what the policy covers.
3. **Add `cursor-pointer`** to the existing `className` so hover feels interactive.
4. **Open a PR** off `main` on a `fix/footer-cookie-policy-tooltip` branch with `Closes #1` in the body so the issue auto-closes on merge.

## 5. Verification Strategy

### Manual checks
- Run `npm run dev`, scroll to the footer, hover "Cookie Policy" → native browser tooltip appears with the description (after the OS-controlled delay, typically ~500ms).
- Cursor changes to a pointer on hover.
- Existing background-highlight hover effect still works (no visual regression).
- Other footer links (Privacy Policy, Terms of Service, Contact Us) behave as before.

### Unit / integration tests
- None added. The change is a single attribute on a static element; there is no test infrastructure for native tooltip behavior in this repo, and Vitest/React-Testing-Library wouldn't observe the browser's tooltip render anyway.

## 6. Files Modified

| File Path | Nature of Change |
|-----------|------------------|
| `src/components/Footer.jsx` | Added `title` + `cursor-pointer` to the Cookie Policy `<a>` (+4 / −1) |

## 7. New Files Created

| File Path | Purpose |
|-----------|---------|
| `.claude/settings.json` | Enables the `commit-commands` Claude Code plugin for anyone opening the repo. Unrelated to the issue — bundled into PR #4 as a convenience. |

## 8. Existing Utilities Leveraged

| Utility | Benefit |
|---------|---------|
| Native HTML `title` attribute | Zero-cost, accessible browser tooltip — no new dependency |
| Existing Tailwind `cursor-pointer` utility | Reuses the project's styling system; no inline styles |

## 9. Acceptance Criteria (met by PR #4)

- ✅ Hovering "Cookie Policy" surfaces descriptive text via native tooltip.
- ✅ Cursor changes to a pointer on hover.
- ✅ No visual regression in adjacent footer links.
- ✅ Issue auto-closed via `Closes #1` in PR description.

## 10. Out of Scope / Observations

- **Terms of Service link.** Still has no `href` and no `title` — same problem as issue #1 and #2. Neither PR #4 nor PR #5 touched it. A follow-up `fix/footer-terms-tooltip` would close the pattern.
- **Wiring up real `/cookies`, `/privacy`, `/terms` routes.** The link still has no `href` because those pages don't exist yet. A future change should either add the pages or remove the policy links until they do.
- **Bundled tooling change.** PR #4 included `.claude/settings.json` (plugin enablement) alongside the Cookie Policy fix. Cleaner future practice: keep tooling/config changes in their own PR so the fix-PR's diff stays narrowly scoped to the issue it closes.
- **Custom React tooltip component.** Native `title` was sufficient; deferred indefinitely.
