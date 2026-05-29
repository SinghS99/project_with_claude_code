# 🔎 Change Investigation Report

**Target**: `src/components/Footer.jsx` lines 142–180 (footer bottom bar — legal links row + copyright)
**Investigation Date**: 2026-05-29
**Repository**: https://github.com/SinghS99/project_with_claude_code.git
**Branch**: main

---

## 📋 Investigation Summary

| Detail                  | Value                                     |
| ----------------------- | ----------------------------------------- |
| File(s) Analyzed        | `src/components/Footer.jsx`               |
| Lines Investigated      | 142–180 (39 lines)                        |
| Total Commits on File   | 2                                         |
| Unique Authors          | 2 (same person, two identities)           |
| File Age (First Commit) | 2026-05-28 (1 day old)                    |
| Last Modified           | 2026-05-29 by SinghS99 (PR #4)            |

---

## 👥 Author Breakdown

Lines 142–180 (39 lines) only — current state:

| # | Author   | Email                                          | Commits on File | Lines Owned (in range) | First Contribution | Last Contribution |
| - | -------- | ---------------------------------------------- | --------------- | ---------------------- | ------------------ | ----------------- |
| 1 | Singh    | 2229777@cognizant.com                          | 1               | 35 / 39 (≈ 89.7%)      | 2026-05-28         | 2026-05-28        |
| 2 | SinghS99 | 92422476+SinghS99@users.noreply.github.com     | 1               | 4 / 39 (≈ 10.3%)       | 2026-05-29         | 2026-05-29        |

> Both identities map to the same human (the repo owner) — the work / corporate identity `Singh <2229777@cognizant.com>` made the initial commit locally, the GitHub identity `SinghS99 <92422476+SinghS99@users.noreply.github.com>` merged PR #4 via GitHub.

**Primary Owner**: Singh (`2229777@cognizant.com`) — initial author of the section.
**Most Recent Contributor**: SinghS99 — Cookie Policy tooltip fix on 2026-05-29.
**CODEOWNERS**: Not configured.

---

## 📅 Change Timeline

### `91edecd` — 2026-05-29 09:54:30 +0530

- **Author**: SinghS99 <92422476+SinghS99@users.noreply.github.com>
- **Message**: `Fix/footer cookie policy tooltip (#4)`
- **Body**:
	> `fix(footer): add tooltip to Cookie Policy link on hover` — The Cookie Policy link in the footer had no href and no title attribute, so hovering produced no native tooltip or description. Added a title describing what the link covers, plus `cursor-pointer` so the element feels interactive.
	>
	> Also bundled a second commit: `chore: enable commit-commands Claude Code plugin` — adds `.claude/settings.json` so the commit-commands plugin is enabled for anyone who opens this repo in Claude Code.
	>
	> Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
- **Ticket References**: `Closes #1`, PR `#4`
- **Lines Changed (this file)**: +4 / -1
- **What Changed (in target range)**: Replaced the single-line `<a className="...">` opening tag for the Cookie Policy link with a multi-line `<a>` that adds a `title="Learn how JobPortal uses cookies..."` attribute and appends `cursor-pointer` to its className. Pure UX/accessibility fix — no behavior change, no href added.

### `0234e24` — 2026-05-28 18:18:14 +0530

- **Author**: Singh <2229777@cognizant.com>
- **Message**: `Initial commit: React 19 job portal UI`
- **Body**: —
- **Ticket References**: None
- **Lines Changed (this file)**: Whole file added (initial import).
- **What Changed (in target range)**: Introduced the entire footer bottom bar — the legal-links row (`Privacy Policy`, `Terms of Service`, `Cookie Policy`, `Contact Us`) and the copyright block (`© 2026 JobPortal. All rights reserved.` + tagline). All four legal links were inserted as anchor-style elements without `href`s; only `Contact Us` was wired up as a real `<Link to="/contact">`.

---

## 🔬 Line-by-Line Blame (Current State, lines 142–180)

Compressed into contiguous blocks (same commit + author):

| Lines     | Commit    | Author   | Date       | What's here                                                                  |
| --------- | --------- | -------- | ---------- | ---------------------------------------------------------------------------- |
| 142–151   | `0234e24` | Singh    | 2026-05-28 | Outer row container + `Privacy Policy` and `Terms of Service` placeholder `<a>` tags |
| 152–155   | `91edecd` | SinghS99 | 2026-05-29 | **Cookie Policy `<a>` opening tag — multi-line, with `title=` and `cursor-pointer`** |
| 156–180   | `0234e24` | Singh    | 2026-05-28 | Cookie Policy `<span>`+ hover gradient, `Contact Us` `<Link>`, copyright + tagline block, closing tags |

Spot detail for the touched span:

| Line | Code (truncated)                                                            | Author   | Commit    |
| ---- | --------------------------------------------------------------------------- | -------- | --------- |
| 152  | `              <a`                                                          | SinghS99 | `91edecd` |
| 153  | `                title="Learn how JobPortal uses cookies to improve y...` | SinghS99 | `91edecd` |
| 154  | `                className="group relative hover:text-white transitio...` | SinghS99 | `91edecd` |
| 155  | `              >`                                                           | SinghS99 | `91edecd` |

---

## 🎫 Linked Tickets & References

| Ticket    | Commit    | Author   | Date       | Subject                                |
| --------- | --------- | -------- | ---------- | -------------------------------------- |
| Issue #1  | `91edecd` | SinghS99 | 2026-05-29 | Fix/footer cookie policy tooltip (#4)  |
| PR #4     | `91edecd` | SinghS99 | 2026-05-29 | Fix/footer cookie policy tooltip (#4)  |

The initial commit has no ticket reference (expected for an initial import).

---

## 💡 Insights

- **Churn Assessment**: Very low. 2 commits in 2 days, only one of which is post-initial-import. The legal-links region has been touched exactly once since it landed.
- **Bus Factor**: Effectively **1** — both Git identities belong to the same person (`SinghS99` / `Singh`). No second human has ever edited this footer. If `SinghS99` is unavailable, no one else has context on the placeholder `<a>` tags (no `href`, no route) for Privacy Policy / Terms of Service / Cookie Policy.
- **Stale Code Risk**: Low (file is 1 day old) — but worth noting that **three of the four legal links (`Privacy Policy`, `Terms of Service`, `Cookie Policy`) still have no `href` or `to=`**. PR #4 only patched the *tooltip* on Cookie Policy; the underlying "links go nowhere" issue from #1 is not fully addressed. Likely follow-up work.
- **Review Gaps**: The initial commit (`0234e24`) had no ticket reference, but as an initial scaffolding commit that's expected. The fix commit (`91edecd`) properly linked `Closes #1` and went through PR #4 — good hygiene.
- **Bundling**: PR #4 squashed two unrelated changes (`fix(footer)` + `chore: enable commit-commands plugin`). The chore did not touch this file, but per the project's git conventions (one purpose per PR / Conventional Commits), splitting them would have been cleaner.

---

```
═══════════════════════════════════════════════════════════════
  🔎 CHANGE INVESTIGATION COMPLETE
═══════════════════════════════════════════════════════════════

  Target:          src/components/Footer.jsx :142-180
  File(s):         src/components/Footer.jsx

  📊 Quick Stats:
     Total Commits:    2
     Unique Authors:   2 identities (1 person)
     File Age:         1 day
     Last Changed:     2026-05-29 by SinghS99 (PR #4)

  👤 Primary Owner:    Singh — 35/39 lines (≈ 89.7%)
  👤 Last Contributor: SinghS99 on 2026-05-29 (lines 152-155)

  🎫 Ticket References Found: 2 (Issue #1, PR #4)
  ⚠️  Commits Without Tickets: 1 (initial commit — expected)

  📄 Full report saved: ./change-investigation-report.md
═══════════════════════════════════════════════════════════════
```
