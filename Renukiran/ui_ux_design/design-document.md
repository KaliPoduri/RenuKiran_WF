# RWF Learning Management System — UI/UX Design Document

**Version:** 3.0
**Date:** 2026-03-11
**Scope:** Phase 1 — Office-Driven Training Management
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
8. [Appendix: Screen Inventory](#appendix-screen-inventory)

---

## 1. Design Decisions Summary

| Decision | Choice |
|----------|--------|
| Scope | Phase 1: Office-Driven Training Management |
| Users | Staff only — Admin, Office Coordinator, Trainer |
| Candidate access | None — candidates do not use the system in Phase 1 |
| Authentication | Email + password, admin-created accounts only |
| Language | English only |
| Device priority | Desktop-first (office use), mobile-responsive |
| Application form | 5-step wizard based on Baseline Assessment Form |
| Assessments | Structured — MCQ + Practical + Case Study, weighted grading |
| Placement tracking | Active workflow with automated follow-up reminders at 1/3/6 months |
| Notifications | Internal only — in-app alerts for staff |
| Dashboard | Operational + impact metrics for donor/CSR reporting |

### What Changed from v2.0

The previous design (v2.0) was built around candidate self-enrollment with phone+OTP authentication. Phase 1 shifts to an office-driven model:

- **Removed:** Candidate portal, self-enrollment, phone+OTP auth, user dashboard, email response pages, waitlist flow, i18n (8 languages), language selector
- **Added:** Office Coordinator role, 5-step application wizard, assessment entry, placement tracking with follow-ups, role-based dashboards, structured grading
- **Reworked:** Login (email+password), admin dashboard (impact metrics), batch management, attendance, user management

---

## 2. Information Architecture & Navigation

### 2.1 Site Map

```
RWF Learning Management System
├── /login ............................ Email + Password Authentication
│
├── /admin ............................ Admin Portal
│   ├── /admin/dashboard .............. Overview, KPIs & Impact Metrics
│   ├── /admin/courses ................ Course Configuration
│   │   └── /admin/courses/new ........ Add/Edit Course (modal)
│   ├── /admin/batches ................ Batch Management
│   │   └── /admin/batches/:id ........ Batch Detail & Candidate List
│   ├── /admin/users .................. Staff Account Management
│   │   └── /admin/users/new .......... Add Staff Account (modal)
│   ├── /admin/reports ................ Impact & Operational Reports
│   └── /admin/notifications .......... In-App Alert Center
│
├── /oc ............................... Office Coordinator Portal
│   ├── /oc/dashboard ................. Operational Dashboard
│   ├── /oc/applications .............. Candidate Application List
│   │   ├── /oc/applications/new ...... Application Entry Wizard (5 steps)
│   │   └── /oc/applications/:id ...... Candidate Profile & History
│   ├── /oc/batches ................... Batch Assignment View
│   │   └── /oc/batches/:id .......... Batch Detail & Assign Candidates
│   ├── /oc/placements ................ Placement Tracking
│   │   └── /oc/placements/:id ....... Placement Detail & Follow-ups
│   └── /oc/notifications ............ In-App Alert Center
│
├── /trainer .......................... Trainer Portal
│   ├── /trainer/dashboard ............ My Batches Overview
│   ├── /trainer/batches/:id .......... Batch Detail
│   │   ├── Attendance tab ........... Mark Attendance
│   │   └── Assessments tab .......... Enter Assessment Results
│   └── /trainer/notifications ........ In-App Alert Center
```

### 2.2 Navigation Patterns

**Admin Layout:**
- Desktop: Persistent left sidebar (260px, collapsible) with icon + label navigation. Top bar with notification bell and profile avatar.
- Sidebar items: Dashboard, Courses, Batches, Users, Reports, Notifications
- Mobile: Hamburger menu opening a drawer overlay

**Office Coordinator Layout:**
- Desktop: Persistent left sidebar (260px) with fewer items than Admin.
- Sidebar items: Dashboard, Applications, Batches, Placements, Notifications
- Mobile: Hamburger menu

**Trainer Layout:**
- Desktop: Simplified left sidebar (260px).
- Sidebar items: Dashboard, My Batches, Notifications
- Mobile: Hamburger menu

**Key Principle:** Each role sees only the navigation items relevant to their responsibilities. Admin can access all screens. The sidebar highlights the active section with a left border accent.

### 2.3 Role-Based Access Matrix

| Screen | Admin | Office Coordinator | Trainer |
|--------|-------|-------------------|---------|
| Admin Dashboard | ✅ | — | — |
| OC Dashboard | ✅ | ✅ | — |
| Trainer Dashboard | ✅ | — | ✅ |
| Course Management | ✅ | View only | — |
| Batch Management | ✅ | Assign candidates | View assigned |
| User Management | ✅ | — | — |
| Application Entry | ✅ | ✅ | — |
| Candidate List | ✅ | ✅ | — |
| Candidate Profile | ✅ | ✅ | View assigned |
| Attendance Marking | ✅ | — | ✅ |
| Assessment Entry | ✅ | — | ✅ |
| Assessment Results | ✅ | ✅ | ✅ (own batches) |
| Placement Tracking | ✅ | ✅ | — |
| Reports | ✅ | — | — |
| Notifications | ✅ | ✅ | ✅ |

---

## 3. Design System & Visual Language

### 3.1 Color Palette

Semantic color system derived from the Renukiran Welfare Foundation logo (deep blue figures + bright green swoosh).

| Token | Role | Value | Usage |
|-------|------|-------|-------|
| `--brand-blue` | Brand primary | `#2B5EA7` | Primary buttons, active nav, key actions |
| `--brand-blue-dark` | Primary shade | `#1E4A8A` | Hover/pressed states, focus rings |
| `--brand-blue-light` | Primary tint | `#EBF0F9` | Selected rows, active backgrounds |
| `--brand-blue-50` | Lightest blue | `#F0F4FA` | Page backgrounds |
| `--brand-green` | Brand accent | `#7CB518` | Success accents, positive indicators |
| `--brand-green-dark` | Accent shade | `#5E8A12` | Pressed states for accent elements |
| `--brand-green-light` | Accent tint | `#F0F8E4` | Success backgrounds |
| `--neutral-50` | Lightest gray | `#FAFAFA` | Page backgrounds |
| `--neutral-100` | Light gray | `#F4F4F5` | Card backgrounds, table stripes |
| `--neutral-200` | Border gray | `#E4E4E7` | Borders, dividers |
| `--neutral-500` | Mid gray | `#71717A` | Placeholder text, disabled |
| `--neutral-600` | Body text | `#52525B` | Secondary text |
| `--neutral-900` | Headings | `#18181B` | Headings, primary text |
| `--success` | Positive | `#16A34A` | Placed, completed, good attendance |
| `--warning` | Caution | `#EAB308` | Under review, at-risk attendance |
| `--danger` | Error/alert | `#DC2626` | Not placed, low attendance, errors |
| `--info` | Informational | `#2563EB` | Links, info tooltips |

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

**Primary font: Inter** — excellent readability, tabular number support, variable font. No Indic script fonts needed in Phase 1 (English only).

### 3.3 Spacing & Grid

- Base unit: 4px
- Common spacings: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64px
- Grid: 12-column on desktop (max-width 1280px), 4-column on mobile
- Breakpoints: `sm: 640px`, `md: 768px`, `lg: 1024px`, `xl: 1280px`
- Touch targets: Minimum 44x44px for all interactive elements

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

### 3.6 Component Library

| Component | Variants | Notes |
|-----------|----------|-------|
| Button | Primary, Secondary, Outline, Ghost, Danger | Min height 44px, loading spinner state |
| Input | Text, Number, Phone, Email, Password, Textarea | Clear labels, inline validation, helper text |
| Select/Dropdown | Single, Multi | Search-enabled for long lists |
| Checkbox Group | Vertical list | For multi-select form fields (government schemes, motivations) |
| Radio Group | Vertical list, Card style | For single-select (housing type, education level) |
| Table | Sortable, filterable, paginated | Row click for detail view |
| Card | Stat card, Batch card, Candidate card | Consistent padding, elevation-1 |
| Badge | Status (New/Under Review/Selected/Assigned/Completed/Placed/Not Placed) | Color-coded per status |
| Dialog/Modal | Confirmation, Form, Alert | Backdrop, trap focus, escape to close |
| Toast/Notification | Success, Error, Warning, Info | Auto-dismiss 5s |
| Tabs | Horizontal | Used in batch detail (Attendance/Assessments) |
| Stepper | Horizontal 5-step | Application wizard progress indicator |
| Avatar | Initials fallback | 32/40/48px sizes |
| Empty State | Illustration + CTA | For tables/lists with no data |
| Skeleton Loader | Content placeholder | Pulse animation during load |
| Progress Bar | Linear | Batch capacity, attendance percentage |

---

## 4. Page-by-Page Screen Designs

---

### 4.1 Login (`/login`)

**Screenshot:** [`ui_ux_design/screenshots/01-login.png`](screenshots/01-login.png)
**HTML Mockup:** [`ui_ux_design/mockups/01-login.html`](mockups/01-login.html)

```
+---------------------------------------------------+
|                                                    |
|            +---------------+                       |
|            |  RWF Logo     |                       |
|            +---------------+                       |
|        Renukiran Learning Portal                   |
|                                                    |
|  +---------------------------------------------+  |
|  |  Email                                       |  |
|  |  +---------------------------------------+   |  |
|  |  |  admin@renukiran.org                  |   |  |
|  |  +---------------------------------------+   |  |
|  |                                              |  |
|  |  Password                                    |  |
|  |  +---------------------------------------+   |  |
|  |  |  ••••••••                    [eye]    |   |  |
|  |  +---------------------------------------+   |  |
|  |                                              |  |
|  |  [x] Remember me          Forgot password?   |  |
|  |                                              |  |
|  |  +---------------------------------------+   |  |
|  |  |           Sign In                     |   |  |
|  |  +---------------------------------------+   |  |
|  |                                              |  |
|  |  Staff accounts are created by the admin.    |  |
|  |  Contact your administrator for access.      |  |
|  +---------------------------------------------+  |
|                                                    |
|  ----------- Powered by RWF -----------            |
+---------------------------------------------------+
```

**Design Rationale:**
- Single-purpose screen. Email + password only. No registration link — staff accounts are admin-created.
- RWF logo with brand identity at top. Centered card with shadow for clear focal point.
- Password field has show/hide toggle (eye icon).
- "Remember me" checkbox for convenience on shared office computers.
- "Forgot password?" triggers admin-controlled password reset flow.
- Helper text at bottom clarifies: no self-signup, contact admin.
- On successful login, system checks user role and auto-routes to the appropriate dashboard (`/admin/dashboard`, `/oc/dashboard`, or `/trainer/dashboard`).

**Interaction Details:**
- Email validation on blur (format check)
- Password field: minimum 8 characters, show/hide toggle
- Sign In button disabled until both fields have input
- On submit: loading spinner on button
- Error states: "Invalid email or password" (generic, no hints about which field is wrong for security)
- After 5 failed attempts: "Too many attempts. Try again in 15 minutes."

---

### 4.2 Admin Dashboard (`/admin/dashboard`)

**Screenshot:** [`ui_ux_design/screenshots/02-admin-dashboard.png`](screenshots/02-admin-dashboard.png)
**HTML Mockup:** [`ui_ux_design/mockups/02-admin-dashboard.html`](mockups/02-admin-dashboard.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Dashboard                           [bell] [avatar] | |
| |        |  +------------------------------------------------------+ |
| | Dash   |  |                                                      | |
| | Courses|  |  +----------+ +----------+ +----------+ +----------+ | |
| | Batches|  |  |Total     | |Active    | |Candidates| |Pending   | | |
| | Users  |  |  |Courses   | |Batches   | |Enrolled  | |Assign.   | | |
| | Reports|  |  |    8     | |    5     | |   142    | |    23    | | |
| | Notif  |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |                                                      | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |  |Assessment| |Placement | |Job       | |Avg       | | |
| |        |  |  |Pass Rate | |Rate      | |Retention | |Attendance| | |
| |        |  |  |   78%    | |   62%    | |   85%    | |   88%    | | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |                                                      | |
| |        |  |  Recent Applications         [View All ->]           | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |  | Name      | Course     | Status    | Date      |  | |
| |        |  |  +-----------+------------+-----------+-----------+  | |
| |        |  |  | Priya S.  | Stitching  | New       | Mar 10    |  | |
| |        |  |  | Rahul K.  | Computers  | Assigned  | Mar 9     |  | |
| |        |  |  | Meena D.  | Beauty     | Selected  | Mar 8     |  | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |                                                      | |
| |        |  |  Batch Capacity               Attendance Alerts      | |
| |        |  |  +---------------------+  +---------------------+   | |
| |        |  |  | Stitching B1: 18/20 |  | Kavita R: 68% ⚠    |   | |
| |        |  |  | Computers B2: 19/20 |  | Anita K:  72% ⚠    |   | |
| |        |  |  | Beauty B1:    12/20 |  |                     |   | |
| |        |  |  +---------------------+  +---------------------+   | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Two rows of stat cards** — top row shows operational metrics (counts), bottom row shows impact metrics (percentages). All cards are clickable, navigating to the relevant list or report.
- **Impact metrics row** (pass rate, placement rate, retention, attendance) supports donor/CSR reporting needs. Color-coded: green >75%, yellow 50-75%, red <50%.
- **Recent Applications table** shows the 5 latest with status badges. "View All" navigates to full candidate list.
- **Batch Capacity bars** — green/yellow/red by fill level. Flags batches nearing capacity.
- **Attendance Alerts** — users below 70% threshold with warning icons. Quick visibility for admin.
- Sidebar: 260px persistent, RWF branding at top, active item has blue left border.
- Top bar: notification bell (red badge count), avatar with initials dropdown (Profile, Logout).

---

### 4.3 Course Management (`/admin/courses`)

**Screenshot:** [`ui_ux_design/screenshots/03-course-management.png`](screenshots/03-course-management.png)
**HTML Mockup:** [`ui_ux_design/mockups/03-course-management.html`](mockups/03-course-management.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Courses                    [+ Add Course]           | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  [Search...          ] [Status: All v] [Sort: Name]  | |
| |        |  |                                                      | |
| |        |  |  +---------------------------------------------------+|
| |        |  |  | 🧵 Stitching - Basic              Active         ||
| |        |  |  |    Duration: 3 months | Max: 20/batch             ||
| |        |  |  |    Assessments: MCQ(30%) + Practical(50%) +       ||
| |        |  |  |                 Case Study(20%)                   ||
| |        |  |  |    [Edit] [View Batches] [...]                    ||
| |        |  |  +---------------------------------------------------+|
| |        |  |                                                      | |
| |        |  |  +---------------------------------------------------+|
| |        |  |  | 💻 Computer Fundamentals           Active         ||
| |        |  |  |    Duration: 2 months | Max: 20/batch             ||
| |        |  |  |    Assessments: MCQ(40%) + Practical(40%) +       ||
| |        |  |  |                 Case Study(20%)                   ||
| |        |  |  |    [Edit] [View Batches] [...]                    ||
| |        |  |  +---------------------------------------------------+|
| |        |  |                                                      | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- Card-based layout with category icons and color accents per course type.
- Status badges: Active (green), Upcoming (blue), Closed (gray).
- Each card shows assessment weight breakdown — critical info for admin course configuration.
- Search + filter + sort toolbar for quick access.
- "+" button opens the Add Course modal.

---

### 4.4 Add/Edit Course Modal (`/admin/courses` — modal)

**Screenshot:** [`ui_ux_design/screenshots/04-add-course-modal.png`](screenshots/04-add-course-modal.png)
**HTML Mockup:** [`ui_ux_design/mockups/04-add-course-modal.html`](mockups/04-add-course-modal.html)

```
+---------------------------------------------------+
|  Add New Course                              [X]   |
|---------------------------------------------------|
|                                                    |
|  Course Name *                                     |
|  +---------------------------------------------+  |
|  |  e.g., Stitching - Basic                    |  |
|  +---------------------------------------------+  |
|                                                    |
|  Category *                                        |
|  +---------------------------------------------+  |
|  |  Select category...                      v  |  |
|  +---------------------------------------------+  |
|  (Stitching, Computers, Beauty, Bag Making,        |
|   Food Enterprise, Handicraft, Other)              |
|                                                    |
|  Duration (months) *        Max per Batch *        |
|  +------------------+      +------------------+   |
|  |  3               |      |  20              |   |
|  +------------------+      +------------------+   |
|                                                    |
|  Description                                       |
|  +---------------------------------------------+  |
|  |                                             |  |
|  +---------------------------------------------+  |
|                                                    |
|  Assessment Weights                                |
|  +---------------------------------------------+  |
|  |  MCQ:        [  30  ] %                     |  |
|  |  Practical:  [  50  ] %                     |  |
|  |  Case Study: [  20  ] %                     |  |
|  |              Total: 100% ✓                  |  |
|  +---------------------------------------------+  |
|                                                    |
|  Status:  (•) Active  ( ) Upcoming  ( ) Closed     |
|                                                    |
|  +---------------------+  +--------------------+  |
|  |      Cancel         |  |    Save Course     |  |
|  +---------------------+  +--------------------+  |
+---------------------------------------------------+
```

**Design Rationale:**
- Modal (560px max-width) keeps context visible behind it.
- Category dropdown matches the enterprise tracks from the baseline form (Stitching, Computers, Beauty, Bag Making, Food Enterprise, Handicraft).
- Assessment weights section validates that total = 100%. Shows green checkmark when valid, red warning when not.
- "Save Course" disabled until all required fields filled and weights sum to 100%.

---

### 4.5 Batch Management (`/admin/batches`)

**Screenshot:** [`ui_ux_design/screenshots/05-batch-management.png`](screenshots/05-batch-management.png)
**HTML Mockup:** [`ui_ux_design/mockups/05-batch-management.html`](mockups/05-batch-management.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Batches                    [+ Create Batch]         | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  [Search...    ] [Course: All v] [Status: All v]     | |
| |        |  |                                                      | |
| |        |  |  +----+------------+----------+--------+--------+---+| |
| |        |  |  | ID | Course     | Trainer  | Dates  |Capacity|Sts|| |
| |        |  |  +----+------------+----------+--------+--------+---+| |
| |        |  |  | B1 | Stitching  | Suman K. | Mar-Jun| 18/20  |Act|| |
| |        |  |  | B2 | Computers  | Raj P.   | Mar-May| 19/20  |Act|| |
| |        |  |  | B3 | Beauty     | Asha M.  | Apr-Jul| 0/20   |Upc|| |
| |        |  |  | B4 | Stitching  | Suman K. | Jan-Mar| 20/20  |Cmp|| |
| |        |  |  +----+------------+----------+--------+--------+---+| |
| |        |  |                                                      | |
| |        |  |  <- 1 2 3 ->              Showing 1-10 of 12        | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- Table view (not cards) — batches are operational data best consumed as rows. Sortable columns.
- Capacity column shows progress bar with color coding (green <80%, yellow 80-95%, red >95%).
- Status badges: Upcoming (blue), Ongoing (green), Completed (gray).
- Row click opens Batch Detail.
- "Create Batch" button opens a modal with fields: Course (dropdown), Batch ID (auto-generated), Trainer (dropdown from staff), Location, Start Date, End Date, Max Capacity, Status.

---

### 4.6 Batch Detail (`/admin/batches/:id` or `/oc/batches/:id`)

**Screenshot:** [`ui_ux_design/screenshots/06-batch-detail.png`](screenshots/06-batch-detail.png)
**HTML Mockup:** [`ui_ux_design/mockups/06-batch-detail.html`](mockups/06-batch-detail.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  <- Back    Stitching Basic - Batch 1                | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  Trainer: Suman K. | Mar 1 - Jun 30, 2026           | |
| |        |  |  Capacity: 18/20 filled  [=============--]           | |
| |        |  |  Status: Ongoing                                     | |
| |        |  |                                                      | |
| |        |  |  [Candidates]  [Attendance]  [Assessments]           | |
| |        |  |  ─────────────                                       | |
| |        |  |                                                      | |
| |        |  |  [+ Assign Candidates]      [Search...      ]       | |
| |        |  |                                                      | |
| |        |  |  +----+----------+----------+-----------+--------+  | |
| |        |  |  | #  | Name     | Phone    | Status    | Attend%|  | |
| |        |  |  +----+----------+----------+-----------+--------+  | |
| |        |  |  | 1  | Priya S. | 98XX123  | Training  | 93%    |  | |
| |        |  |  | 2  | Kavita R.| 98XX456  | Training  | 68% ⚠ |  | |
| |        |  |  | 3  | Sunita M.| 98XX789  | Completed | 85%    |  | |
| |        |  |  +----+----------+----------+-----------+--------+  | |
| |        |  |                                                      | |
| |        |  |  [Remove Selected]                                   | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Header section** shows batch metadata at a glance: trainer, dates, capacity bar, status.
- **Three tabs:** Candidates (default), Attendance, Assessments — keeps all batch operations in one view.
- **Candidates tab** lists all assigned candidates with attendance percentage. Low attendance (<70%) flagged with warning icon and red text.
- **"+ Assign Candidates" button** opens a picker showing unassigned candidates filtered by the course's matching enterprise track.
- Attendance tab shows the attendance marking interface (see Screen 4.12).
- Assessments tab shows assessment entry/results (see Screen 4.13).

---

### 4.7 User Management (`/admin/users`)

**Screenshot:** [`ui_ux_design/screenshots/07-user-management.png`](screenshots/07-user-management.png)
**HTML Mockup:** [`ui_ux_design/mockups/07-user-management.html`](mockups/07-user-management.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Staff Accounts    [+ Add Office Coord.] [+ Add Trainer]|
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  [Search...        ] [Role: All v] [Status: All v]   | |
| |        |  |                                                      | |
| |        |  |  +---+----------+------------------+-------+------+  | |
| |        |  |  |   | Name     | Email            | Role  |Status|  | |
| |        |  |  +---+----------+------------------+-------+------+  | |
| |        |  |  | V | Vinay A. | vinay@rwf.org    | Admin | Act  |  | |
| |        |  |  | S | Suman K. | suman@rwf.org    | Train | Act  |  | |
| |        |  |  | R | Rekha P. | rekha@rwf.org    | OC    | Act  |  | |
| |        |  |  | A | Asha M.  | asha@rwf.org     | Train | Act  |  | |
| |        |  |  +---+----------+------------------+-------+------+  | |
| |        |  |                                                      | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- Staff-only user management. No candidate accounts.
- **Two separate "Add" buttons** — purple "+ Add Office Coordinator" and teal "+ Add Trainer" — each opens a role-specific modal with tailored fields and permissions display. No generic role picker.
- Avatar initials, role badges (Admin=blue, OC=purple, Trainer=teal), status (Active/Inactive).
- Admin role creation restricted — only existing admins can create new admin accounts. Admin accounts are not creatable from the UI (seeded at setup).
- Edit/Deactivate actions per row. Deactivate instead of delete to preserve audit trail.

---

### 4.7b Add Office Coordinator (`/admin/users` — modal)

**Screenshot:** [`ui_ux_design/screenshots/07b-add-office-coordinator.png`](screenshots/07b-add-office-coordinator.png)
**HTML Mockup:** [`ui_ux_design/mockups/07b-add-office-coordinator.html`](mockups/07b-add-office-coordinator.html)

```
+---------------------------------------------------+
|  [OC icon]  Add Office Coordinator           [X]  |
|  Data entry, batch assignment & placement tracking |
|---------------------------------------------------|
|                                                    |
|  ACCOUNT DETAILS                                   |
|                                                    |
|  Full Name *                                       |
|  +---------------------------------------------+  |
|  |  e.g., Rekha Patel                          |  |
|  +---------------------------------------------+  |
|                                                    |
|  Email *                     Phone Number          |
|  +------------------+       +------------------+   |
|  | rekha@rwf.org    |       | 9876543210       |   |
|  +------------------+       +------------------+   |
|  Used for login                                    |
|                                                    |
|  Password *                                        |
|  +---------------------------------------------+  |
|  |  rP7$kLm3Xq                         [eye]  |  |
|  +---------------------------------------------+  |
|  Auto-generated  [Regenerate]                      |
|  Share securely. User must change on first login.  |
|                                                    |
|  ─────────────────────────────────────────         |
|  PERMISSIONS FOR THIS ROLE                         |
|                                                    |
|  +---------------------------------------------+  |
|  | Office Coordinator can:                      |  |
|  | ✓ Enter candidate applications               |  |
|  | ✓ Assign candidates to batches               |  |
|  | ✓ Update candidate status                    |  |
|  | ✓ Manage training progress                   |  |
|  | ✓ Track placements & follow-ups              |  |
|  | ✓ View operational dashboard                 |  |
|  | ✗ Configure courses                          |  |
|  | ✗ Create batches                             |  |
|  | ✗ Manage staff accounts                      |  |
|  | ✗ Access full reports                        |  |
|  +---------------------------------------------+  |
|                                                    |
|  +---------------------+  +--------------------+  |
|  |      Cancel         |  | Create Office Coord|  |
|  +---------------------+  +--------------------+  |
+---------------------------------------------------+
```

**Design Rationale:**
- **Role-specific modal** — header shows OC icon (purple), role title, and one-line description of responsibilities. No ambiguity about what role is being created.
- **Account details section** — Full Name, Email (login credential), Phone, auto-generated Password with show/hide and regenerate.
- **Permissions display** — two-column grid showing what OC **can** (green checkmarks) and **cannot** (red X, grayed text) do. Makes the role boundary crystal clear to the admin creating the account.
- **Purple accent** throughout (icon background, permissions border) to visually associate with OC role badge color.

---

### 4.7c Add Trainer (`/admin/users` — modal)

**Screenshot:** [`ui_ux_design/screenshots/07c-add-trainer.png`](screenshots/07c-add-trainer.png)
**HTML Mockup:** [`ui_ux_design/mockups/07c-add-trainer.html`](mockups/07c-add-trainer.html)

```
+---------------------------------------------------+
|  [Trainer icon]  Add Trainer                 [X]  |
|  Attendance recording & assessment entry           |
|---------------------------------------------------|
|                                                    |
|  ACCOUNT DETAILS                                   |
|                                                    |
|  Full Name *                                       |
|  +---------------------------------------------+  |
|  |  e.g., Suman Kumar                          |  |
|  +---------------------------------------------+  |
|                                                    |
|  Email *                     Phone Number          |
|  +------------------+       +------------------+   |
|  | suman@rwf.org    |       | 9876543210       |   |
|  +------------------+       +------------------+   |
|                                                    |
|  Password *                                        |
|  +---------------------------------------------+  |
|  |  tR4@nWx8Kp                         [eye]  |  |
|  +---------------------------------------------+  |
|  Auto-generated  [Regenerate]                      |
|                                                    |
|  ─────────────────────────────────────────         |
|  COURSE ASSIGNMENT                                 |
|                                                    |
|  Courses this trainer can teach *                  |
|  Select one or more. Trainer only sees these.      |
|                                                    |
|  [✓ Stitching] [ Computers] [✓ Beauty]            |
|  [ Bag Making] [ Food Enterprise] [ Handicraft]   |
|                                                    |
|  ─────────────────────────────────────────         |
|  PERMISSIONS FOR THIS ROLE                         |
|                                                    |
|  +---------------------------------------------+  |
|  | Trainer can:                                 |  |
|  | ✓ Record attendance                          |  |
|  | ✓ Enter assessment results                   |  |
|  | ✓ View assigned batch candidates             |  |
|  | ✓ View in-app notifications                  |  |
|  | ✗ Enter applications                         |  |
|  | ✗ Assign batches                             |  |
|  | ✗ Track placements                           |  |
|  | ✗ Configure courses                          |  |
|  | ✗ Manage users                               |  |
|  | ✗ Access reports                             |  |
|  +---------------------------------------------+  |
|                                                    |
|  +---------------------+  +--------------------+  |
|  |      Cancel         |  |   Create Trainer   |  |
|  +---------------------+  +--------------------+  |
+---------------------------------------------------+
```

**Design Rationale:**
- **Role-specific modal** — header shows Trainer icon (teal), role title, and responsibilities.
- **Course Assignment section** — unique to the Trainer modal. Selectable chip-style checkboxes for each course. The trainer will only see batches for their assigned courses — this is explained in the hint text.
- **Permissions display** — same pattern as OC modal but with Trainer-specific permissions. Fewer "can do" items since Trainer has the most restricted role.
- **Teal accent** throughout to match the Trainer role badge color.
- **Key difference from OC modal:** Trainer gets the "Course Assignment" section; OC does not need it since OC works across all courses.

---

### 4.8 OC Dashboard (`/oc/dashboard`)

**Screenshot:** [`ui_ux_design/screenshots/08-oc-dashboard.png`](screenshots/08-oc-dashboard.png)
**HTML Mockup:** [`ui_ux_design/mockups/08-oc-dashboard.html`](mockups/08-oc-dashboard.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Dashboard                           [bell] [avatar] | |
| | Dash   |  +------------------------------------------------------+ |
| | Apps   |  |                                                      | |
| | Batches|  |  +----------+ +----------+ +----------+ +----------+ | |
| | Place  |  |  |New       | |Under     | |Assigned  | |Pending   | | |
| | Notif  |  |  |Applicatns| |Review    | |to Batch  | |Placement | | |
| |        |  |  |    12    | |     8    | |    98    | |    15    | | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |                                                      | |
| |        |  |  Quick Actions                                       | |
| |        |  |  +-------------------+  +---------------------+     | |
| |        |  |  | + New Application |  | Assign to Batches   |     | |
| |        |  |  +-------------------+  +---------------------+     | |
| |        |  |                                                      | |
| |        |  |  Recent Applications          [View All ->]          | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |  | Name      | Course     | Status    | Date      |  | |
| |        |  |  +-----------+------------+-----------+-----------+  | |
| |        |  |  | Priya S.  | Stitching  | New       | Mar 10    |  | |
| |        |  |  | Rahul K.  | Computers  | Selected  | Mar 9     |  | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |                                                      | |
| |        |  |  Upcoming Follow-ups                                 | |
| |        |  |  +-----------------------------------------------+  | |
| |        |  |  | Kavita R. | 1-month follow-up due | Mar 15    |  | |
| |        |  |  | Sunita M. | 3-month follow-up due | Mar 18    |  | |
| |        |  |  +-----------------------------------------------+  | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- Focused on OC's daily work: application status counts, quick action buttons, recent apps, and upcoming placement follow-ups.
- **Quick Actions** — large buttons for the two most common tasks (enter new application, assign to batches). Reduces navigation steps.
- **Upcoming Follow-ups** — surfaces placement follow-up reminders so OC doesn't miss them. Sorted by due date.
- Simpler sidebar than Admin — only 5 items.

---

### 4.9 Application Entry Wizard (`/oc/applications/new`) — 5 Steps

**Screenshot (Step 1):** [`ui_ux_design/screenshots/09-app-wizard-step1.png`](screenshots/09-app-wizard-step1.png)
**Screenshot (Step 2):** [`ui_ux_design/screenshots/10-app-wizard-step2.png`](screenshots/10-app-wizard-step2.png)
**Screenshot (Step 3):** [`ui_ux_design/screenshots/11-app-wizard-step3.png`](screenshots/11-app-wizard-step3.png)
**Screenshot (Step 4):** [`ui_ux_design/screenshots/12-app-wizard-step4.png`](screenshots/12-app-wizard-step4.png)
**Screenshot (Step 5):** [`ui_ux_design/screenshots/13-app-wizard-step5.png`](screenshots/13-app-wizard-step5.png)
**HTML Mockup:** [`ui_ux_design/mockups/09-application-wizard.html`](mockups/09-application-wizard.html)

#### Step 1 — Personal Information

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  New Application                                     | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  Step 1    Step 2    Step 3    Step 4    Step 5      | |
| |        |  |  [●]-------[ ]-------[ ]-------[ ]-------[ ]        | |
| |        |  |  Personal  Household Education Training  Needs       | |
| |        |  |                                                      | |
| |        |  |  SECTION 1 — Personal Information                    | |
| |        |  |                                                      | |
| |        |  |  Batch *                                             | |
| |        |  |  ( ) Batch 1   ( ) Batch 2                           | |
| |        |  |                                                      | |
| |        |  |  Full Name *              Age *                      | |
| |        |  |  +---------------------+  +--------+                | |
| |        |  |  |                     |  |        |                | |
| |        |  |  +---------------------+  +--------+                | |
| |        |  |                                                      | |
| |        |  |  Father/Husband Name *                               | |
| |        |  |  +------------------------------------------+       | |
| |        |  |  |                                          |       | |
| |        |  |  +------------------------------------------+       | |
| |        |  |                                                      | |
| |        |  |  Mobile Number *           Alternate Number          | |
| |        |  |  +---------------------+  +---------------------+   | |
| |        |  |  |                     |  |                     |   | |
| |        |  |  +---------------------+  +---------------------+   | |
| |        |  |                                                      | |
| |        |  |  Full Address *                                      | |
| |        |  |  +------------------------------------------+       | |
| |        |  |  |                                          |       | |
| |        |  |  +------------------------------------------+       | |
| |        |  |                                                      | |
| |        |  |  Local Resident of Garhi? *                          | |
| |        |  |  ( ) Yes   ( ) No                                    | |
| |        |  |                                                      | |
| |        |  |  If Migrant — Year of Arrival in Delhi               | |
| |        |  |  +---------------------+                             | |
| |        |  |  |                     |                             | |
| |        |  |  +---------------------+                             | |
| |        |  |                                                      | |
| |        |  |  Aadhaar Number            Bank Account Available *  | |
| |        |  |  +---------------------+   ( ) Yes   ( ) No         | |
| |        |  |  |                     |                             | |
| |        |  |  +---------------------+                             | |
| |        |  |                                                      | |
| |        |  |  Caste Category *                                    | |
| |        |  |  ( ) General  ( ) OBC  ( ) SC  ( ) ST               | |
| |        |  |                                                      | |
| |        |  |                          [Save Draft]  [Next ->]     | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

#### Step 2 — Household & Socio-Economic Details

```
+----------------------------------------------------------------------+
|  Step 1    Step 2    Step 3    Step 4    Step 5                       |
|  [✓]-------[●]-------[ ]-------[ ]-------[ ]                        |
|                                                                       |
|  SECTION 2 — Household & Socio-Economic Details                      |
|                                                                       |
|  Total Family Members *       Working Members in Family *            |
|  +------------------+        +------------------+                    |
|  |                  |        |                  |                    |
|  +------------------+        +------------------+                    |
|                                                                       |
|  Monthly Household Income (₹) *                                      |
|  +------------------------------------------+                        |
|  |                                          |                        |
|  +------------------------------------------+                        |
|                                                                       |
|  Primary Source of Income *                                           |
|  +------------------------------------------+                        |
|  |                                          |                        |
|  +------------------------------------------+                        |
|                                                                       |
|  Housing Type *                                                       |
|  ( ) Rented   ( ) Own (Pucca)   ( ) Own (Kutcha)                    |
|                                                                       |
|  Government Schemes Availed                                           |
|  [x] Ration Card    [ ] Widow Pension    [ ] Old Age Pension         |
|  [ ] Jan Dhan Account   [ ] Ujjwala      [ ] Other: [______]        |
|                                                                       |
|  Migration Risk (Next 6 Months) *                                     |
|  ( ) Will stay long-term    ( ) Maybe will move                      |
|  ( ) Likely to move         ( ) Does not know                        |
|                                                                       |
|                      [<- Back]  [Save Draft]  [Next ->]              |
+----------------------------------------------------------------------+
```

#### Step 3 — Education & Work Background

```
+----------------------------------------------------------------------+
|  Step 1    Step 2    Step 3    Step 4    Step 5                       |
|  [✓]-------[✓]-------[●]-------[ ]-------[ ]                        |
|                                                                       |
|  SECTION 3 — Education & Work Background                             |
|                                                                       |
|  Education Level *                                                    |
|  ( ) No formal education  ( ) Primary  ( ) Secondary                 |
|  ( ) Higher Secondary     ( ) Graduate ( ) Other: [______]           |
|                                                                       |
|  Existing Skills / Experience                                         |
|                                                                       |
|  Stitching Experience *                                               |
|  ( ) None  ( ) Basic  ( ) Good  ( ) Advanced                        |
|                                                                       |
|  Sewing Machine at Home *    ( ) Yes   ( ) No                        |
|  Beauty/Parlour Experience * ( ) Yes   ( ) No                        |
|  Food Business Experience *  ( ) Yes   ( ) No                        |
|  Handicraft Experience *     ( ) Yes   ( ) No                        |
|                                                                       |
|  Previous Skill Training                                              |
|  +------------------------------------------+                        |
|  |  e.g., Government scheme, NGO program    |                        |
|  +------------------------------------------+                        |
|                                                                       |
|                      [<- Back]  [Save Draft]  [Next ->]              |
+----------------------------------------------------------------------+
```

#### Step 4 — Training Interest & Availability

```
+----------------------------------------------------------------------+
|  Step 1    Step 2    Step 3    Step 4    Step 5                       |
|  [✓]-------[✓]-------[✓]-------[●]-------[ ]                        |
|                                                                       |
|  SECTION 4 — Training Interest & Availability                        |
|                                                                       |
|  Preferred Enterprise Track *                                         |
|  [ ] Stitching / Garment Production                                  |
|  [ ] Bag Making                                                       |
|  [ ] Beauty / Parlour Services                                       |
|  [ ] Home-based Food Enterprise                                      |
|  [ ] Handicraft & Decoration Work                                    |
|  [ ] Other: [______]                                                 |
|                                                                       |
|  Distance to Training Centre *                                        |
|  ( ) <1 km   ( ) 1-2 km   ( ) >2 km                                 |
|                                                                       |
|  Motivation for Joining Training *                                    |
|  [ ] Want income immediately                                         |
|  [ ] Want to support family                                          |
|  [ ] Want to learn market-demand skills                              |
|  [ ] Want home-based work                                            |
|  [ ] Want to start micro-enterprise                                  |
|  [ ] Want to join SHG after program                                  |
|  [ ] Other: [______]                                                 |
|                                                                       |
|                      [<- Back]  [Save Draft]  [Next ->]              |
+----------------------------------------------------------------------+
```

#### Step 5 — Need-Based Assessment

```
+----------------------------------------------------------------------+
|  Step 1    Step 2    Step 3    Step 4    Step 5                       |
|  [✓]-------[✓]-------[✓]-------[✓]-------[●]                        |
|                                                                       |
|  SECTION 5 — Need-Based Assessment                                   |
|                                                                       |
|  Economic Situation *                                                 |
|  [ ] Extremely low income     [ ] Single mother / widow             |
|  [ ] No stable income         [ ] High financial stress             |
|  [ ] Family dependent on her  [ ] Other: [______]                   |
|                                                                       |
|  Learning & Skill Needs *                                             |
|  [ ] Foundation-level training   [ ] Machine support needed          |
|  [ ] Confidence building         [ ] Speed improvement              |
|  [ ] Finishing / Quality control [ ] Business basics                |
|  [ ] Other: [______]                                                 |
|                                                                       |
|  Enterprise Aspirations *                                             |
|  [ ] Start home kitchen        [ ] Work in mobile parlour           |
|  [ ] Join garment job-work     [ ] Start boutique/home stitching    |
|  [ ] Join handicraft work      [ ] Start micro-enterprise           |
|  [ ] Join SHG after 6 months   [ ] Other: [______]                  |
|                                                                       |
|  Willing to Participate in Production *                               |
|  ( ) Yes   ( ) No   ( ) Maybe                                       |
|                                                                       |
|                      [<- Back]  [Save Draft]  [Submit Application]   |
+----------------------------------------------------------------------+
```

**Design Rationale for the Wizard:**
- **Horizontal stepper** at top shows progress (completed steps get a checkmark, current step is filled, future steps are empty). This provides constant orientation.
- **"Save Draft" on every step** — OC may not have all information at once (e.g., candidate comes back with documents later). Draft applications appear in the candidate list with "Draft" status.
- **Back/Next navigation** — no skipping steps. Back preserves entered data. Next validates current step.
- **Checkbox groups** for multi-select fields (government schemes, motivations, economic situation, aspirations) match the paper form layout.
- **Radio groups** for single-select fields (caste category, housing type, education level) with card-style layout for larger touch targets.
- **Conditional fields** — "Year of Arrival in Delhi" only shows when "Local Resident?" is "No".
- **Final step** has "Submit Application" (green) instead of "Next". Shows confirmation dialog: "Submit application for [Name]? Status will be set to New Application."
- All fields from the Baseline Assessment Form are captured across the 5 steps with no omissions.

---

### 4.10 Candidate List (`/oc/applications`)

**Screenshot:** [`ui_ux_design/screenshots/14-candidate-list.png`](screenshots/14-candidate-list.png)
**HTML Mockup:** [`ui_ux_design/mockups/14-candidate-list.html`](mockups/14-candidate-list.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Candidates              [+ New Application]         | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  [Search...    ] [Status: All v] [Course: All v]     | |
| |        |  |                                                      | |
| |        |  |  +---+----------+----------+------------+--------+  | |
| |        |  |  |   | Name     | Phone    | Course     | Status |  | |
| |        |  |  +---+----------+----------+------------+--------+  | |
| |        |  |  | P | Priya S. | 98XX123  | Stitching  | New    |  | |
| |        |  |  | K | Kavita R.| 98XX456  | Computers  | Review |  | |
| |        |  |  | S | Sunita M.| 98XX789  | Beauty     | Assign |  | |
| |        |  |  | A | Anita K. | 98XX012  | Stitching  | Train  |  | |
| |        |  |  | R | Radha P. | 98XX345  | Handicraft | Placed |  | |
| |        |  |  | M | Meena D. | 98XX678  | Computers  | Draft  |  | |
| |        |  |  +---+----------+----------+------------+--------+  | |
| |        |  |                                                      | |
| |        |  |  <- 1 2 3 ... ->          Showing 1-20 of 142       | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- Searchable, filterable table of all candidates across all statuses.
- Avatar initials + name for visual anchoring.
- Status badges color-coded: Draft (gray), New (blue), Under Review (yellow), Selected (cyan), Assigned to Batch (green), Training (teal), Completed (green-dark), Placed (green), Not Placed (red).
- Row click opens Candidate Profile.
- "New Application" button starts the wizard.
- Pagination for large lists.

---

### 4.11 Candidate Profile (`/oc/applications/:id`)

**Screenshot:** [`ui_ux_design/screenshots/15-candidate-profile.png`](screenshots/15-candidate-profile.png)
**HTML Mockup:** [`ui_ux_design/mockups/15-candidate-profile.html`](mockups/15-candidate-profile.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  <- Back    Priya Sharma              [Edit] [...]   | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | Status: Assigned to Batch                        || |
| |        |  |  | New -> Under Review -> Selected -> Assigned      || |
| |        |  |  |  ✓       ✓              ✓           ●           || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  [Personal] [Household] [Education] [Training] [Need]| |
| |        |  |  ─────────                                           | |
| |        |  |                                                      | |
| |        |  |  Full Name: Priya Sharma                             | |
| |        |  |  Age: 24                                             | |
| |        |  |  Father/Husband: Ram Sharma                          | |
| |        |  |  Mobile: 9876543210                                  | |
| |        |  |  Address: Tilpat, Faridabad, HR-121003               | |
| |        |  |  Local Resident: Yes                                 | |
| |        |  |  Aadhaar: XXXX XXXX 1234                             | |
| |        |  |  Bank Account: Yes                                   | |
| |        |  |  Caste Category: OBC                                 | |
| |        |  |                                                      | |
| |        |  |  Batch Assignment                                    | |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | Batch: Stitching B1 | Trainer: Suman K.         || |
| |        |  |  | Period: Mar 1 - Jun 30 | Attendance: 93%        || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  Status History                                      | |
| |        |  |  Mar 10 - New Application (by Rekha P.)              | |
| |        |  |  Mar 11 - Under Review (by Vinay A.)                 | |
| |        |  |  Mar 12 - Selected (by Vinay A.)                     | |
| |        |  |  Mar 13 - Assigned to Batch B1 (by Rekha P.)         | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Status progress bar** at top shows the candidate's journey through all states. Completed states have checkmarks, current state is highlighted.
- **5 tabs** mirror the wizard sections — allows viewing all captured data organized the same way it was entered.
- **Batch Assignment card** (if assigned) shows batch details with attendance percentage.
- **Status History** — full audit trail of who changed the status and when. Important for accountability.
- **Edit button** reopens the wizard pre-filled with existing data.
- **Overflow menu (...)** — options: Change Status, Remove from Batch, Print Application, Delete (admin only).
- **Aadhaar masked** for security — only last 4 digits shown, full number accessible via "Show" link.

---

### 4.12 Attendance Marking (`/trainer/batches/:id` — Attendance Tab)

**Screenshot:** [`ui_ux_design/screenshots/16-attendance.png`](screenshots/16-attendance.png)
**HTML Mockup:** [`ui_ux_design/mockups/16-attendance.html`](mockups/16-attendance.html)

```
+--------------------------------------------------------------------+
|  Stitching Basic - Batch 1 - Attendance                            |
|  +-------------------------------------------------------------+  |
|  | Date: [<] March 11, 2026 [>]                                |  |
|  +-------------------------------------------------------------+  |
|                                                                    |
|  Class 15 of 48 | Marked: 0/18 | [Mark All Present]               |
|                                                                    |
|  +----------+-----------+----------+----------+------------+       |
|  | Name     | Attend.%  | Today    | Streak   | Alert      |       |
|  +----------+-----------+----------+----------+------------+       |
|  | Priya S. | 93% green | (•)P (○)A (○)L | 12 days   |            |
|  | Kavita R.| 68% red   | (○)P (○)A (○)L | 0 (abs 2) | Below 70% |
|  | Sunita M.| 85% green | (○)P (○)A (○)L | 8 days    |            |
|  | Anita K. | 72% yellow| (○)P (○)A (○)L | 1 day     | At risk    |
|  +----------+-----------+----------+----------+------------+       |
|                                                                    |
|  P = Present  A = Absent  L = Leave                                |
|                                                                    |
|  +-------------------------------------------------------------+  |
|  | [Save Attendance]                                            |  |
|  +-------------------------------------------------------------+  |
+--------------------------------------------------------------------+
```

**Design Rationale:**
- Accessed as a tab within Batch Detail (trainer sees only their assigned batches).
- **Date navigator** with arrows for day-by-day navigation. Calendar icon for date picker.
- **"Class X of Y"** provides progress context. "Marked: 0/18" shows how many marked today.
- **Three-state radio:** Present (green), Absent (red), Leave (yellow). Larger than standard radio buttons (44px touch targets). Leave is new compared to the old design — the updated PRD specifies it.
- **Attendance % column** — three-tier color: green (>80%), yellow (70-80%), red (<70%). Uses colored dot + mini progress bar for dual encoding (accessible).
- **Streak column** — consecutive present days or absent days for context.
- **Alert column** — "Below 70%" (red badge) and "At risk" (yellow badge) only for flagged users.
- **"Mark All Present"** — convenience button, then mark individual absences/leaves.
- **Save Attendance** — saves all markings for the current date. Confirmation toast: "Attendance saved for March 11."

---

### 4.13 Assessment Entry (`/trainer/batches/:id` — Assessments Tab)

**Screenshot:** [`ui_ux_design/screenshots/17-assessment-entry.png`](screenshots/17-assessment-entry.png)
**HTML Mockup:** [`ui_ux_design/mockups/17-assessment-entry.html`](mockups/17-assessment-entry.html)

```
+--------------------------------------------------------------------+
|  Stitching Basic - Batch 1 - Assessments                           |
|                                                                    |
|  Course Weights: MCQ (30%) | Practical (50%) | Case Study (20%)    |
|  Pass Threshold: 50%                                               |
|                                                                    |
|  +----------+---------+----------+----------+-------+--------+     |
|  | Name     | MCQ/100 | Prac/100 | Case/100 | Final | Result |     |
|  +----------+---------+----------+----------+-------+--------+     |
|  | Priya S. | [  85 ] | [  90  ] | [  78  ] | 86.1% | Pass ✓ |     |
|  | Kavita R.| [  60 ] | [  45  ] | [  50  ] | 50.0% | Pass ✓ |     |
|  | Sunita M.| [  70 ] | [  80  ] | [  65  ] | 74.0% | Pass ✓ |     |
|  | Anita K. | [  40 ] | [  35  ] | [  30  ] | 35.0% | Fail ✗ |     |
|  +----------+---------+----------+----------+-------+--------+     |
|                                                                    |
|  Trainer Remarks (per candidate — click name to expand):           |
|  +-------------------------------------------------------------+  |
|  | Anita K.: [Needs more practice with machine operation.     ] |  |
|  |           Recommend re-assessment in 2 weeks.                |  |
|  +-------------------------------------------------------------+  |
|                                                                    |
|  Summary: 3 Pass | 1 Fail | Pass Rate: 75%                        |
|                                                                    |
|  [Save Assessments]     [Publish Results]                          |
+--------------------------------------------------------------------+
```

**Design Rationale:**
- **Weight display at top** — trainer sees the grading formula before entering scores. Weights come from course configuration.
- **Score entry per assessment type** — each column is an editable number field (0-100). Final percentage calculated automatically using weighted formula.
- **Auto-calculated result** — Pass/Fail determined by threshold (default 50%, configurable per course). Green checkmark for pass, red X for fail.
- **Trainer remarks** — expandable per candidate. Text area for qualitative feedback. Important for candidates who fail — documents reason and recommendation.
- **Summary bar** at bottom — quick stats: pass count, fail count, pass rate.
- **Two-stage save** — "Save Assessments" saves as draft (editable). "Publish Results" locks scores and makes them visible in reports. Confirmation dialog before publish.

---

### 4.14 Assessment Results View (`/admin/reports` or Batch Detail — Assessments Tab, read-only)

**Screenshot:** [`ui_ux_design/screenshots/18-assessment-results.png`](screenshots/18-assessment-results.png)
**HTML Mockup:** [`ui_ux_design/mockups/18-assessment-results.html`](mockups/18-assessment-results.html)

```
+--------------------------------------------------------------------+
|  Assessment Results — Stitching Basic, Batch 1                     |
|                                                                    |
|  +----------+ +----------+ +----------+ +----------+               |
|  |Total     | |Pass Rate | |Avg Score | |Highest   |               |
|  |Assessed  | |          | |          | |Score     |               |
|  |   18     | |   83%    | |   72.4%  | |  92.1%   |               |
|  +----------+ +----------+ +----------+ +----------+               |
|                                                                    |
|  +----------+---------+----------+----------+-------+--------+     |
|  | Name     | MCQ     | Practical| Case St. | Final | Result |     |
|  +----------+---------+----------+----------+-------+--------+     |
|  | Priya S. | 85      | 90       | 78       | 86.1% | Pass   |     |
|  | Kavita R.| 60      | 45       | 50       | 50.0% | Pass   |     |
|  | Anita K. | 40      | 35       | 30       | 35.0% | Fail   |     |
|  +----------+---------+----------+----------+-------+--------+     |
|                                                                    |
|  [Export CSV]                                                      |
+--------------------------------------------------------------------+
```

**Design Rationale:**
- Read-only view of published assessment results. Accessible by Admin and OC.
- **Stat cards** at top: total assessed, pass rate, average score, highest score.
- **Table** shows all candidates with scores and results, sortable by any column.
- **Export CSV** for reporting purposes — one of the few export features in Phase 1, justified by donor reporting needs.

---

### 4.15 Placement Tracking (`/oc/placements`)

**Screenshot:** [`ui_ux_design/screenshots/19-placement-tracking.png`](screenshots/19-placement-tracking.png)
**HTML Mockup:** [`ui_ux_design/mockups/19-placement-tracking.html`](mockups/19-placement-tracking.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Placement Tracking          [+ Record Placement]    | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |  |Placed    | |Active    | |Left Job  | |Pending   | | |
| |        |  |  |Total     | |Jobs      | |          | |Follow-up | | |
| |        |  |  |   45     | |    38    | |     7    | |    12    | | |
| |        |  |  +----------+ +----------+ +----------+ +----------+ | |
| |        |  |                                                      | |
| |        |  |  [Search...    ] [Status: All v] [Course: All v]     | |
| |        |  |                                                      | |
| |        |  |  +----------+----------+--------+--------+--------+ | |
| |        |  |  | Name     | Employer | Role   | Salary | Status | | |
| |        |  |  +----------+----------+--------+--------+--------+ | |
| |        |  |  | Priya S. | ABC Ltd  | Tailor | ₹8000  | Active | | |
| |        |  |  | Radha P. | XYZ Sal. | Asst.  | ₹6000  | Active | | |
| |        |  |  | Meena D. | Self     | Own    | ₹5000  | Active | | |
| |        |  |  | Kavita R.| DEF Co.  | Helper | ₹7000  | Left   | | |
| |        |  |  +----------+----------+--------+--------+--------+ | |
| |        |  |                                                      | |
| |        |  |  Upcoming Follow-ups                                 | |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | Priya S. | 1-month  | Due: Mar 15  | [Record]   || |
| |        |  |  | Radha P. | 3-month  | Due: Mar 18  | [Record]   || |
| |        |  |  | Sunita M.| 6-month  | Overdue!     | [Record]   || |
| |        |  |  +--------------------------------------------------+| |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Stat cards** for placement overview — total placed, active jobs, left job, pending follow-ups.
- **Placement table** with status badges: Active (green), Left (red), Unknown (gray).
- **"Record Placement" button** opens a modal with fields: Candidate (searchable dropdown, showing only "Training Completed" candidates), Employer Name, Job Role, Salary (₹), Placement Date, Notes.
- **Upcoming Follow-ups section** — surfaces the automated reminders. System auto-creates follow-up tasks at 1, 3, and 6 months after placement date. "Record" button opens a follow-up form: Job Continuation Status (Active/Left/Unknown), Current Salary, Notes.
- **Overdue follow-ups** highlighted in red — ensures no follow-up is missed.

---

### 4.16 Placement Detail (`/oc/placements/:id`)

**Screenshot:** [`ui_ux_design/screenshots/20-placement-detail.png`](screenshots/20-placement-detail.png)
**HTML Mockup:** [`ui_ux_design/mockups/20-placement-detail.html`](mockups/20-placement-detail.html)

```
+----------------------------------------------------------------------+
|  <- Back    Priya Sharma — Placement Record                          |
|                                                                      |
|  +----------------------------------------------------------------+  |
|  | Employer: ABC Textiles Ltd    | Role: Tailor                   |  |
|  | Salary: ₹8,000/month         | Placed: Feb 15, 2026           |  |
|  | Status: Active                                                 |  |
|  +----------------------------------------------------------------+  |
|                                                                      |
|  Follow-up Timeline                                                  |
|  +----------------------------------------------------------------+  |
|  |  1 Month (Mar 15)        ✓ Completed                          |  |
|  |  Status: Active | Salary: ₹8,000 | "Doing well, learning new  |  |
|  |  patterns"                                                     |  |
|  |                                                                |  |
|  |  3 Months (May 15)       ○ Upcoming                           |  |
|  |  Due in 65 days                                                |  |
|  |                                                                |  |
|  |  6 Months (Aug 15)       ○ Upcoming                           |  |
|  |  Due in 157 days                                               |  |
|  +----------------------------------------------------------------+  |
|                                                                      |
|  Candidate Info                                                      |
|  Course: Stitching Basic | Batch: B1 | Assessment: 86.1% (Pass)     |
|  [View Full Profile]                                                 |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Placement header** with key employment details.
- **Follow-up timeline** — vertical timeline showing 1/3/6 month milestones. Completed follow-ups show recorded data. Upcoming ones show due date countdown. Overdue shows red warning.
- **Candidate context** — links back to the candidate profile for full history.

---

### 4.17 Trainer Dashboard (`/trainer/dashboard`)

**Screenshot:** [`ui_ux_design/screenshots/21-trainer-dashboard.png`](screenshots/21-trainer-dashboard.png)
**HTML Mockup:** [`ui_ux_design/mockups/21-trainer-dashboard.html`](mockups/21-trainer-dashboard.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  My Dashboard                        [bell] [avatar] | |
| | Dash   |  +------------------------------------------------------+ |
| | Batches|  |                                                      | |
| | Notif  |  |  Welcome, Suman!                                     | |
| |        |  |                                                      | |
| |        |  |  My Active Batches                                   | |
| |        |  |                                                      | |
| |        |  |  +---------------------------------------------------+| |
| |        |  |  | Stitching Basic - Batch 1                        || |
| |        |  |  | 18 candidates | Mar 1 - Jun 30                   || |
| |        |  |  | Class 15 of 48 | Avg Attendance: 84%              || |
| |        |  |  | Today's attendance: Not yet marked                || |
| |        |  |  | [Mark Attendance]  [View Batch]                   || |
| |        |  |  +---------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  +---------------------------------------------------+| |
| |        |  |  | Computers Fund. - Batch 2                        || |
| |        |  |  | 19 candidates | Mar 1 - May 31                   || |
| |        |  |  | Class 22 of 40 | Avg Attendance: 91%              || |
| |        |  |  | Today's attendance: ✓ Marked                     || |
| |        |  |  | [View Batch]                                      || |
| |        |  |  +---------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  Low Attendance Alerts                               | |
| |        |  |  +---------------------------------------------------+| |
| |        |  |  | Kavita R. (Stitching B1) — 68% ⚠                || |
| |        |  |  | Anita K. (Stitching B1) — 72% ⚠                || |
| |        |  |  +---------------------------------------------------+| |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Simplified sidebar** — only 3 items (Dashboard, My Batches, Notifications). Trainers don't need access to applications, placements, or system config.
- **Batch cards** — each active batch as a card with key stats. Most important action surfaced: "Mark Attendance" for today if not yet done. Once marked, shows green checkmark.
- **Low Attendance Alerts** — trainer sees their own students below 70%. Clicking a name navigates to that student's attendance detail.
- **Welcome message** — personal greeting for a friendly interface.

---

### 4.18 Notifications Center (All Roles)

**Screenshot:** [`ui_ux_design/screenshots/22-notifications.png`](screenshots/22-notifications.png)
**HTML Mockup:** [`ui_ux_design/mockups/22-notifications.html`](mockups/22-notifications.html)

```
+----------------------------------------------------------------------+
| +--------+  +------------------------------------------------------+ |
| |Sidebar |  |  Notifications                  [Mark All Read]      | |
| |        |  +------------------------------------------------------+ |
| |        |  |                                                      | |
| |        |  |  Today                                               | |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | ⚠ Low attendance alert: Kavita R. (68%)         || |
| |        |  |  | Stitching B1 — 2 hours ago                      || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | 📋 Placement follow-up due: Priya S.            || |
| |        |  |  | 1-month check — Due today                        || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  Yesterday                                           | |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | ✓ Batch B3 created: Beauty Basic                || |
| |        |  |  | By Vinay A. — yesterday at 3:45 PM              || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |  | 📊 Assessment results published: Stitching B1   || |
| |        |  |  | Pass rate: 83% — yesterday at 11:30 AM          || |
| |        |  |  +--------------------------------------------------+| |
| |        |  |                                                      | |
| |        |  |  Older                                               | |
| |        |  |  ...                                                 | |
| +--------+  +------------------------------------------------------+ |
+----------------------------------------------------------------------+
```

**Design Rationale:**
- **Grouped by time** — Today, Yesterday, Older. Newest first.
- **Icon + color per type** — warning (yellow) for attendance alerts, clipboard (blue) for follow-ups, checkmark (green) for confirmations, chart (purple) for reports.
- **Unread notifications** have a blue left border and slightly different background.
- **Click navigates** to the relevant page (e.g., clicking attendance alert goes to that batch's attendance tab).
- **"Mark All Read"** button for cleanup.
- **Role-filtered** — each role sees only relevant notifications:
  - Admin: all system notifications
  - OC: application updates, follow-up reminders, batch assignments
  - Trainer: attendance alerts, assessment reminders, batch updates

---

## 5. Interaction Flows & State Diagrams

### 5.1 Candidate Application Status Flow

```
New Application
    ↓
Under Review (Admin/OC reviews application)
    ↓
Selected (Approved for training)
    ↓
Assigned to Batch (OC assigns to specific batch)
    ↓
Training In Progress
    ↓
Training Completed
    ↓
+-------+-------+
|               |
Placed      Not Placed
```

Additional transitions:
- Any status → Rejected (application rejected at review)
- Any status → Withdrawn (candidate drops out)
- Draft → New Application (OC completes and submits a saved draft)

### 5.2 Candidate Status State Machine

| Status | Badge Color | Available Actions |
|--------|-------------|-------------------|
| Draft | Gray | Edit, Submit, Delete |
| New Application | Blue | Review, Reject |
| Under Review | Yellow | Select, Reject |
| Selected | Cyan | Assign to Batch, Reject |
| Assigned to Batch | Green | Start Training, Remove from Batch |
| Training In Progress | Teal | Complete Training, Withdraw |
| Training Completed | Dark Green | Record Placement, Mark Not Placed |
| Placed | Green (success) | Record Follow-up |
| Not Placed | Red | Record Placement (if placed later) |
| Rejected | Red (muted) | Re-open Application |
| Withdrawn | Gray | Re-open Application |

### 5.3 Assessment Flow

```
Trainer enters scores per candidate (MCQ, Practical, Case Study)
    ↓
System auto-calculates weighted final score
    ↓
System auto-determines Pass/Fail based on threshold
    ↓
Trainer adds remarks (required for Fail)
    ↓
Save as Draft (editable)
    ↓
Publish Results (locked, visible in reports)
```

### 5.4 Placement Follow-up Flow

```
OC records placement (employer, role, salary, date)
    ↓
System creates 3 follow-up tasks:
    - 1 month after placement date
    - 3 months after placement date
    - 6 months after placement date
    ↓
As each due date approaches (7 days before):
    → In-app notification to OC
    ↓
OC records follow-up:
    - Job continuation: Active / Left / Unknown
    - Current salary
    - Notes
    ↓
If "Left" → Status changes to "Left Job"
If "Active" → Remains "Active", next follow-up awaits
If "Unknown" → Flagged for re-contact
```

### 5.5 Authentication Flow

```
User navigates to /login
    ↓
Enters email + password
    ↓
System validates credentials
    ↓
+------- Role check -------+
|           |               |
Admin    OC            Trainer
  ↓        ↓               ↓
/admin/  /oc/          /trainer/
dashboard dashboard    dashboard
```

### 5.6 Batch Lifecycle

| Status | Meaning | Transitions |
|--------|---------|-------------|
| Upcoming | Created, not yet started | → Ongoing (start date reached) |
| Ongoing | Training in progress | → Completed (end date reached or manual) |
| Completed | Training finished | Terminal state |

---

## 6. Responsive Design

### 6.1 Breakpoints & Layout Behavior

| Breakpoint | Width | Layout |
|-----------|-------|--------|
| Mobile | <768px | Sidebar hidden, hamburger menu opens drawer. Tables convert to stacked cards. Single-column forms. |
| Tablet | 768-1023px | Collapsed sidebar (icons only), expands on hover. 2-column form fields where appropriate. |
| Desktop | 1024px+ | Full sidebar with labels. 12-column content grid. Multi-column forms. |

### 6.2 Tables on Mobile

All data tables convert to stacked card lists on mobile:

```
+-------------------------------------+
|  Priya Sharma              New      |
|  Phone: 9876543210                  |
|  Course: Stitching Basic            |
|  Applied: Mar 10, 2026             |
|  [View Profile]                     |
+-------------------------------------+
```

### 6.3 Application Wizard on Mobile

- Stepper becomes compact (numbers only, no labels)
- Form fields stack to single column
- Full-width buttons
- Same 5-step flow, more vertical scrolling per step

### 6.4 Dashboard on Mobile

- Stat cards: 2x2 grid instead of 4-across
- Tables show top 3 items with "View All" link
- Quick action buttons stack vertically

---

## 7. Accessibility (WCAG 2.1 AA)

| Requirement | Implementation |
|-------------|---------------|
| Color contrast | All text/bg combinations meet 4.5:1 (normal) / 3:1 (large text). Status badges use color AND text — never color alone |
| Keyboard navigation | Full tab order. Focus visible ring (2px, brand-blue). Skip-to-content link. Escape closes modals |
| Screen readers | ARIA labels on icon-only buttons. `role="alert"` on toasts. `aria-live="polite"` on dynamic content. Table headers use `scope` |
| Touch targets | Minimum 44x44px for all buttons, links, checkboxes, radio buttons |
| Form accessibility | Labels via `for`/`id`. Errors linked via `aria-describedby`. Required fields marked with `*` and `aria-required` |
| Motion | Respect `prefers-reduced-motion`. Animations max 200ms |
| Zoom | Content reflows at 200% zoom without horizontal scroll |

---

## Appendix: Screen Inventory

| # | Screen | Route | Screenshot | HTML Mockup |
|---|--------|-------|------------|-------------|
| 01 | Login | `/login` | `screenshots/01-login.png` | `mockups/01-login.html` |
| 02 | Admin Dashboard | `/admin/dashboard` | `screenshots/02-admin-dashboard.png` | `mockups/02-admin-dashboard.html` |
| 03 | Course Management | `/admin/courses` | `screenshots/03-course-management.png` | `mockups/03-course-management.html` |
| 04 | Add/Edit Course | `/admin/courses` (modal) | `screenshots/04-add-course-modal.png` | `mockups/04-add-course-modal.html` |
| 05 | Batch Management | `/admin/batches` | `screenshots/05-batch-management.png` | `mockups/05-batch-management.html` |
| 06 | Batch Detail | `/admin/batches/:id` | `screenshots/06-batch-detail.png` | `mockups/06-batch-detail.html` |
| 07 | User Management | `/admin/users` | `screenshots/07-user-management.png` | `mockups/07-user-management.html` |
| 07b | Add Office Coordinator | `/admin/users` (modal) | `screenshots/07b-add-office-coordinator.png` | `mockups/07b-add-office-coordinator.html` |
| 07c | Add Trainer | `/admin/users` (modal) | `screenshots/07c-add-trainer.png` | `mockups/07c-add-trainer.html` |
| 08 | OC Dashboard | `/oc/dashboard` | `screenshots/08-oc-dashboard.png` | `mockups/08-oc-dashboard.html` |
| 09 | App Wizard Step 1 | `/oc/applications/new` | `screenshots/09-app-wizard-step1.png` | `mockups/09-application-wizard.html` |
| 10 | App Wizard Step 2 | `/oc/applications/new` | `screenshots/10-app-wizard-step2.png` | (same mockup) |
| 11 | App Wizard Step 3 | `/oc/applications/new` | `screenshots/11-app-wizard-step3.png` | (same mockup) |
| 12 | App Wizard Step 4 | `/oc/applications/new` | `screenshots/12-app-wizard-step4.png` | (same mockup) |
| 13 | App Wizard Step 5 | `/oc/applications/new` | `screenshots/13-app-wizard-step5.png` | (same mockup) |
| 14 | Candidate List | `/oc/applications` | `screenshots/14-candidate-list.png` | `mockups/14-candidate-list.html` |
| 15 | Candidate Profile | `/oc/applications/:id` | `screenshots/15-candidate-profile.png` | `mockups/15-candidate-profile.html` |
| 16 | Attendance Marking | `/trainer/batches/:id` | `screenshots/16-attendance.png` | `mockups/16-attendance.html` |
| 17 | Assessment Entry | `/trainer/batches/:id` | `screenshots/17-assessment-entry.png` | `mockups/17-assessment-entry.html` |
| 18 | Assessment Results | `/admin/reports` | `screenshots/18-assessment-results.png` | `mockups/18-assessment-results.html` |
| 19 | Placement Tracking | `/oc/placements` | `screenshots/19-placement-tracking.png` | `mockups/19-placement-tracking.html` |
| 20 | Placement Detail | `/oc/placements/:id` | `screenshots/20-placement-detail.png` | `mockups/20-placement-detail.html` |
| 21 | Trainer Dashboard | `/trainer/dashboard` | `screenshots/21-trainer-dashboard.png` | `mockups/21-trainer-dashboard.html` |
| 22 | Notifications | (all roles) | `screenshots/22-notifications.png` | `mockups/22-notifications.html` |

All screenshot and mockup files are located under `ui_ux_design/` relative to the project root.
