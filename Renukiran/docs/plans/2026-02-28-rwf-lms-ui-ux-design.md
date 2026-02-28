# RWF Learning Management System — UI/UX Design Document

**Version:** 1.0
**Date:** 2026-02-28
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
9. [Tech Stack Recommendation](#9-tech-stack-recommendation)

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

Semantic color system. Values are brand-derived placeholders — finalize when logo is provided.

| Token | Role | Value | Usage |
|-------|------|-------|-------|
| `--primary` | Brand primary | `#E8762B` (warm orange/saffron) | CTAs, active nav, key actions |
| `--primary-light` | Primary tint | `#FFF3EB` | Hover states, selected rows, badges |
| `--primary-dark` | Primary shade | `#C45E1A` | Pressed states, focus rings |
| `--secondary` | Supporting | `#1A6B5E` (deep teal) | Secondary buttons, info badges |
| `--secondary-light` | Secondary tint | `#E8F5F2` | Secondary backgrounds |
| `--neutral-50` | Lightest gray | `#FAFAFA` | Page backgrounds |
| `--neutral-100` | Light gray | `#F4F4F5` | Card backgrounds, table stripes |
| `--neutral-200` | Border gray | `#E4E4E7` | Borders, dividers |
| `--neutral-600` | Body text | `#52525B` | Secondary text |
| `--neutral-900` | Headings | `#18181B` | Headings, primary text |
| `--success` | Positive | `#16A34A` | Approved, confirmed, good attendance |
| `--warning` | Caution | `#EAB308` | Waitlisted, below threshold |
| `--danger` | Error/alert | `#DC2626` | Rejected, low attendance, errors |
| `--info` | Informational | `#2563EB` | Links, info tooltips |

Rationale: Warm orange/saffron as primary evokes community warmth and is culturally resonant in India. Teal as secondary provides strong contrast. All text/background combinations meet WCAG AA contrast ratios.

### 3.2 Typography

| Level | Font | Size (Desktop / Mobile) | Weight | Usage |
|-------|------|------------------------|--------|-------|
| Display | Noto Sans | 32px / 24px | 700 | Page titles |
| H1 | Noto Sans | 24px / 20px | 600 | Section headers |
| H2 | Noto Sans | 20px / 18px | 600 | Card headers |
| H3 | Noto Sans | 16px / 16px | 600 | Sub-sections |
| Body | Noto Sans | 16px / 14px | 400 | Default text |
| Body-sm | Noto Sans | 14px / 13px | 400 | Table cells, captions |
| Label | Noto Sans | 12px / 12px | 500 | Form labels, badges |

Noto Sans chosen because it has unified glyphs for all 8 target scripts (Devanagari, Telugu, Kannada, Tamil, Malayalam, Bengali) with consistent x-height and visual weight. Font loading: preload Latin + user's selected script, lazy-load others.

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

### 4.1 Authentication — Login Screen (`/`)

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

Interaction notes:
- Country code defaults to +91 (India), dropdown for others
- Language selector persists choice in localStorage, applies immediately

### 4.2 Authentication — OTP Verification

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

- OTP fields auto-advance on digit entry, support clipboard paste
- Resend countdown: 30 seconds, then "Resend OTP" link becomes active
- After successful OTP, system determines role (admin/user) and redirects

### 4.3 Admin Dashboard (`/admin/dashboard`)

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

Dashboard widgets:
1. Stat Cards (top row) — Active trainings, Total enrolled, Pending approvals, Waitlist count. Clickable — navigates to relevant list
2. Recent Enrollments Table — Last 5 enrollments with quick status. "View All" goes to full enrollment management
3. Training Capacity Bars — Visual fill indicator per training (green->yellow->red as capacity fills)
4. Attendance Alerts (below fold) — Users below 70% threshold, with "Send Alert" quick action

### 4.4 Training Management (`/admin/trainings`)

Training list view with search, filter, sort. Each training is a card showing:
- Name, category icon, status badge (Active/Upcoming/Closed)
- Duration, slot capacity (used/total)
- Timeslot details (Morning/Evening times)
- Actions: View Details, Edit, overflow menu (Delete, Clone, Close)

Add/Edit Training Modal fields:
- Training Name (required)
- Category (dropdown: Stitching, Computers, Beauty, Design, etc.)
- Duration (months), Max Slots per Batch (default 20)
- Timeslots: Morning (start-end), Evening (start-end), with [+ Add Timeslot]
- Description (textarea)
- Status: Active / Upcoming / Closed

### 4.5 Batch Allocation (`/admin/batches/:id`) — Core Admin Workflow

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

Interaction flows:
1. Select users via checkboxes -> sticky bulk action bar appears at top
2. Approve -> assigns user to Pref 1 slot. If Pref 1 full, dialog: "Assign to Pref 2?"
3. Waitlist -> marks as waitlisted, user remains in queue
4. Reject -> optional reason dialog
5. Send Confirmation Emails -> emails all newly Approved users
6. Send Waitlist Notifications -> sends Opt Out / Wait deep-link emails
7. Date column shows enrollment timestamp for FCFS ordering (default sort)
8. Slot capacity bars at top update in real-time as admins approve/remove

Approval Confirmation Dialog:
- Lists users being approved with their assigned slot
- Shows slot capacity warning if nearing full
- Checkbox: "Send confirmation emails immediately"
- Actions: Cancel, Confirm & Save

### 4.6 Attendance Management (`/admin/trainings/:id` -> Attendance Tab)

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

- Date navigator with arrows and calendar picker
- Slot filter (Morning/Evening)
- Attendance % with color thresholds: green (>80%), yellow (70-80%), red (<70%)
- "Send Low Attendance Alerts" only appears when users are below 70%
- Streak column shows consecutive present/absent days

### 4.7 User Management (`/admin/users`)

Data table with search, role filter, status filter. Columns: checkbox, Name, Phone, Role (Admin/User), Training, Status (Active/Inactive/Waitlist). Bulk actions: Delete, Export CSV. Add User modal: Name, Phone, Email (optional), Role, Center.

### 4.8 User Portal — Browse Trainings (`/user/trainings`)

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

Larger fonts, warmer tone, simpler language than admin portal.

### 4.9 Enrollment Flow (`/user/trainings/:id`)

- Training detail page with description, duration, start date, class count
- Slot selection: two radio groups for Preferred Slot (Option 1) and Backup Slot (Option 2)
- Each option shows slot time and remaining seats
- Submit button with info text: "Your enrollment will be reviewed by the admin"
- Post-submission: confirmation card with "What happens next" steps (1. Admin reviews, 2. SMS confirmation, 3. Training starts date)

### 4.10 User Dashboard (`/user/dashboard`)

- Welcome greeting with user's name
- Current training card: name, slot, date range
- Two stat circles: Attendance % (color-coded) and Progress (class X of Y)
- Attendance calendar: month view with green dots (present), red dots (absent), empty (no class)
- Legend: Present count, Absent count
- Enrollment status and next class info

### 4.11 Email Response Page (`/respond/:token`)

- No auth required — token-validated
- RWF logo, personalized greeting
- Context: which training, which slot, waitlist position
- Two large action cards:
  - "Wait for Next Batch" — with explanation text
  - "Opt Out" — with explanation text
- After selection: confirmation message with "Go to Renukiran Portal" link
- Token validity: 7 days, shown at bottom

### 4.12 Notification Log (`/admin/notifications`)

Data table with filters: Type (Confirmation/Waitlist/Low Attendance/General), Status (Sent/Delivered/Opened/Failed), Date range. Columns: Date, Recipient, Type, Status, Response (for waitlist emails: Waiting/Opted Out/Pending).

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

## 9. Tech Stack Recommendation

### Recommended: Next.js 15 + shadcn/ui + Tailwind CSS 4

### 9.1 Next.js 15 (App Router)

Why Next.js over Vite/CRA/Remix:

- **Server Components reduce bundle size.** Admin data tables (batch allocation, attendance grids, user lists) render on the server and send only HTML. Users on low-bandwidth connections get faster initial page loads.

- **Route-based code splitting is automatic.** Admin portal (`/admin/*`) and user portal (`/user/*`) are separate route groups with separate bundles. A beneficiary never downloads admin table code.

- **Built-in API routes for the email response page.** `/respond/:token` needs a server endpoint to validate tokens and record responses. Next.js API routes handle this without a separate backend.

- **`next-intl` is the best i18n solution for 8 languages.** Native App Router integration, SSR for translated content, ICU message format for Hindi/Marathi pluralization, automatic locale detection via middleware.

- **Image optimization.** `next/image` auto-serves WebP/AVIF with responsive sizing.

- **Middleware for auth.** Protects admin routes at the edge before any page code loads.

### 9.2 shadcn/ui

Why shadcn/ui over Mantine, Chakra, Ant Design, Material UI:

- **You own the code.** Components are copied into `components/ui/`. Custom brand styling means editing files directly, not fighting theme APIs or `!important` overrides.

- **Built on Radix Primitives.** Every component gets WCAG-compliant keyboard navigation, focus management, and ARIA attributes. The admin batch allocation table's checkbox selection, modal confirmations, and toast notifications all need this accessibility foundation.

- **No dependency lock-in.** Mantine (~6.5MB) and Ant Design (~11MB) are full packages. shadcn/ui components are files in your repo — you control the upgrade timeline.

- **Tailwind-native.** No theme object to learn, no CSS-in-JS runtime overhead. Design tokens are CSS variables consumed by both Tailwind and custom components.

- **No unused code.** You only copy components you need. For MVP1: ~15 components (Button, Input, Table, Dialog, Tabs, Badge, Card, Select, Dropdown, Toast, Avatar, Skeleton, Sheet, Popover, Command).

Key shadcn/ui components for this project:

| Component | Usage |
|-----------|-------|
| DataTable | Batch allocation, user list, attendance grid, notification log |
| Dialog | Approval confirmation, add training, add user |
| Sheet | Mobile sidebar navigation, filter panels |
| Command | Search-enabled language picker, training search |
| Tabs | Training detail view (Slots / Enrollments / Attendance) |
| Badge | Enrollment status indicators |
| Toast | Success/error notifications after bulk actions |
| Form (+react-hook-form +zod) | All forms with validation |

### 9.3 Tailwind CSS 4

Why Tailwind over CSS Modules, Styled Components, vanilla CSS:

- **Responsive design is declarative.** `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3` handles training card layout across all breakpoints in one line.

- **Design tokens as CSS variables.** Tailwind 4's `@theme` maps to the design system. When brand colors are finalized, one file changes and the entire app updates.

- **No runtime cost.** Compiles to static CSS. No JavaScript overhead (unlike Styled Components or Emotion). Matters on low-powered phones.

- **Consistent spacing.** The 4px base unit maps to Tailwind's scale (`p-1` = 4px, `p-2` = 8px). Every developer uses the same system.

### 9.4 Supporting Libraries

| Library | Purpose | Why |
|---------|---------|-----|
| `next-intl` | i18n (8 languages) | Best Next.js App Router integration, SSR, ICU format |
| `react-hook-form` + `zod` | Forms + validation | Performant, schema validation, TypeScript-native |
| `@tanstack/react-table` | Data tables | Headless, works with shadcn/ui styling. Sort, filter, paginate, select |
| `date-fns` | Date formatting | Locale-aware, tree-shakeable |
| `lucide-react` | Icons | Consistent set, tree-shakeable, used by shadcn/ui |
| `nuqs` | URL state | Type-safe URL search params for filters, pagination |
| `sonner` | Toasts | shadcn/ui recommended, accessible |

### 9.5 Backend Suggestion (for context)

While outside UI/UX scope, consider:
- Supabase or Firebase for auth (phone OTP built-in), database, and email triggers
- Resend or SendGrid for transactional emails
- PostgreSQL for relational data (trainings -> batches -> enrollments -> attendance)

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
