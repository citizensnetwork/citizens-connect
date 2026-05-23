# Branch Audit — Citizens Connect

> **Audit date:** 2026-05-23
> **Auditor:** Copilot (automated review)
> **Repo:** `citizensnetwork/citizens-connect`
> **Baseline:** `origin/main` @ `aba287a` (place panel + RLS + cover-remove batch)
> **Reference docs used:** `.github/MASTER_DIRECTION.md`, `.github/VISION.md`, `CITIZENS_STRATEGIC_DIRECTION_MAY2026.md`, `CITIZENS_CONNECT_RESTRUCTURING_STRATEGY.md`, `RESUME_HERE.md`, `docs/FUTURE_IDEAS.md`

---

## TL;DR

The repo currently has **33 non-`main` branches** (excluding `copilot/review-non-main-branches`, the working branch for this audit). After comparing every branch against `origin/main`:

- **32 / 33 branches are fully contained in `main`** — every commit on the branch tip is already reachable from `origin/main`. They have **zero unique commits**. They were either merged, squashed-and-merged, or completely superseded by later work.
- **1 / 33 branches** (`copilot/task-274617442-…`) has 2 unique commits, but those commits modify code that has since been **deleted by locked decisions D5 and D6** (FullCalendar removed, Featured Panel removed).

**Recommendation: delete all 33 branches.** None of them contain code that should be preserved or re-merged. Keeping them adds noise to branch lists, confuses future agents, and risks someone reopening a stale PR against an obsolete code surface (a ~200-commit-behind branch touching a removed component would be ~100% conflicts).

---

## Methodology

For every remote branch:

1. Computed `git rev-list --count origin/main..<branch>` (commits unique to the branch — "ahead").
2. Computed `git rev-list --count <branch>..origin/main` (commits the branch is missing — "behind").
3. Verified `ahead = 0` branches with `git merge-base --is-ancestor <branch> origin/main` (sanity check that the tip really is reachable from main).
4. Inspected the unique commits of the `ahead > 0` branch.
5. Cross-referenced each branch's intent (from name + last-commit message) against:
   - **`MASTER_DIRECTION.md`** locked decisions D1–D16.
   - **`VISION.md`** principles and ecosystem channels.
   - **`CITIZENS_STRATEGIC_DIRECTION_MAY2026.md`** Pretoria-first + WCI focus + post-June-9 roadmap.
   - **`CITIZENS_CONNECT_RESTRUCTURING_STRATEGY.md`** `.claude/` segment structure.
   - **`RESUME_HERE.md`** current platform state (714 tests, all 17 audit surfaces clean, place-panel batch).
   - **`docs/FUTURE_IDEAS.md`** deferred features.

---

## Verdict matrix

Legend:

- **Status — Contained:** branch tip is an ancestor of `origin/main`. Zero unique commits. Safe to delete.
- **Status — Stale + against locked direction:** branch has unique commits but they target code/features that have been removed or replaced by a locked decision. Safe to delete.
- All branches sorted by last-commit date (newest first).

| # | Branch (`copilot/…`) | Last commit | Ahead | Behind | Verdict | Reasoning |
|---|---|---|---:|---:|---|---|
| 1 | `fix-map-visibility-issue-again` | 2026-05-23 | 0 | 2 | **Delete — Contained** | Already merged into `main` (PR #32 was the last predecessor of the place-panel batch). |
| 2 | `fix-interactivity-issues` | 2026-04-24 | 0 | 77 | **Delete — Contained** | Marker click-to-preview restoration is in `main` (subsequent batches added quick-action popup + EventsView refactors). |
| 3 | `add-dismiss-button-to-banner` | 2026-04-24 | 0 | 81 | **Delete — Contained** | Banner dismiss work landed; superseded by later notification + landing-page work. |
| 4 | `update-homepage-visuals` | 2026-04-17 | 0 | 135 | **Delete — Contained** | Landing-page taglines were rewritten *again* on 2026-05-23 (see `RESUME_HERE.md` §2). Any value is already in `main`. |
| 5 | `update-search-bar-layout` | 2026-04-17 | 0 | 133 | **Delete — Contained** | Search bar was completely rebuilt in Batch QP1 (quick-search panel, tab-gated tiles, city chips). This branch's layout is dead architecture. |
| 6 | `optimize-event-booking-ui` | 2026-04-17 | 0 | 144 | **Delete — Contained** | RSVP / Connect / Consider flows have since been fully rebuilt under FEAT-04. |
| 7 | `update-event-icon-sizes` | 2026-04-17 | 0 | 157 | **Delete — Contained** | Marker sizing now governed by `src/lib/map/markers.ts` (temporal style + DOM cull, Batch 13). |
| 8 | `remove-trending-menu-item-standardize-map-icons` | 2026-04-17 | 0 | 150 | **Delete — Contained** | Trending was removed/transformed and map icons standardised in main long ago. |
| 9 | `adjust-category-search-popup` | 2026-04-17 | 0 | 155 | **Delete — Contained** | Category panel and quick-action popup are entirely re-built (`QuickActionPopup`, `BurgerConsiderSection`, etc.). |
| 10 | `adapt-map-features-to-google-maps` | 2026-04-17 | 0 | 130 | **Delete — Contained AND against D4** | Decision **D4** locks **MapLibre GL JS + MapTiler Cloud**. *"No migration to Mapbox."* — and certainly no migration to Google Maps. Even if it had unique commits, this would be a hard delete. |
| 11 | `update-trending-label-and-title-bar` | 2026-04-16 | 0 | 179 | **Delete — Contained** | Trending UI restructured in main. |
| 12 | `update-trending-tag-icons-and-designs` | 2026-04-16 | 0 | 163 | **Delete — Contained** | Same — trending design pass is in main. |
| 13 | `revert-map-icons-changes` | 2026-04-16 | 0 | 159 | **Delete — Contained** | Revert was applied. |
| 14 | `start-implementation` | 2026-04-16 | 0 | 198 | **Delete — Contained** | Generic kickoff branch; entirely subsumed. |
| 15 | `update-landing-page-text-visibility` | 2026-04-16 | 0 | 187 | **Delete — Contained** | Landing copy has been rewritten twice since. |
| 16 | `apply-glass-theme-to-ui-windows` | 2026-04-16 | 0 | 191 | **Delete — Contained** | Glass-overlay calendar (FEAT-02) and floating glass controls are in main per the connect-ui-system instructions. |
| 17 | `fix-event-icon-quick-window` | 2026-04-16 | 0 | 168 | **Delete — Contained** | Quick popup is now `QuickActionPopup` with View/Join/Consider/Share/Visit — fully rebuilt. |
| 18 | `hide-place-icons-on-zoom` | 2026-04-16 | 0 | 182 | **Delete — Contained** | Progressive geo-clustering + place marker zoom behaviour already in main (Batch 13). |
| 19 | `fix-error-in-actions-run` | 2026-04-16 | 0 | 197 | **Delete — Contained** | Lockfile sync only; lockfile has changed many times since. |
| 20 | `fix-build-error` | 2026-04-16 | 0 | 183 | **Delete — Contained** | Test aria-label rename only; trending was later removed entirely. |
| 21 | `adjust-icon-spacing-and-layering` | 2026-04-16 | 0 | 172 | **Delete — Contained** | Icon layering is governed by current marker system. |
| 22 | `update-ui-design-elements` | 2026-04-15 | 0 | 222 | **Delete — Contained** | Generic UI sweep; entirely subsumed by the 60/30/10 system codified in `.github/instructions/connect-ui-system.instructions.md`. |
| 23 | `fix-href-attribute-error` | 2026-04-15 | 0 | 220 | **Delete — Contained** | Events link + MapTiler style test fix — both areas have changed many times. |
| 24 | `fix-map-visibility-and-update-icons` | 2026-04-15 | 0 | 217 | **Delete — Contained** | One of six overlapping map-visibility branches; the survivor is `main`. |
| 25 | `fix-map-display-issues` | 2026-04-15 | 0 | 216 | **Delete — Contained** | Merge commit of another map-visibility branch; the survivor is `main`. |
| 26 | `align-event-svg-icons` | 2026-04-15 | 0 | 214 | **Delete — Contained AND references removed feature** | "expand featured panel handle touch target" — Featured Panel is **removed by D6**. |
| 27 | `fix-map-movement-and-review-posting` | 2026-04-15 | 0 | 212 | **Delete — Contained AND references removed feature** | Mentions "featured panel map glitch" — D6 removes the panel. Review posting is now under reviews/messaging audit surfaces (all clean per RESUME_HERE §1.5). |
| 28 | `fix-test-error-issue` | 2026-04-15 | 0 | 212 | **Delete — Contained** | Duplicate of #27 (same last commit SHA `place icon, featured panel map glitch, event review submission error`). |
| 29 | `fix-error-from-last-commit` | 2026-04-15 | 0 | 210 | **Delete — Contained** | ReviewForm test mock fix; ReviewForm has been re-shipped many times since. |
| 30 | `fix-map-and-featured-bar-issues` | 2026-04-15 | 0 | 208 | **Delete — Contained AND against D6** | Mentions "featured-panel scroll glitch" — Featured Panel is **removed**. |
| 31 | `fix-map-visibility-issue` | 2026-04-15 | 0 | 206 | **Delete — Contained** | One of six overlapping map-visibility branches. |
| 32 | `task-274617442-1202352508-5ba92d16-9366-42b7-a4f3-abfed1555b75` | 2026-04-15 | **2** | 216 | **Delete — Stale + against D5 and D6** | The only branch with unique commits. Diff touches `EventCalendar.tsx` (FullCalendar — **removed by D5**), `EventsView.tsx`, `EventMap.tsx`, `markers.ts`, and the commit message says *"feat: UI improvements — place markers, featured panel, calendar colors, map controls"*. Featured Panel is **removed by D6** and FullCalendar is **removed by D5** (replaced with glass-overlay calendar FEAT-02). The unique work is unmergeable and against locked direction. |

---

## Cross-reference: every branch vs. locked decisions

| Locked decision | Branches that conflict |
|---|---|
| **D4** — Map engine locked to MapLibre + MapTiler, no migration to Mapbox | `adapt-map-features-to-google-maps` |
| **D5** — FullCalendar **removed**, replaced with glass-overlay calendar | `task-274617442-…` (touches `EventCalendar.tsx`) |
| **D6** — Featured Panel **removed entirely** | `align-event-svg-icons`, `fix-map-movement-and-review-posting`, `fix-test-error-issue`, `fix-map-and-featured-bar-issues`, `task-274617442-…` |
| **D7** — 11-agent system **discarded** (now Architect + Security inline) | none directly |
| **D14** — Universal right-side panel pattern for content views | `optimize-event-booking-ui`, `update-search-bar-layout`, `adjust-category-search-popup` (all predate the universal panel pattern) |
| **D15** — Burger bar contents (filters, Considerations, friends only) | `remove-trending-menu-item-standardize-map-icons`, `update-trending-*` (predate burger-bar finalisation) |

No branch conflicts with D1–D3 (brand, slogan, palette), D8 (Capacitor), D9 (Next.js), D10 (Supabase), D11 (PayFast), D12 (CASI), D13 (English-only), D16 (simplicity).

---

## Cross-reference: every branch vs. strategic priorities

From `CITIZENS_STRATEGIC_DIRECTION_MAY2026.md` and `RESUME_HERE.md`, current platform priorities are:

1. **URGENT (before June 9 WCI):** apply migration 092, upload POPUP images, verify demo flow on deployed site (Stephen + dev tasks, **no code changes from agents**).
2. **Post-June-9:** onboard WCI orgs (content seeding), complete remaining audit polish queue, push notifications (FCM/APNs credentials), "Where to Serve" filter, real landing-page rewrite informed by WCI feedback.
3. **Q3–Q4 2026:** PayFast wire-up, monorepo migration before Citizens Wear, mobile app store submission.

**None** of the 33 audited branches contributes to any of these priorities. Every branch is either:

- A point fix from mid-April 2026 already merged into `main`, or
- A duplicate / overlapping attempt at the same fix, or
- Work on a surface that has since been replaced (Featured Panel, FullCalendar, original trending tab, pre-`QuickActionPopup` event icons, pre-Batch-QP1 search bar), or
- A migration explicitly forbidden by a locked decision (`adapt-map-features-to-google-maps`).

---

## Deletion plan

### What to delete

All 33 branches in the verdict table above. They split into:

- **32 branches** that are tip-reachable from `origin/main` — risk-free deletions (GitHub will not warn; the commits are preserved in `main`).
- **1 branch** (`copilot/task-274617442-…`) with 2 unique commits that **must not** be merged (FullCalendar + Featured Panel revival). The commits will be lost on delete — that is the desired outcome, since the work is against locked direction. If absolute preservation is desired, tag it first with `git tag archive/task-274617442 <sha>` before delete.

### How to delete (sandbox cannot push delete-refs)

This sandboxed agent cannot `git push --delete` to the remote. The owner can delete in one of three ways:

**Option A — bulk shell script (recommended):**

```bash
# From a clone with push rights:
git fetch --all --prune
branches=$(git branch -r | grep '^  origin/copilot/' \
  | grep -v 'origin/copilot/review-non-main-branches' \
  | sed 's|  origin/||')

echo "$branches" | while read b; do
  echo "Deleting $b"
  git push origin --delete "$b"
done
```

**Option B — GitHub CLI:**

```bash
gh api repos/citizensnetwork/citizens-connect/branches \
  --paginate --jq '.[].name' \
  | grep '^copilot/' \
  | grep -v 'copilot/review-non-main-branches' \
  | xargs -I {} gh api -X DELETE repos/citizensnetwork/citizens-connect/git/refs/heads/{}
```

**Option C — GitHub UI:** Settings → Branches → branch list → bin icon on each branch. Slowest, but lowest-risk.

### Whitelist (do NOT delete)

- `main` — the trunk.
- `copilot/review-non-main-branches` — this branch (carrying the audit report itself). Delete it after this PR is merged.

### Optional: archive before delete

If any historical preservation is desired:

```bash
# Tag the one branch with unique commits before deleting:
git tag archive/task-274617442-featured-fullcalendar-attempt \
  origin/copilot/task-274617442-1202352508-5ba92d16-9366-42b7-a4f3-abfed1555b75
git push origin archive/task-274617442-featured-fullcalendar-attempt
```

All other branches add nothing to history beyond what is already in `main`.

---

## What I did not delete

I (the audit agent) did not delete any branches because the sandbox environment forbids `git push` (including delete-refs). The deletion step requires a human or a CI job with push rights — see "Deletion plan" above.

---

## Appendix A — Branches by theme

For readability, here is the same 33-branch list grouped by what they were trying to do.

### Six overlapping map-visibility attempts (Apr 15)
`fix-map-visibility-issue`, `fix-map-visibility-issue-again`, `fix-map-display-issues`, `fix-map-visibility-and-update-icons`, `fix-map-and-featured-bar-issues`, `fix-map-movement-and-review-posting` — all merged or superseded; map visibility is solved by current `EventMap.tsx` + `src/lib/map/markers.ts`.

### Map icons / markers (Apr 16–17)
`update-event-icon-sizes`, `revert-map-icons-changes`, `align-event-svg-icons`, `adjust-icon-spacing-and-layering`, `fix-event-icon-quick-window`, `hide-place-icons-on-zoom` — superseded by Batch 13 (DOM marker culling) and the canonical `markers.ts` builders.

### Trending (removed / restructured)
`update-trending-label-and-title-bar`, `update-trending-tag-icons-and-designs`, `remove-trending-menu-item-standardize-map-icons` — trending was removed from the burger bar per D15.

### UI / landing / search (Apr 15–17)
`update-ui-design-elements`, `update-homepage-visuals`, `update-landing-page-text-visibility`, `apply-glass-theme-to-ui-windows`, `optimize-event-booking-ui`, `update-search-bar-layout`, `adjust-category-search-popup` — all subsumed by the codified 60/30/10 system (`connect-ui-system.instructions.md`) and Batch QP1 (quick-search panel).

### Build / test / lockfile fixes (Apr 15–16)
`fix-build-error`, `fix-error-from-last-commit`, `fix-error-in-actions-run`, `fix-test-error-issue`, `fix-href-attribute-error` — point fixes, all absorbed.

### Misc
`add-dismiss-button-to-banner`, `fix-interactivity-issues`, `start-implementation` — all absorbed.

### Against locked direction
- `adapt-map-features-to-google-maps` — **D4 violation** (MapLibre + MapTiler locked).
- `task-274617442-…` — **D5 + D6 violation** (only branch with unique commits, and they target removed code).

---

## Appendix B — Raw ahead/behind data

```
date       | branch                                              | ahead | behind
2026-05-23 | fix-map-visibility-issue-again                      |   0   |   2
2026-04-24 | fix-interactivity-issues                            |   0   |  77
2026-04-24 | add-dismiss-button-to-banner                        |   0   |  81
2026-04-17 | adapt-map-features-to-google-maps                   |   0   | 130
2026-04-17 | update-search-bar-layout                            |   0   | 133
2026-04-17 | update-homepage-visuals                             |   0   | 135
2026-04-17 | optimize-event-booking-ui                           |   0   | 144
2026-04-17 | remove-trending-menu-item-standardize-map-icons     |   0   | 150
2026-04-17 | adjust-category-search-popup                        |   0   | 155
2026-04-17 | update-event-icon-sizes                             |   0   | 157
2026-04-16 | revert-map-icons-changes                            |   0   | 159
2026-04-16 | update-trending-tag-icons-and-designs               |   0   | 163
2026-04-16 | fix-event-icon-quick-window                         |   0   | 168
2026-04-16 | adjust-icon-spacing-and-layering                    |   0   | 172
2026-04-16 | update-trending-label-and-title-bar                 |   0   | 179
2026-04-16 | hide-place-icons-on-zoom                            |   0   | 182
2026-04-16 | fix-build-error                                     |   0   | 183
2026-04-16 | update-landing-page-text-visibility                 |   0   | 187
2026-04-16 | apply-glass-theme-to-ui-windows                     |   0   | 191
2026-04-16 | fix-error-in-actions-run                            |   0   | 197
2026-04-16 | start-implementation                                |   0   | 198
2026-04-15 | fix-map-visibility-issue                            |   0   | 206
2026-04-15 | fix-map-and-featured-bar-issues                     |   0   | 208
2026-04-15 | fix-error-from-last-commit                          |   0   | 210
2026-04-15 | fix-map-movement-and-review-posting                 |   0   | 212
2026-04-15 | fix-test-error-issue                                |   0   | 212
2026-04-15 | align-event-svg-icons                               |   0   | 214
2026-04-15 | task-274617442-1202352508-5ba92d16-9366-42b7-a4f3-… |  *2*  | 216
2026-04-15 | fix-map-display-issues                              |   0   | 216
2026-04-15 | fix-map-visibility-and-update-icons                 |   0   | 217
2026-04-15 | fix-href-attribute-error                            |   0   | 220
2026-04-15 | update-ui-design-elements                           |   0   | 222
```

Generated via `git rev-list --count origin/main..<branch>` and `<branch>..origin/main` on 2026-05-23 against `origin/main` @ `aba287a`.
