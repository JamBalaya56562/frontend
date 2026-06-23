# Open-Source-Economy/frontend — Issues & PRs Investigation

A snapshot of the current open work in the upstream repository
[`Open-Source-Economy/frontend`](https://github.com/Open-Source-Economy/frontend): every open issue and open
pull request, what it asks for, its status, and a suggested next action.

_Generated: 2026-06-23_

## TL;DR

- **5 open issues** (#74, #73, #72, #70, #51) and **2 open PRs** (#75, #71).
- Several issues come from **Figma Make AI-generated UI** that must be imported, cleaned, refactored, and wired to the
  backend (#74, partly #72).
- **Newsletter (#70)** is being resolved by our PR **#75**, which supersedes the abandoned PR **#71**.
- Two issues are **product/content**, not pure engineering: a service-catalog addition (#73, awaiting requester spec) and a
  legal imprint page (#51, handed to the designer during a site redesign).
- Repo history shows a large graveyard of **closed/un-merged onboarding PRs** (#52–#65) — onboarding has been churned on
  heavily; new onboarding work (#74, #72) should reuse the merged baseline (#56, #57, #58, #60), not the closed attempts.

---

## Open issues

### #74 — Feature: Add Step 6 (Review & Confirm) to Maintainer Onboarding Flow
- **Author / opened:** LaurianeOSE · 2025-12-17 · 0 comments
- **Type:** Feature (onboarding) · **Source:** Figma Make AI-generated UI
- **Ask:** Add a 6th "Review & Confirm" step to the existing 5-step maintainer onboarding flow, letting users review all
  entered data before submitting.
- **Notes from the issue:**
  - Workflow is explicit: **import the Figma UI → clean/refactor → add logic + backend connections.**
  - Figma elements that don't fit the current data model should be **commented out (with a TODO)**, not deleted, to
    preserve design intent.
  - Staging to explore the current flow: `https://stage-ose.vercel.app/`.
  - The issue itself is flagged "generated using AI and not fully reviewed" — treat details as guidance, not spec.
- **Status:** Open, unassigned, no PR.
- **Suggested next action:** Locate the Figma source (see `Open-Source-Economy/figma-frontend`), scope which step
  components already exist under `src/views/pages/onboarding/steps/`, and build Step 6 reading from the existing onboarding
  state/data model.

### #73 — Open source strategy consulting to be added to services
- **Author / opened:** gravax · 2025-11-20 · 2 comments
- **Type:** Product / service-catalog request
- **Ask:** Offer **project-agnostic** advisory services (licensing, community management, governance strategy) in the
  Advisory section, not tied to a specific open-source project.
- **Discussion state:** LaurianeOSE asked the requester to define a small, discrete set of offerings
  (Category / Title / Description). gravax confirmed the services are project-agnostic and started outlining them.
- **Status:** Open — **blocked on requester providing the finalized service list** before implementation.
- **Suggested next action:** Wait for the structured offering list; technically this is adding entries to the services
  data + supporting a project-agnostic service category (current model attaches services to projects).

### #72 — 🧩 Maintainer Login State & Profile Page
- **Author / opened:** LaurianeOSE · 2025-11-08 · 0 comments
- **Type:** Feature (auth/profile) — largest in scope of the open issues
- **Ask (three parts):**
  1. **Header login state** — when logged in, show the maintainer's name/avatar and hide the "Are you a developer" banner.
  2. **Maintainer Profile page** — view + edit onboarding data (uses `OnboardingBackendAPI` endpoints).
  3. **Redirect logic** — after login/registration, send incomplete profiles to onboarding and complete profiles to a
     dashboard. Must be handled frontend-side (GitHub auth allows only one redirect URL).
- **Extra:** projects added after registration aren't auto-included in the selected services list — wants UX to associate
  new projects with the service list.
- **Relevant code:** `src/views/auth`; profile check via `getDeveloperProfile` →
  `FullDeveloperProfile.profileEntry.profile.onboardingCompleted`. Auth is already implemented and working; only
  maintainers can currently register/log in.
- **Status:** Open, unassigned, no PR.
- **Suggested next action:** Split into sub-tasks (header state, profile page, redirect guard). Start with the redirect
  guard + header state since auth already works.

### #70 — Feature: Newsletter Subscription  ← being resolved by our PR #75
- **Author / opened:** LaurianeOSE · 2025-11-05 · 0 comments
- **Type:** Feature (integration + refactor)
- **Ask:** Connect the footer newsletter form to the backend so submissions persist, and extract the logic from
  `Footer.tsx` into a dedicated component.
- **Acceptance criteria:** validate email before submit · use the newsletter endpoint · visible loading state ·
  success/error feedback · logic moved out of `Footer.tsx`.
- **Caveat:** the issue references **old paths** (`src/ultils/handleApiCall.ts`, `BackendAPI.ts.subscribeToNewsletter`,
  `forms/validators.ts`). The current codebase uses the service/hook/RHF+Zod stack instead — criteria were met using the
  current architecture rather than the literally-named files.
- **Status:** Open → addressed by **PR #75** (`closes #70`).
- **Suggested next action:** Merge PR #75.

### #51 — An imprint (Impressum) is required for websites in the UK and Germany
- **Author / opened:** henderkes · 2025-08-15 · 1 comment
- **Type:** Legal / content page
- **Ask:** Add a dedicated imprint (Impressum) page — legally required in Germany/UK and good practice elsewhere. Company
  info already exists via the CHE-440.058.692 registration link.
- **Discussion state:** LaurianeOSE replied that the site is being redesigned and transferred the issue to the designer.
- **Status:** Open — **deferred to the ongoing website redesign / design team.**
- **Suggested next action:** No engineering action until the redesign lands; then add a static imprint route.

---

## Open pull requests

### #75 — feat: newsletter subscription section (closes #70)  ← ours
- **Author / branch:** JamBalaya56562 · `newsletter-finalize` · opened 2026-06-23
- **What:** Dedicated `NewsletterSection` extracted from `Footer.tsx`; RHF + Zod validation; submits via
  `communicationService.subscribeToNewsletter`; loading/success/error states; removes unused `newsletterDemoToggle` flag.
- **State:** Open, single squashed commit, 2 files changed. Supersedes #71.
- **Action:** Ready for review/merge → closes #70.

### #71 — Feature/newsletter integration  ← abandoned, to be closed
- **Author / branch:** densteph0411-dev · `feature/newsletter-integration` · opened 2025-11-07
- **State:** **CONFLICTING / DIRTY** (merge conflicts), last updated 2025-12-28, 2 files / +156 −79, 1 review comment.
- **Why it's stuck:** written against an **older codebase** (`src/ultils/`, `handleApiCall`, `getBackendAPI()`,
  `validateEmail`) that no longer exists; author not resuming.
- **Action:** **Close in favor of PR #75** once #75 merges.

---

## Context: closed history (why onboarding issues need care)

- **Merged (15 total)** include the onboarding baseline — #56 (Refactor Step 2), #57 (Refactor/onboarding),
  #58 (OnboardingBackendAPI mock), #60 (refactor onboarding components), #33 (Dev) — plus recent pekko campaign tweaks
  (#67, #68, #69).
- **A large set of onboarding PRs were closed un-merged** (#52, #53, #54, #55, #59, #61, #63, #64, #65) — multiple parallel
  attempts at the same flow. New onboarding work (#74, #72) should build on the **merged** baseline above and avoid
  reviving the closed branches.
- **Closed issues:** #66 (spam), #6 (buttons → tailwind), #5 (home video), #3 (test issue).

---

## Suggested priority order

1. **#75 → merge** (closes #70) and **close #71**. Low risk, already done.
2. **#72** — login state + profile page + redirect guard. High user value, auth already works; break into sub-tasks.
3. **#74** — onboarding Step 6 (Review & Confirm). Self-contained; depends on Figma import.
4. **#73** — add advisory/strategy services. **Blocked** on requester's finalized offering list.
5. **#51** — imprint page. **Deferred** to the website redesign.

> Note: counts and statuses are a 2026-06-23 snapshot of the upstream repo and will drift as issues/PRs change.
