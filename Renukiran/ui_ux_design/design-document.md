# RWF Learning Management System — UI/UX Design Document

**Version:** 2.0
**Date:** 2026-03-01
**Scope:** MVP1 — Enrollment & Batch Allocation
**Author:** UI/UX Design Phase

---

## Table of Contents

1. [Design Decisions Summary](#1-design-decisions-summary)
2. [Information Architecture & Navigation](#2-information-architecture--navigation)
3. [Design System & Visual Language](#3-design-system--visual-language)
4. [Page-by-Page Screen Designs](#4-page-by-page-screen-designs)
5. [Interaction Flows & State Diagrams](#5-interaction-flows--state-diagrams)
6. [Responsive Design](#6-responsive-design)
7. [Accessibility (WCAG 2.1 AA)](#7-accessibility-wcag-21-aa)
8. [Internationalization (i18n)](#8-internationalization-i18n)
9. [Appendix: Email Templates](#appendix-email-templates)

### Screenshot Artifacts

All high-fidelity mockup screenshots are located in `ui_ux_design/screenshots/`. Interactive HTML mockups are in `ui_ux_design/mockups/` and can be opened in any browser for inspection.

---

## 1. Design Decisions Summary

| Decision | Choice |
|----------|--------|
| Scope | MVP1: Enrollment & Batch Allocation |
| Device priority | Equal priority mobile + desktop |
| Languages | 8: English, Hindi, Telugu, Kannada, Tamil, Malayalam, Marathi, Bengali |
| Branding | Derive design system from existing logo |
| Email interactions | Token-based deep links, no login required |
| Admin batch UX | Table + checkboxes + bulk actions |
| Authentication | Phone + OTP for all users |

---

## 2. Information Architecture & Navigation

### 2.1 Site Map

```
RWF Learning Management System
├── / (Landing / Login)
│   ├── Phone + OTP Authentication
│   └── Language Selector
│
├── /admin (Admin Dashboard)
│   ├── /admin/dashboard .............. Overview & KPIs
│   ├── /admin/trainings .............. Training Management
│   │   ├── /admin/trainings/new ...... Create Training
│   │   └── /admin/trainings/:id ...... Training Detail
│   │       ├── Slots tab ............ Manage Timeslots
│   │       ├── Enrollments tab ...... View/Approve Enrollees
│   │       └── Attendance tab ....... Mark Attendance
│   ├── /admin/batches ................ Batch Allocation
│   │   └── /admin/batches/:id ....... Batch Detail & Slot Assignment
│   ├── /admin/users .................. User Management
│   │   ├── /admin/users/new ......... Add User
│   │   └── /admin/users/:id ......... User Profile & History
│   └── /admin/notifications ......... Email/Notification Log
│
├── /user (User Portal)
│   ├── /user/dashboard .............. My Dashboard
│   ├── /user/trainings .............. Browse Active Trainings
│   │   └── /user/trainings/:id ..... Training Detail + Enroll
│   ├── /user/my-enrollments ......... My Enrollments & Status
│   ├── /user/attendance ............. My Attendance History
│   └── /user/profile ................ My Profile
│
└── /respond/:token .................. Email Response (no auth)
    └── Opt Out / Wait confirmation page
```

### 2.2 Navigation Patterns

**Admin Layout:**
- Desktop: Persistent left sidebar (collapsible) with icon + label navigation. Top bar with language switcher, notifications bell, profile avatar
- Mobile: Bottom navigation bar with 4 key items (Dashboard, Trainings, Users, More). "More" opens a drawer with remaining items

**User Layout:**
- Desktop: Horizontal top navigation with logo, nav links, language switcher, profile. Simpler than admin
- Mobile: Bottom navigation bar with 4 items (Home, Trainings, My Enrollments, Profile). Larger touch targets

**Key UX Principle:** The user-facing portal uses larger fonts, bigger touch targets, simpler language, and fewer options per screen than the admin interface — designed for users with limited digital literacy.

---

## 3. Design System & Visual Language

### 3.1 Color Palette

Semantic color system derived from the Renukiran Welfare Foundation logo. The logo features deep blue human figures symbolizing community and trust, with a bright green swoosh and wordmark representing growth and empowerment.

| Token | Role | Value | Usage |
|-------|------|-------|-------|
| `--brand-blue` | Brand primary | `#2B5EA7` (deep blue) | CTAs, active nav, key actions, primary buttons |
| `--brand-blue-dark` | Primary shade | `#1E4A8A` | Pressed/hover states, focus rings |
| `--brand-blue-light` | Primary tint | `#EBF0F9` | Selected rows, active backgrounds, highlights |
| `--brand-blue-50` | Lightest blue | `#F0F4FA` | Page backgrounds, subtle accents |
| `--brand-green` | Brand accent | `#7CB518` (bright green) | Accent buttons, enroll CTAs, positive accents |
| `--brand-green-dark` | Accent shade | `#5E8A12` | Pressed states for accent elements |
| `--brand-green-light` | Accent tint | `#F0F8E4` | Success-adjacent backgrounds |
| `--neutral-50` | Lightest gray | `#FAFAFA` | Page backgrounds |
| `--neutral-100` | Light gray | `#F4F4F5` | Card backgrounds, table stripes |
| `--neutral-200` | Border gray | `#E4E4E7` | Borders, dividers |
| `--neutral-500` | Mid gray | `#71717A` | Placeholder text, disabled |
| `--neutral-600` | Body text | `#52525B` | Secondary text |
| `--neutral-900` | Headings | `#18181B` | Headings, primary text |
| `--success` | Positive | `#16A34A` | Approved, confirmed, good attendance |
| `--warning` | Caution | `#EAB308` | Waitlisted, below 70% threshold |
| `--danger` | Error/alert | `#DC2626` | Rejected, low attendance, errors |
| `--info` | Informational | `#2563EB` | Links, info tooltips |

**Rationale:** The blue-green palette directly mirrors the Renukiran logo. Deep blue (`#2B5EA7`) as primary conveys institutional trust and reliability — critical for an NGO serving underserved communities. Bright green (`#7CB518`) as accent conveys growth and opportunity — used strategically on enrollment CTAs and positive outcomes. The dual-brand approach gives the admin interface a professional blue tone while the user-facing portal uses warmer green accents for approachability. All text/background combinations meet WCAG AA contrast ratios (4.5:1 minimum).

### 3.2 Typography

| Level | Font | Size (Desktop / Mobile) | Weight | Usage |
|-------|------|------------------------|--------|-------|
| Display | Inter | 32px / 24px | 700 | Page titles |
| H1 | Inter | 24px / 20px | 600 | Section headers |
| H2 | Inter | 20px / 18px | 600 | Card headers |
| H3 | Inter | 16px / 16px | 600 | Sub-sections |
| Body | Inter | 16px / 14px | 400 | Default text |
| Body-sm | Inter | 14px / 13px | 400 | Table cells, captions |
| Label | Inter | 12px / 12px | 500 | Form labels, badges |

**Primary font: Inter** — chosen for its excellent readability at small sizes, tabular number support (critical for attendance percentages and slot counts), and variable font support for optimal loading. **Fallback for Indic scripts: Noto Sans** (Devanagari, Telugu, Kannada, Tamil, Malayalam, Bengali variants) — provides unified glyphs across all 8 target scripts with consistent x-height. Font loading strategy: preload Inter Latin + Noto Sans for user's selected script, lazy-load others.

### 3.3 Spacing & Grid

- Base unit: 4px
- Common spacings: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64px
- Grid: 12-column on desktop (max-width 1280px), 4-column on mobile
- Breakpoints: `sm: 640px`, `md: 768px`, `lg: 1024px`, `xl: 1280px`
- Touch targets: Minimum 44x44px for all interactive elements (WCAG 2.5.5)

### 3.4 Elevation & Shadows

| Level | Shadow | Usage |
|-------|--------|-------|
| 0 | None | Flat elements |
| 1 | `0 1px 3px rgba(0,0,0,0.1)` | Cards, table containers |
| 2 | `0 4px 12px rgba(0,0,0,0.1)` | Dropdowns, popovers |
| 3 | `0 8px 24px rgba(0,0,0,0.12)` | Modals, dialogs |

### 3.5 Border Radius

- Small: 6px (buttons, inputs, badges)
- Medium: 8px (cards, panels)
- Large: 12px (modals, featured cards)
- Full: 9999px (avatars, pills)

### 3.6 Component Library (Core Components for MVP1)

| Component | Variants | Notes |
|-----------|----------|-------|
| Button | Primary, Secondary, Outline, Ghost, Danger | Min height 44px, loading state with spinner |
| Input | Text, Phone, OTP (6-digit segmented) | Clear labels, inline validation, helper text |
| Select/Dropdown | Single, Multi | Search-enabled for long lists |
| Table | Sortable, filterable, paginated | Checkbox column, bulk action bar |
| Card | Training card, Stat card, User card | Consistent padding, elevation-1 |
| Badge | Status (Approved/Waitlisted/Rejected/Pending) | Color-coded per semantic palette |
| Dialog/Modal | Confirmation, Form, Alert | Backdrop, trap focus, escape to close |
| Toast/Notification | Success, Error, Warning, Info | Auto-dismiss with progress bar |
| Tabs | Horizontal | Used in training detail views |
| Avatar | Image, Initials fallback | 32/40/48px sizes |
| Language Picker | Dropdown with script name in native text | e.g., "hindi", "telugu" |
| OTP Input | 6-digit segmented boxes | Auto-advance, paste support |
| Empty State | Illustration + CTA | For tables/lists with no data |
| Skeleton Loader | Content placeholder | Pulse animation during load |

---

## 4. Page-by-Page Screen Designs

Each screen includes a wireframe for structural reference and a link to its high-fidelity screenshot mockup.

---

### 4.1 Authentication — Login Screen (`/`)

**Screenshot:** [`docs/screenshots/01-login.png`](screenshots/01-login.png)
**HTML Mockup:** [`docs/mockups/01-login.html`](mockups/01-login.html)

```
+---------------------------------------------------+
|  [Language: English v]              top-right      |
|                                                    |
|            +---------------+                       |
|            |  RWF Logo     |                       |
|            +---------------+                       |
|        Renukiran Learning Portal                   |
|                                                    |
|  +---------------------------------------------+  |
|  |  Enter Mobile Number                        |  |
|  |  +----------+------------------------+      |  |
|  |  | +91  v   |  98XXXXXXXX            |      |  |
|  |  +----------+------------------------+      |  |
|  |                                             |  |
|  |  +---------------------------------------+  |  |
|  |  |        Send OTP                       |  |  |
|  |  +---------------------------------------+  |  |
|  |                                             |  |
|  |  New user? Contact your center admin        |  |
|  +---------------------------------------------+  |
|                                                    |
|  ----------- Powered by RWF -----------            |
+---------------------------------------------------+
```

**UX Design Rationale:**
- **Single-purpose screen.** Only one action is possible: enter phone number. This eliminates confusion for users with limited digital literacy. No registration link, no social login — just phone + OTP.
- **Language selector is pre-auth.** Placed in the top-right corner and available before any interaction, so users can switch to their native language (Hindi, Telugu, etc.) immediately. Persisted in localStorage across sessions.
- **Logo with human figures.** The abstract SVG figures from the Renukiran logo appear above the brand name, reinforcing the NGO's community identity. "Renukiran" rendered in brand green, "WELFARE FOUNDATION" in brand blue.
- **+91 country code prefix.** Displayed in a separate box to the left of the phone input, defaulting to India. This prevents input errors and makes the phone field feel familiar.
- **"New here? Contact your training center"** — Deliberately avoids a self-registration flow. Admin-controlled user creation ensures data quality and prevents unauthorized access. The link provides center contact info.
- **Subtle gradient background.** Blue-50 to white gradient creates depth without distraction. The centered card with shadow-lg provides a clear focal point.

**Interaction Details:**
- Country code defaults to +91 (India), dropdown available for others
- Phone input accepts 10 digits, auto-formats with space (98765 43210)
- "Send OTP" button remains disabled until 10 digits are entered
- On submit: loading spinner on button, then redirect to OTP screen
- Error state: red border on input + "Phone number not registered. Contact your center."

---

### 4.2 Authentication — OTP Verification

**Screenshot:** [`docs/screenshots/02-otp-verify.png`](screenshots/02-otp-verify.png)
**HTML Mockup:** [`docs/mockups/02-otp-verify.html`](mockups/02-otp-verify.html)

```
+---------------------------------------------------+
|  <- Back                                           |
|                                                    |
|          Enter verification code                   |
|    Sent to +91 98XXX XXXXX                         |
|                                                    |
|      +---+ +---+ +---+ +---+ +---+ +---+          |
|      |   | |   | |   | |   | |   | |   |          |
|      +---+ +---+ +---+ +---+ +---+ +---+          |
|                                                    |
|      +---------------------------------------+     |
|      |          Verify                       |     |
|      +---------------------------------------+     |
|                                                    |
|      Didn't receive? Resend in 0:28                |
+---------------------------------------------------+
```

**UX Design Rationale:**
- **Lock icon** centered above the heading provides a visual security cue — users understand they're in a verification step.
- **Masked phone number** (98765 XXXXX) confirms the correct number was entered without exposing the full number.
- **6 segmented input boxes** (48x56px each) — each box holds one digit. Filled boxes show the digit with a blue border and light blue background. The active box has a focus glow. Remaining boxes show a dot placeholder. This is more intuitive than a single text field for users unfamiliar with OTP flows.
- **Auto-advance behavior.** After entering a digit, focus automatically moves to the next box. Supports clipboard paste of the full OTP code.
- **Back arrow** (top-left) returns to login screen to re-enter phone number if wrong.
- **Resend countdown** (30 seconds) prevents accidental spam. Timer shown in brand-blue for visibility. After countdown, "Resend" becomes a clickable link.

**Interaction Details:**
- OTP fields auto-advance on digit entry, support clipboard paste
- Resend countdown: 30 seconds, then "Resend OTP" link becomes active
- After successful OTP, system determines role (admin/user) from the database and redirects to the appropriate portal
- After 3 failed attempts: "Too many attempts. Try again in 5 minutes." message

### 4.3 Admin Dashboard (`/admin/dashboard`)

**Screenshot:** [`docs/screenshots/03-admin-dashboard.png`](screenshots/03-admin-dashboard.png)
**HTML Mockup:** [`docs/mockups/03-admin-dashboard.html`](mockups/03-admin-dashboard.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Dashboard                      [bell] [EN v] [avatar]| |
| |        |  +------------------------------------------------------+ |
| | Dash   |  |                                                      | |
| | Train  |  |  +----------+ +----------+ +----------+ +----------+ | |
| | Users  |  |  |Active    | |Total     | |Pending   | |Waitlist  | | |
| | Batch  |  |  |Trainings | |Enrolled  | |Approvals | |Count     | | |
| | Notif  |  |  |    5     | |   127    | |    23    | |   14     | | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |                                                      | |
| |        |  |  Recent Enrollments           [View All ->]          | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |  | Name      | Training   | Slot Pref | Status   |  | |
| |        |  |  +-----------+------------+-----------+----------+  | |
| |        |  |  | Priya S.  | Stitching  | Morning   | Pending  |  | |
| |        |  |  | Rahul K.  | Computers  | Evening   | Approved |  | |
| |        |  |  | Meena D.  | Beauty     | Morning   | Waitlist |  | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |                                                      | |
| |        |  |  Trainings Overview                                  | |
| |        |  |  +----------------------------------------------+   | |
| |        |  |  | Stitching Basic  ============--  18/20        |   | |
| |        |  |  | Computer Fund.   ==============- 19/20        |   | |
| |        |  |  | Beauty Basic     ======--------  12/20        |   | |
| |        |  |  +----------------------------------------------+   | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**UX Design Rationale:**
- **260px persistent sidebar** with Renukiran branding at top ("RWF" in green, "WELFARE FOUNDATION" in blue). Active nav item has a 3px blue left border and light blue background for clear wayfinding. Navigation items are 44px tall for easy click/touch targets.
- **Four stat cards** at top give an instant operational snapshot. Each card is clickable — "Pending Approvals" navigates directly to the batch allocation screen. Numbers are displayed at 32px bold for at-a-glance reading. Each card has a subtle colored icon background matching its semantic meaning.
- **Recent Enrollments table** (60% width) shows the 5 latest enrollments with color-coded status badges (Pending=yellow, Approved=green, Waitlisted=orange). "View All" link provides quick access to full enrollment management.
- **Training Capacity bars** (40% width) use color-coded progress bars — green when under 60% full, yellow at 60-90%, red above 90%. This instantly flags which trainings are nearing capacity and need admin attention.
- **Attendance Alerts** section below the fold shows users below the 70% threshold with a one-click "Send Alert" button. Warning icon and red percentage text make at-risk users immediately visible.
- **Top bar** includes language selector (persistent across all admin pages), notification bell with red badge count, and admin avatar with initials.

**Dashboard Widgets:**
1. Stat Cards (top row) — Active trainings, Total enrolled, Pending approvals, Waitlist count. Clickable — navigates to relevant list
2. Recent Enrollments Table — Last 5 enrollments with quick status. "View All" goes to full enrollment management
3. Training Capacity Bars — Visual fill indicator per training (green->yellow->red as capacity fills)
4. Attendance Alerts (below fold) — Users below 70% threshold, with "Send Alert" quick action

---

### 4.4 Training Management (`/admin/trainings`)

**Screenshot (List):** [`docs/screenshots/04-admin-trainings.png`](screenshots/04-admin-trainings.png)
**Screenshot (Add Modal):** [`docs/screenshots/05-admin-add-training.png`](screenshots/05-admin-add-training.png)
**HTML Mockups:** [`04-admin-trainings.html`](mockups/04-admin-trainings.html), [`05-admin-add-training.html`](mockups/05-admin-add-training.html)

**UX Design Rationale:**
- **Card-based list** (not a table) for trainings — each card shows a category icon on a colored background (yellow for stitching, blue for computers, pink for beauty, green for design), making visual scanning fast. Status badges (Active=green, Upcoming=blue, Closed=gray) appear next to the title.
- **Capacity progress bar** on each card gives immediate visual feedback on slot fill level without needing to open the training detail.
- **Search + Filter + Sort toolbar** — search is real-time, filters allow narrowing by status or category, sort allows ordering by name/capacity/date.
- **"+ Add Training" button** (brand blue, top-right) opens a centered modal overlay with a blurred/dimmed backdrop, keeping the user's context visible behind it.
- **Add Training Modal** (560px max-width) has clear field grouping: identity (name, category), logistics (duration, max slots), scheduling (timeslots with morning/evening time pickers), content (description), and status (radio buttons). "+ Add Timeslot" link allows adding custom timeslots beyond morning/evening.

**Training List Details:**
- Name, category icon, status badge (Active/Upcoming/Closed)
- Duration, slot capacity (used/total) with progress bar
- Timeslot details (Morning/Evening times)
- Actions: View Details, Edit, overflow menu (Delete, Clone, Close)

**Add/Edit Training Modal Fields:**
- Training Name (required)
- Category (dropdown: Stitching, Computers, Beauty, Design, etc.)
- Duration (months), Max Slots per Batch (default 20)
- Timeslots: Morning (start-end), Evening (start-end), with [+ Add Timeslot]
- Description (textarea)
- Status: Active / Upcoming / Closed (radio buttons)

---

### 4.5 Batch Allocation (`/admin/batches/:id`) — Core Admin Workflow

**Screenshot (Table):** [`docs/screenshots/06-admin-batch-allocation.png`](screenshots/06-admin-batch-allocation.png)
**Screenshot (Confirm Dialog):** [`docs/screenshots/07-admin-batch-confirm-dialog.png`](screenshots/07-admin-batch-confirm-dialog.png)
**HTML Mockups:** [`06-admin-batch-allocation.html`](mockups/06-admin-batch-allocation.html), [`07-admin-batch-confirm-dialog.html`](mockups/07-admin-batch-confirm-dialog.html)

```
+------------------------------------------------------------------------+
|  Stitching Basic - Batch Allocation                                    |
|  Morning Slot: 18/20 filled | Evening Slot: 15/20 filled              |
|                                                                        |
|  +- Bulk Actions ---------------------------------------------------+  |
|  | With selected (3): [Approve] [Waitlist] [Reject] [Send Email]   |  |
|  +------------------------------------------------------------------+  |
|                                                                        |
|  +- Filter ---------------------------------------------------------+  |
|  | Status: [All v]  Slot Pref: [All v]  Search: [               ]  |  |
|  +------------------------------------------------------------------+  |
|                                                                        |
|  +----+----------+----------+----------+-----------+--------+------+   |
|  | [] | Name     | Phone    | Pref 1   | Pref 2    | Status | Date |   |
|  +----+----------+----------+----------+-----------+--------+------+   |
|  | [x]| Priya S. | 98XXX123 | Morning  | Evening   | Pend   | Feb 1|   |
|  | [x]| Kavita R.| 98XXX456 | Morning  | Morning   | Pend   | Feb 2|   |
|  | [] | Sunita M.| 98XXX789 | Evening  | Morning   | Appr   | Jan28|   |
|  | [x]| Anita K. | 98XXX012 | Morning  | Evening   | Pend   | Feb 5|   |
|  | [] | Radha P. | 98XXX345 | Evening  | Evening   | Wait   | Feb 8|   |
|  +----+----------+----------+----------+-----------+--------+------+   |
|                                                                        |
|  <- 1 2 3 ->                    Showing 1-20 of 34 enrollees          |
|                                                                        |
|  +------------------------------------------------------------------+  |
|  |  [Send Confirmation Emails]    [Send Waitlist Notifications]     |  |
|  +------------------------------------------------------------------+  |
+------------------------------------------------------------------------+
```

**UX Design Rationale — This is the most critical screen in the entire application:**
- **Slot capacity indicators at top** — two cards showing Morning (18/20) and Evening (15/20) with progress bars. These update in real-time as the admin approves users, providing constant awareness of remaining capacity. Yellow bar at 90%, green at 75%.
- **Sticky bulk action bar** — appears only when rows are selected. Blue-light background (`#EBF0F9`) makes it visually distinct. Shows selection count ("3 selected") with color-coded action buttons: green Approve, yellow-outline Waitlist, red-outline Reject, neutral Email. This bar stays visible when scrolling through long lists.
- **Default sort by enrollment date** — implements the FCFS (First Come First Serve) requirement. The earliest enrollees appear first, making the admin's decision-making intuitive: work top-to-bottom.
- **Pref 1 and Pref 2 columns** — the two slot preference columns are the key differentiator of this screen. Admins can see at a glance which slot each user prefers and their backup choice, enabling informed allocation decisions.
- **Selected rows highlighted** — checked rows get a light blue background tint, making the current selection visually obvious even in long lists. The header checkbox shows an indeterminate state (dash) when some rows are selected.
- **Confirmation dialog** (07 screenshot) — prevents accidental bulk actions. Lists each user with their assigned slot using arrow notation (Priya Sharma -> Morning Slot). Includes a yellow warning box when a slot is nearly full. Checkbox to "Send confirmation emails immediately" defaults to checked for efficiency.

**Interaction Flows:**
1. Select users via checkboxes -> sticky bulk action bar appears at top
2. Approve -> assigns user to Pref 1 slot. If Pref 1 full, dialog: "Assign to Pref 2?"
3. Waitlist -> marks as waitlisted, user remains in queue
4. Reject -> optional reason dialog
5. Send Confirmation Emails -> emails all newly Approved users
6. Send Waitlist Notifications -> sends Opt Out / Wait deep-link emails to all Waitlisted users
7. Date column shows enrollment timestamp for FCFS ordering (default sort)
8. Slot capacity bars at top update in real-time as admins approve/remove

---

### 4.6 Attendance Management (`/admin/trainings/:id` -> Attendance Tab)

**Screenshot:** [`docs/screenshots/08-admin-attendance.png`](screenshots/08-admin-attendance.png)
**HTML Mockup:** [`docs/mockups/08-admin-attendance.html`](mockups/08-admin-attendance.html)

```
+--------------------------------------------------------------------+
|  Stitching Basic - Attendance                                      |
|  +-------------------------------------------------------------+  |
|  | Date: [<] February 28, 2026 [>]     Slot: [Morning v]      |  |
|  +-------------------------------------------------------------+  |
|                                                                    |
|  Class 15 of 48 | Marked: 16/18 | [Mark All Present]              |
|                                                                    |
|  +----------+-----------+----------+--------------+------------+   |
|  | Name     | Attend.%  | Today    | Streak       | Alert      |   |
|  +----------+-----------+----------+--------------+------------+   |
|  | Priya S. | 93% green | oP oA   | 12 days      |            |   |
|  | Kavita R.| 68% red   | oP oA   | 0 (absent 2) | Below 70%  |   |
|  | Sunita M.| 85% green | *P oA   | 8 days       |            |   |
|  | Anita K. | 72% yellow| oP oA   | 1 day        | At risk    |   |
|  +----------+-----------+----------+--------------+------------+   |
|                                                                    |
|  P = Present  A = Absent  (radio buttons per row)                  |
|                                                                    |
|  +-------------------------------------------------------------+  |
|  | [Save Attendance]  [Send Low Attendance Alerts (2 users)]   |  |
|  +-------------------------------------------------------------+  |
+--------------------------------------------------------------------+
```

**UX Design Rationale:**
- **Tab-based navigation** within the training detail view (Slots | Enrollments | Attendance). The active "Attendance" tab has a blue underline indicator. This keeps all training-related management in one place.
- **Date navigator** with left/right arrows and a calendar icon allows quick day-by-day navigation. The current date is displayed prominently. Slot dropdown (Morning/Evening) filters the attendance list.
- **"Class 15 of 48 | Marked: 16/18"** — progress context tells the admin exactly where they are in the training cycle and how many students they've already marked today.
- **Attendance % column** uses a three-tier color system with both a colored dot and a mini progress bar: green dot + green bar (>80%), yellow dot + yellow bar (70-80%), red dot + red bar (<70%). This dual encoding ensures accessibility — the information is conveyed through color, shape, AND text.
- **Present/Absent radio buttons** are styled as clear, large circular buttons (green outline for Present selected, red for Absent). The visual distinction prevents marking errors.
- **Streak column** shows consecutive present days (with a checkmark) or consecutive absent days. This provides motivational context for the admin.
- **Alert column** shows "Below 70%" (red badge) and "At risk" (yellow badge) warnings only for users who need attention.
- **"Send Low Attendance Alerts" button** — appears in danger-outline style only when users are below 70%. Shows the count "(2 users)" so the admin knows the scope before clicking. Placed next to "Save Attendance" for a natural workflow: mark attendance -> save -> send alerts.
- **"Mark All Present" button** — convenience for days when most students are present; admin can then mark individual absences.

---

### 4.7 User Management (`/admin/users`)

**Screenshot:** [`docs/screenshots/09-admin-users.png`](screenshots/09-admin-users.png)
**HTML Mockup:** [`docs/mockups/09-admin-users.html`](mockups/09-admin-users.html)

**UX Design Rationale:**
- **Avatar initials** next to each name provide visual anchoring and make the table feel more personal than a plain text list.
- **Role badges** — "Admin" in blue pill, "User" in gray pill — make role identification instant. Admins are visually distinct from regular users.
- **Status badges** — Active (green dot + green badge), Waitlisted (orange), Inactive (gray) — use the same color vocabulary as the rest of the system for consistency.
- **Edit/Delete action buttons** per row (icon buttons) for quick user management. Delete requires confirmation dialog.
- **Bulk actions** (Delete, Export CSV) appear at the bottom. Export CSV enables admins to generate reports for donor/CSR dashboards.

**Details:**
- Data table with search, role filter, status filter
- Columns: checkbox, Name (with avatar), Phone, Role (Admin/User), Training, Status (Active/Inactive/Waitlist)
- Bulk actions: Delete, Export CSV
- Add User modal: Name, Phone, Email (optional), Role, Center
- Pagination: "Showing 1-8 of 142 users"

---

### 4.8 User Portal — Browse Trainings (`/user/trainings`)

**Screenshot:** [`docs/screenshots/10-user-browse-trainings.png`](screenshots/10-user-browse-trainings.png)
**HTML Mockup:** [`docs/mockups/10-user-browse-trainings.html`](mockups/10-user-browse-trainings.html)

```
+--------------------------------------------------------------+
|  +- RWF Logo -+  Trainings   My Courses   Profile  [lang v] |
|  +------------+                                              |
|                                                              |
|  Available Trainings                                         |
|  Choose a course to start your learning journey              |
|                                                              |
|  +---------------------------------------------------+      |
|  |  Stitching - Basic                                |      |
|  |  Learn foundational stitching skills              |      |
|  |  Duration: 3 months                               |      |
|  |                                                   |      |
|  |  Morning: 9:00-11:00  |  Evening: 4:00-6:00      |      |
|  |  Seats: 2 remaining       14 remaining            |      |
|  |                                                   |      |
|  |  [            Enroll Now            ]             |      |
|  +---------------------------------------------------+      |
|                                                              |
|  +---------------------------------------------------+      |
|  |  Computer Fundamentals                            |      |
|  |  Learn computers, MS Office, internet basics      |      |
|  |  Duration: 2 months                               |      |
|  |                                                   |      |
|  |  Morning: 10:00-12:00 |  Evening: 3:00-5:00      |      |
|  |  Seats: 1 remaining       5 remaining             |      |
|  |                                                   |      |
|  |  [            Enroll Now            ]             |      |
|  +---------------------------------------------------+      |
+--------------------------------------------------------------+
```

**UX Design Rationale — This is the primary user-facing screen:**
- **Simpler navigation.** Top bar (not sidebar) with only 3 nav items: Trainings, My Courses, Profile. The Renukiran logo text ("Renukiran" in green, "Learning Portal" in blue) provides brand presence without a heavy sidebar.
- **Language selector in native script.** Displayed as "hindi" (in Devanagari) with a dropdown arrow, not "Hindi" in English. This helps non-English speakers identify and use the switcher.
- **Warm, encouraging copy.** "Choose a course to start your learning journey" — welcoming language for first-time users from underserved communities.
- **Large, spacious training cards.** Each card has generous padding (24px), full-width layout, clear hierarchy: title (bold, 20px) -> description -> metadata -> slot availability -> CTA. No cognitive overload.
- **Slot availability with urgency indicators.** "2 seats left" in orange badge creates appropriate urgency. "14 seats left" in green feels relaxed. "FULL" in gray with "Registration Full" badge clearly communicates unavailability. "1 seat left" in red badge creates high urgency.
- **Full-width green "Enroll Now" button.** Brand green (`#7CB518`) is used for the primary user action across the portal. The button is 48px tall — oversized for easy touch targets. On the "upcoming" card (Design Basics), the button is replaced with a muted "Coming Soon" text.
- **Max-width 960px centered.** Narrower than the admin portal (1280px) to maintain comfortable reading width and a less overwhelming feel.

---

### 4.9 Enrollment Flow (`/user/trainings/:id`)

**Screenshot (Form):** [`docs/screenshots/11-user-enrollment.png`](screenshots/11-user-enrollment.png)
**Screenshot (Success):** [`docs/screenshots/12-user-enrollment-success.png`](screenshots/12-user-enrollment-success.png)
**HTML Mockups:** [`11-user-enrollment.html`](mockups/11-user-enrollment.html), [`12-user-enrollment-success.html`](mockups/12-user-enrollment-success.html)

**UX Design Rationale:**
- **"Back to Trainings" link** at top-left provides clear escape route. Users never feel trapped.
- **Training header card** repeats the training name, description, and key metadata (duration, class count, start date) so the user has full context while making their slot choice.
- **Large radio cards** (not small radio buttons) — each slot option is a full-width clickable card (approx 140px tall) with the slot name (MORNING/EVENING), time range in bold, and remaining seats badge. The selected card gets a blue border and light blue background tint. This design is critical for low-digital-literacy users who may struggle with small radio inputs.
- **Two selection groups** — "Preferred Slot (Option 1)" and "Backup Slot (Option 2)" are clearly labeled with red asterisks for required fields. The user selects one slot in each group.
- **Info box below the submit button** (light blue background, info icon) explains: "Your enrollment will be reviewed by the admin. You'll receive a confirmation via SMS." This sets correct expectations — the user won't wonder why they don't have instant access.
- **Success page** (12 screenshot) — centered card with a large green CSS checkmark animation. Shows enrollment details (training, preferred slot, backup slot) followed by a numbered "What happens next?" section with 3 steps in blue numbered circles. This reduces anxiety by explaining the process.

---

### 4.10 User Dashboard (`/user/dashboard`)

**Screenshot:** [`docs/screenshots/13-user-dashboard.png`](screenshots/13-user-dashboard.png)
**HTML Mockup:** [`docs/mockups/13-user-dashboard.html`](mockups/13-user-dashboard.html)

**UX Design Rationale:**
- **Personal welcome.** "Welcome back, Priya!" with subtitle "Here's your learning progress" — warm, personal, encouraging.
- **Current training card** prominently displays the training name, slot assignment, date range, and a green "Confirmed" badge. This is the first thing the user sees — answering the question "Am I enrolled? In what? When?"
- **Two circular progress rings** using CSS conic-gradient — Attendance (87%, green ring with "Good!" tag) and Progress (31%, blue ring with "Class 15/48" tag). The circular visualization is universally understood and creates a sense of accomplishment. Color-coding: green >80%, yellow 70-80%, red <70%.
- **Attendance calendar** — month-view grid (Mon-Sun columns) with color-coded dots on each day: green dots for present, red dots for absent, empty circles for no-class days (weekends/holidays). The legend at the bottom shows totals: "Present (23) Absent (2) No class." This gives users a complete at-a-glance view of their attendance pattern. Month navigation arrows allow viewing history.
- **"Next Class" info bar** — blue-tinted bar at bottom with calendar icon: "Next Class: March 1, 9:00 AM." Answers the most common user question.

---

### 4.11 Email Response Page (`/respond/:token`)

**Screenshot (Choice):** [`docs/screenshots/14-email-response.png`](screenshots/14-email-response.png)
**Screenshot (Confirmation):** [`docs/screenshots/15-email-response-confirm.png`](screenshots/15-email-response-confirm.png)
**HTML Mockups:** [`14-email-response.html`](mockups/14-email-response.html), [`15-email-response-confirm.html`](mockups/15-email-response-confirm.html)

**UX Design Rationale:**
- **No navigation bar, no login required.** This page is accessed from an email link with a secure token. The standalone layout (centered card, no nav, gradient background matching the login page) makes it feel like a dedicated response form, not a full portal page. This eliminates friction for users who may not remember their login.
- **RWF branding at top** (same logo treatment as login page) — provides trust and brand recognition.
- **Personalized greeting.** "Hi Radha," — uses the user's first name from the database. Context paragraph explains the situation clearly: which training, which slot, and the waitlist position ("#3 on the waitlist" in bold blue).
- **Two large action cards** (not small buttons) — each card is a full-width clickable area with an icon, title in bold, and explanation text. "Wait for Next Batch" has a blue border and an hourglass icon. "Opt Out" has a neutral border and an X icon. The explanation text reduces the cognitive load of the decision.
- **Token validity notice** at bottom — "This link is valid for 7 days" prevents confusion about expired links.
- **Confirmation page** (15 screenshot) — clean centered card with a blue checkmark, confirming the response was saved and what the user chose. "Go to Renukiran Portal" button provides an optional next step.

---

### 4.12 Notification Log (`/admin/notifications`)

**Screenshot:** [`docs/screenshots/16-admin-notifications.png`](screenshots/16-admin-notifications.png)
**HTML Mockup:** [`docs/mockups/16-admin-notifications.html`](mockups/16-admin-notifications.html)

**UX Design Rationale:**
- **Comprehensive filter row** — four filters (Type, Status, Date Range, Search) allow admins to quickly find specific notifications. This is important for auditing and troubleshooting email delivery issues.
- **Color-coded type badges** — Waitlist (blue), Low Attendance (red), Confirmation (green), General (gray) — make visual scanning of the table fast.
- **Status column uses colored text** — Sent (gray), Delivered (blue), Opened (green), Failed (red) — providing delivery state at a glance.
- **Response column** — for waitlist notifications, shows the user's response: Waiting (blue badge), Opted Out (gray badge), Pending (yellow badge). This closes the feedback loop — admins can see who responded to waitlist emails without leaving the page.
- **"Resend" action** for failed notifications — appears in red text only on failed rows, providing a one-click recovery action.
- **Summary bar** at bottom of the table with totals: "Total: 47 | Delivered: 38 | Opened: 22 | Failed: 2" — gives admins a quick health check on their notification pipeline.

**Details:**
- Data table with filters: Type (Confirmation/Waitlist/Low Attendance/General), Status (Sent/Delivered/Opened/Failed), Date range
- Columns: Date, Recipient, Type, Status, Response (for waitlist emails), Actions
- Pagination with page numbers
- Summary bar with delivery statistics

---

## 5. Interaction Flows & State Diagrams

### 5.1 User Enrollment Flow

```
User Login (OTP) -> Browse Trainings -> Select Training + Slot Preferences -> Submit Enrollment
    |
    v
Status: PENDING
    |
    +--- Admin Approves ---> APPROVED ---> Confirmation Email Sent
    |
    +--- Admin Waitlists --> WAITLISTED -> Waitlist Email Sent (Opt Out / Wait)
    |                                          |
    |                                +-----+-------+
    |                                |             |
    |                           WAITING      OPTED_OUT
    |                           (next batch)
    |
    +--- Admin Rejects ---> REJECTED
```

### 5.2 Enrollment Status State Machine

States: PENDING -> APPROVED | WAITLISTED | REJECTED
- WAITLISTED -> WAITING (user chose "Wait for Next Batch" via email)
- WAITLISTED -> OPTED_OUT (user chose "Opt Out" via email)
- WAITING -> APPROVED (slot opens or new batch created)

### 5.3 Admin Batch Allocation Flow

1. Admin opens Batch Allocation for a training
2. Reviews enrollee list (sorted by enrollment date = FCFS)
3. Selects users via checkboxes
4. Approve: Check slot capacity -> If Pref 1 has space, assign to Pref 1. If Pref 1 full, prompt "Assign to Pref 2?" -> Yes: assign to Pref 2 / No: move to Waitlist
5. Waitlist: Mark as WAITLISTED
6. Reject: Optional reason dialog -> Mark as REJECTED
7. Send Confirmation Emails: batch email to all APPROVED users
8. Send Waitlist Notifications: email with Opt Out / Wait deep-link buttons

### 5.4 Attendance Alert Flow

1. Admin marks attendance for a class (date + slot)
2. System recalculates attendance % for each user
3. Users below 70% flagged with alert indicator
4. Admin clicks "Send Low Attendance Alerts"
5. Confirmation dialog: "Send alerts to N users below 70%?"
6. Email sent with attendance warning
7. Notification logged

### 5.5 Training Card States (User Portal)

| State | Visual | CTA |
|-------|--------|-----|
| Open (seats available) | Green seat count | "Enroll Now" (primary button) |
| Almost Full (<3 seats) | Orange "Almost Full!" badge | "Enroll Now" (urgent styling) |
| Full (waitlist possible) | Red "Full" badge | "Join Waitlist" (secondary button) |
| Closed | Gray overlay | "Registration Closed" (disabled) |
| Already Enrolled | Blue "Enrolled" badge | "View My Enrollment" (link) |

### 5.6 Enrollment Row States (Admin Table)

| Status | Badge Color | Available Actions |
|--------|-------------|-------------------|
| Pending | Yellow | Approve, Waitlist, Reject |
| Approved | Green | Revoke, Change Slot |
| Waitlisted | Orange | Approve (if slot opens), Reject |
| Waiting (next batch) | Blue | Approve (next batch), Remove |
| Opted Out | Gray | Remove |
| Rejected | Red | Re-enroll |

---

## 6. Responsive Design

### 6.1 Breakpoints & Layout Behavior

| Breakpoint | Width | Admin Layout | User Layout |
|-----------|-------|-------------|-------------|
| Mobile | <768px | Sidebar hidden, bottom nav (4 items + "More" drawer) | Bottom nav (4 items), single-column cards |
| Tablet | 768-1023px | Collapsed sidebar (icons only), expands on hover | Top nav, 2-column card grid |
| Desktop | 1024px+ | Full sidebar with labels, 12-column content grid | Top nav, 3-column card grid (max 1280px) |

### 6.2 Admin Table on Mobile

Tables convert to stacked card list on mobile:

```
+-------------------------------------+
| []  Priya S.              Pending   |
|     Phone: 98XXX123                 |
|     Pref 1: Morning | Pref 2: Eve  |
|     Enrolled: Feb 1                 |
|     [Approve] [Waitlist] [...]      |
+-------------------------------------+
```

### 6.3 User Training Cards

- Desktop: Horizontal card with icon, details, and CTA side-by-side
- Mobile: Vertical card, full-width, stacked content

---

## 7. Accessibility (WCAG 2.1 AA)

| Requirement | Implementation |
|-------------|---------------|
| Color contrast | All text/bg combinations meet 4.5:1 (normal) / 3:1 (large text). Status badges use color AND text/icon — never color alone |
| Keyboard navigation | Full tab order. Focus visible ring (2px, primary color). Skip-to-content link. Escape closes modals |
| Screen readers | ARIA labels on icon-only buttons. `role="alert"` on toasts. `aria-live="polite"` on dynamic content. Table headers use `scope` |
| Touch targets | Minimum 44x44px for all buttons, links, checkboxes. Radio buttons have full-row click area |
| Form accessibility | Labels via `for`/`id`. Errors linked via `aria-describedby`. Required fields marked with `*` and `aria-required` |
| Motion | Respect `prefers-reduced-motion`. Animations max 200ms. No auto-playing animations |
| Zoom | Content reflows at 200% zoom without horizontal scroll. No fixed-width containers |

---

## 8. Internationalization (i18n)

### 8.1 Supported Languages

```
locales/
├── en.json    (English - default, source of truth)
├── hi.json    (Hindi - हिन्दी)
├── te.json    (Telugu - తెలుగు)
├── kn.json    (Kannada - ಕನ್ನಡ)
├── ta.json    (Tamil - தமிழ்)
├── ml.json    (Malayalam - മലയാളം)
├── mr.json    (Marathi - मराठी)
└── bn.json    (Bengali - বাংলা)
```

### 8.2 Design Considerations

| Concern | Solution |
|---------|----------|
| Text expansion | 40% horizontal buffer in layouts. Hindi/Marathi text is 20-30% longer than English |
| Font loading | Noto Sans Latin (base) + user's script variant. Lazy-load others. `font-display: swap` |
| Number formatting | `Intl.NumberFormat` with locale. Dates via `Intl.DateTimeFormat`. Phone numbers in international format |
| Language picker | Native script first, English name in parentheses. Persisted in localStorage. Available before auth |
| Indic line height | `line-height: 1.6` for Indic scripts (vs 1.5 for Latin) due to taller ascenders/descenders |
| Pluralization | ICU message format. Hindi, Marathi, Bengali have different plural rules than English |
| RTL | Not needed — all 8 target languages are LTR |
| User content | Names, training names, descriptions stored in user's input language. Only UI chrome is translated |

### 8.3 Language Selector Component

Dropdown showing each language in its native script with English name in parentheses:
- English (selected indicator)
- हिन्दी (Hindi)
- తెలుగు (Telugu)
- ಕನ್ನಡ (Kannada)
- தமிழ் (Tamil)
- മലയാളം (Malayalam)
- मराठी (Marathi)
- বাংলা (Bengali)

---

## Appendix: Email Templates

### A.1 Enrollment Confirmation Email

Subject: "Your enrollment in [Training Name] is confirmed!"
Body: Training name, assigned slot with time, start date, what to bring, contact info, "View in Portal" button (deep link).

### A.2 Waitlist Notification Email

Subject: "Update on your [Training Name] enrollment"
Body: Training name, slot requested, current waitlist position, two CTA buttons:
- "Wait for Next Batch" (deep link to /respond/:token?action=wait)
- "Opt Out" (deep link to /respond/:token?action=optout)
Token validity: 7 days.

### A.3 Low Attendance Alert Email

Subject: "Attendance alert for [Training Name]"
Body: Current attendance %, minimum required (70%), classes attended vs total, encouragement message, "View Attendance" button (deep link to portal).

---

## Appendix: Screen Inventory

Complete list of all screens with their artifact files.

| # | Screen | Route | Screenshot | HTML Mockup |
|---|--------|-------|------------|-------------|
| 01 | Login | `/` | `screenshots/01-login.png` | `mockups/01-login.html` |
| 02 | OTP Verification | `/verify` | `screenshots/02-otp-verify.png` | `mockups/02-otp-verify.html` |
| 03 | Admin Dashboard | `/admin/dashboard` | `screenshots/03-admin-dashboard.png` | `mockups/03-admin-dashboard.html` |
| 04 | Training List | `/admin/trainings` | `screenshots/04-admin-trainings.png` | `mockups/04-admin-trainings.html` |
| 05 | Add Training Modal | `/admin/trainings` (modal) | `screenshots/05-admin-add-training.png` | `mockups/05-admin-add-training.html` |
| 06 | Batch Allocation | `/admin/batches/:id` | `screenshots/06-admin-batch-allocation.png` | `mockups/06-admin-batch-allocation.html` |
| 07 | Allocation Confirm | `/admin/batches/:id` (dialog) | `screenshots/07-admin-batch-confirm-dialog.png` | `mockups/07-admin-batch-confirm-dialog.html` |
| 08 | Attendance Mgmt | `/admin/trainings/:id/attendance` | `screenshots/08-admin-attendance.png` | `mockups/08-admin-attendance.html` |
| 09 | User Management | `/admin/users` | `screenshots/09-admin-users.png` | `mockups/09-admin-users.html` |
| 10 | Browse Trainings | `/user/trainings` | `screenshots/10-user-browse-trainings.png` | `mockups/10-user-browse-trainings.html` |
| 11 | Enrollment Form | `/user/trainings/:id` | `screenshots/11-user-enrollment.png` | `mockups/11-user-enrollment.html` |
| 12 | Enrollment Success | `/user/trainings/:id/success` | `screenshots/12-user-enrollment-success.png` | `mockups/12-user-enrollment-success.html` |
| 13 | User Dashboard | `/user/dashboard` | `screenshots/13-user-dashboard.png` | `mockups/13-user-dashboard.html` |
| 14 | Email Response | `/respond/:token` | `screenshots/14-email-response.png` | `mockups/14-email-response.html` |
| 15 | Response Confirm | `/respond/:token/confirm` | `screenshots/15-email-response-confirm.png` | `mockups/15-email-response-confirm.html` |
| 16 | Notification Log | `/admin/notifications` | `screenshots/16-admin-notifications.png` | `mockups/16-admin-notifications.html` |

All screenshot and mockup files are located under `ui_ux_design/` relative to the project root.
