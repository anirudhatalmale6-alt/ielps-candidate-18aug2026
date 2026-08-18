# IELPS — post-correction candidate, 18 August 2026

Prepared against the *Consolidated Conditional Approval, CEFR / Routing Resolution &
Immutable Learner UI Governance*, 18 August 2026.

---

## 1. Release identity

| | |
|---|---|
| Repository | `10495109/v0-ielps-platform-h4` |
| Branch | `visual-direction-20260815` |
| Reviewed pre-correction SHA | `5266b36a018c066472390d7161134353da1daf62` |
| **Post-correction SHA** | **`2b7966ca446919dddd928c5b5e1cf5e292e72624`** |
| Next.js `BUILD_ID` | `_WG8LUfYCrr51myvmCeor` |
| Source inventory | `source-inventory-sha256.txt` — 126 tracked files |
| Release manifest hash (SHA-256 of that inventory) | `80922b1a44af9f0087717a4cb46014142848a509766825468bb612dd5631a857` |
| Rollback pointer | release `ielps-a1-c2-learner-20260805-r4-discovery`, unit as recorded in section 7 |

Build, TypeScript and Lint: **clean** (`npm run build` exit 0, `tsc --noEmit` exit 0,
`npm run lint` exit 0).

---

## 2. What changed since the reviewed SHA

Exactly one file: `components/level-band.tsx`.

Section 8 settles the naming conflict. Checked before changing anything: the six
learner-facing names, the six reference descriptors and the six definitions were
**already exact** in `lib/cefr-levels.ts`, which is the single source the ladder and the
A1–C2 pages read from. The rejected set (`B2 Inter Plus`, `C1 Upper-Intermediate`,
`C2 Advanced`) does not appear anywhere in the tree.

One place did not conform. The level band on the Access Panel Home used the **course
title returned by the curriculum server** as its heading, so a visitor at A1 read
`A1 — Foundation English` and never saw the level name. That is the exact confusion
section 8 resolves. The two are now separate:

* the heading names the level from the canonical table — `A1 Beginner`;
* the reference descriptor is stated — `Breakthrough`;
* the course title is stated **as a course title** — `Course: Foundation English`.

The level name is now local and canonical, so it paints correctly before the server
answers and cannot drift from it. Nothing else in the band moved. No Section 5 example
URL was added. Nothing from the section 12 exclusion list was touched.

### Measured, all six levels, against the live curriculum

| ?level= | Heading | Descriptor | Course title (from the server) |
|---|---|---|---|
| A1 | A1 Beginner | Breakthrough | Foundation English |
| A2 | A2 Elementary | Waystage | Everyday Communicator |
| B1 | B1 Intermediate | Threshold | Independent English for Real-Life Progress |
| B2 | B2 Upper Intermediate | Vantage | Upper-Independent English for Work, Study and Mobility |
| C1 | C1 Advanced | Effective proficiency | Advanced English for Academic, Professional and Civic Impact |
| C2 | C2 Proficiency | Mastery | Mastery English for Elite Communication and High-Stakes Performance |

---

## 3. Section 4 — functional acceptance, re-run on the corrected build

Raw output in `functional.json`.

| Check | Result | Measured |
|---|---|---|
| No level band when no `?level=` is present | PASS | `#your-level` absent from the document |
| Valid `?level=A1`–`C2` renders the correct band | PASS | six headings, each begins with its own code |
| Exactly one primary `Continue with {LEVEL}` | PASS | 1 matching control in the band |
| Pathway identity precedes learner placement | PASS | the primary action targets `#pathways` |
| No direct level-page bypass into the Adult Lesson Player | PASS | no link from a level page to the player |
| Canonical `/api/...` only, no `/api/eilps` | PASS | `/api/auth/me`, `/api/auth/pathways`, `/api/auth/refresh`, `/api/curriculum/deep-catalog` |
| Unknown level route returns 404 | PASS | HTTP 404 for `/levels/d9/` |
| Build / TypeScript / Lint clean | PASS | all three exit 0 |

---

## 4. Section 14 — visual inheritance, measured against the live baseline

Each candidate page was loaded beside the closest approved page on
`https://eilps.com/learner/` and the same computed values were read off both. Raw
output in `inheritance.json` and `shell.json`; side-by-side images in `pairs/`.

### PASS — inherited directly

**Access Panel Home** (`/`) vs `https://eilps.com/learner/`

| Property | Baseline | Candidate |
|---|---|---|
| Page shell | `rgb(249,249,252)` | `rgb(249,249,252)` |
| Body font | Inter | Inter |
| Heading font | Bricolage Grotesque | Bricolage Grotesque |
| Text colour | `rgb(13,0,77)` | `rgb(13,0,77)` |
| Content width | 1216 px | 1216 px |
| Header | 73 px, `sticky`, `oklab(0.9830 0.0012 -0.0038 / 0.95)` | identical |
| Primary CTA | gold `rgb(255,206,0)`, 48 px, weight 900 | identical |
| Transition timing | 0.15 s | 0.15 s |
| PiP | `rgb(13,0,77)`, 999 px radius, 66 px | identical |

**Parent dashboard** (`/app/parents/dashboard/`) vs `https://eilps.com/learner/app/parents/dashboard`
— shell, sidebar, breadcrumb, heading font, card radius and the blue `rgb(56,96,190)`
controls at 16 px radius and 36 px height are identical. See
`pairs/B_parent_dashboard__baseline_left__candidate_right.jpg`.

**Keyword pop-up host** — the light shell, Inter body, Bricolage headings and the
purple `rgb(81,46,171)` pill controls match the live lesson player.

### Classification of every remaining difference

| Difference | Where | Class |
|---|---|---|
| Level band present | Access Panel Home with `?level=` | APPROVED FUNCTION DIFFERENCE — the band only exists when a level was chosen |
| `Sign in required` in place of sample rows | Parent dashboard | APPROVED FUNCTION DIFFERENCE — the truthfulness correction; the live baseline shows sample rows labelled *Sample* |
| Pending-content notice under a keyword | Keyword pop-up | APPROVED FUNCTION DIFFERENCE — section 5 |
| Single column below 640 px | all pages | RESPONSIVE DIFFERENCE |
| **Dark indigo shell + Plus Jakarta Sans headings** | **CEFR ladder, A1–C2 level pages** | **UNEXPLAINED VISUAL DIFFERENCE — BLOCK.** See section 5 |
| **Endpoint chips (`GET /api/...`) shown to learners** | Access Panel Home, Parent dashboard | **BLOCK under 2.1** — developer-facing copy. See section 5 |
| **Watermarked iStock comp** | CEFR ladder | **BLOCK under 12 / 15.** See section 5 |

### No-clutter measurement

Text density, measured as characters of visible text per 1000 px of page height:

| Page | Baseline | Candidate |
|---|---|---|
| Access Panel Home | 794 | 981 |
| Parent dashboard | 510 | 638 |
| Level page B2 | 569 | 469 |
| CEFR ladder | 569 | 942 |

The level page is calmer than its baseline. The ladder is the densest surface in the
release; it is one purpose and one hierarchy, but it carries all six levels with a
descriptor, a name and a full definition each, and its numbers sit well above the
learner system's own.

---

## 5. Three items that BLOCK, reported rather than guessed

**5.1 — The CEFR ladder and the A1–C2 level pages do not use the learner shell.**
Measured: shell `rgb(13,0,77)` dark indigo against the learner system's
`rgb(249,249,252)`, headings in **Plus Jakarta Sans** against **Bricolage Grotesque**.
Sections 10 and 11 require these pages to reuse the learner shell. They currently do
not — they came from the Access Panel design package approved on 16 August, which uses
its own dark treatment. Converting them is a real visual change to already-approved UI
and it moves a typeface, which section 12 excludes from this release. Two approved
sources disagree, so this is reported, not guessed. See
`pairs/C_cefr_ladder__baseline_left__candidate_right.jpg`.

**5.2 — The watermarked iStock comp is still on the ladder.** Sections 12 and 15
require it absent and the clean approved image retained. The image in the 16 August
approved package (`public/images/learners.jpg`) is byte-identical to the watermarked
comp, so no clean version exists in any source held here.
**CANONICAL SOURCE NOT FOUND** — the licensed file has to be supplied, or the photo
removed.

**5.3 — Developer-facing copy is visible to learners.** The mini-app cards on the
Access Panel Home and the panels on the Parent dashboard print endpoint chips such as
`POST /api/auth/register` and `GET /api/school/parent/dashboard`. Section 2.1 forbids
implementation or developer-facing copy on a learner surface. These arrived with the
supplied 14 August baseline rather than being introduced here, so they have been left
untouched pending a decision.

---

## 6. Public Landing — unchanged

Section 11. Measured on the server at 10:12 UTC on 18 August: the landing build
directory `/home/anirudhat/eilps/frontend/dist` has a modification time of
17 Aug 12:43 and **0 files modified since 00:00 on 18 August**. Nothing in this release
touches it. The legacy `EILPS` wording and the footer wordmark are deliberately
unchanged, per section 11.

---

## 7. Administrator handoff pack

Everything below is for the Administrator. Nothing here was attempted from this account,
which has no sudo and no root.

**Current unit — `/etc/systemd/system/eilps-learner.service`**

    [Unit]
    Description=IELPS immutable A1-C2 learner experience
    After=network.target eilps-web.service
    Wants=eilps-web.service

    [Service]
    Type=simple
    User=anirudhat
    Group=anirudhat
    WorkingDirectory=/home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery
    Environment=NODE_ENV=production
    Environment=PORT=4302
    Environment=DIST=/home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery/out
    ExecStart=/usr/bin/node /home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery/runtime/learner-server.mjs
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target

**Why the unit has to change.** The current release is a static export served by
`runtime/learner-server.mjs` from an `out/` directory. The candidate is a Next.js
**standalone** application with routes that are rendered on demand
(`/app/[slug]/placement`, `/app/[slug]/onboarding`) and which a static export cannot
produce. Section 10 permits this. It is a unit change, not a file swap.

**Proposed release directory (immutable, dated, never written to again)**

    /home/anirudhat/eilps/releases/ielps-access-panel-learner-20260818-2b7966c/

Contents: the built `.next/standalone/` tree with `.next/static` and `public/` copied
inside it, exactly as tested — that is,

    <release>/server.js
    <release>/.next/…            (standalone server bundle + static)
    <release>/public/…
    <release>/node_modules/…     (the minimal set the standalone build emits)

**Proposed unit — only the four marked lines differ**

    [Unit]
    Description=IELPS immutable A1-C2 learner experience
    After=network.target eilps-web.service
    Wants=eilps-web.service

    [Service]
    Type=simple
    User=anirudhat
    Group=anirudhat
    WorkingDirectory=/home/anirudhat/eilps/releases/ielps-access-panel-learner-20260818-2b7966c   # changed
    Environment=NODE_ENV=production
    Environment=PORT=4302
    Environment=HOSTNAME=127.0.0.1                                                                 # added
    Environment=IELPS_PANEL_BASE_PATH=/learner                                                     # added
    ExecStart=/usr/bin/node /home/anirudhat/eilps/releases/ielps-access-panel-learner-20260818-2b7966c/server.js   # changed
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target

`Environment=DIST=…` is removed — the standalone server does not read it.

**Runtime account and permissions.** Unchanged: `anirudhat:anirudhat`, port 4302,
loopback only, reverse proxy untouched. No new secret and no new environment file. No
database change.

**Controlled restart**

    systemctl daemon-reload
    systemctl restart eilps-learner
    systemctl --no-pager status eilps-learner

**Expected interruption.** The learner experience on port 4302 is unavailable for the
few seconds between stop and the first successful response. Nothing else is restarted:
not the API, not PostgreSQL, not nginx, not the host.

**Health checks and smoke tests** — all against the address a learner actually uses:

    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/access/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/levels/b2/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/app/parents/dashboard/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/levels/d9/     # expect 404
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/                        # landing, expect 200

**Rollback.** Restore the previous unit exactly as printed above and

    systemctl daemon-reload
    systemctl restart eilps-learner

The previous release directory is left in place untouched, so rollback is a unit
restoration only and takes the same few seconds.

---

## 8. What is deliberately not in this release

Typography. Trial-entitlement backend work. Database or schema migrations. Vocabulary
bulk authoring. Vocabulary image or audio generation. Stripe QA environment work.
Webhook work. New Studio backend routes. Signed-in rebranding, including
`hydrated live from the EILPS server` and the footer wordmark. Any unrelated colour,
icon, curriculum or backend business-logic change. The Section 5 example URLs.

---

## 9. Evidence index

* `pairs/` — baseline on the left, candidate on the right, desktop, five pages.
* `responsive/` — whole pages at desktop 1280, tablet 834 and mobile 390, with the
  keyword pop-up open and closed.
* `functional.json` — the section 4 measurements.
* `inheritance.json`, `shell.json` — the computed values behind section 4 of this
  document.
* `source-inventory-sha256.txt` — per-file SHA-256 of all 126 tracked source files.

Production is unchanged and stays unchanged until the exact commit
`2b7966ca446919dddd928c5b5e1cf5e292e72624` receives explicit DEPLOY APPROVED.
