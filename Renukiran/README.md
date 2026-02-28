# RWF Learning Management System

A Learning Management System built for **Renukiran Welfare Foundation (RWF)**, an NGO that provides vocational training (stitching, computers, beauty, design) to underserved communities across India.

## Project Status

**Phase: UI/UX Design (Complete)** — MVP1 scope covers Enrollment & Batch Allocation. No implementation code yet; the repository contains the full design specification and interactive mockups.

## MVP1 Scope

The first milestone focuses on the core enrollment and batch allocation workflow:

- **Phone + OTP authentication** (no passwords, no self-registration)
- **Admin portal** — manage trainings, allocate batches, track attendance, manage users
- **User portal** — browse trainings, enroll with slot preferences, view attendance
- **Email-based interactions** — waitlist opt-out/wait via token-based deep links (no login required)
- **Notification log** — track email delivery status and user responses
- **8 languages** — English, Hindi, Telugu, Kannada, Tamil, Malayalam, Marathi, Bengali

## Repository Structure

```
Renukiran/
├── README.md
├── RWF Learning Management System_V1.3.docx   # Requirements document
└── ui_ux_design/
    ├── design-document.md                      # Full UI/UX specification (v2.0)
    ├── mockups/                                # 16 interactive HTML mockups
    │   ├── 01-login.html
    │   ├── 02-otp-verify.html
    │   ├── 03-admin-dashboard.html
    │   ├── 04-admin-trainings.html
    │   ├── 05-admin-add-training.html
    │   ├── 06-admin-batch-allocation.html
    │   ├── 07-admin-batch-confirm-dialog.html
    │   ├── 08-admin-attendance.html
    │   ├── 09-admin-users.html
    │   ├── 10-user-browse-trainings.html
    │   ├── 11-user-enrollment.html
    │   ├── 12-user-enrollment-success.html
    │   ├── 13-user-dashboard.html
    │   ├── 14-email-response.html
    │   ├── 15-email-response-confirm.html
    │   └── 16-admin-notifications.html
    └── screenshots/                            # High-fidelity PNG screenshots
        ├── 01-login.png
        ├── ...
        └── 16-admin-notifications.png
```

## Screen Inventory

### Authentication

| # | Screen | Route | Description |
|---|--------|-------|-------------|
| 01 | Login | `/` | Phone number input with +91 prefix, language selector, OTP trigger |
| 02 | OTP Verification | `/verify` | 6-digit segmented input, auto-advance, resend countdown |

### Admin Portal

| # | Screen | Route | Description |
|---|--------|-------|-------------|
| 03 | Dashboard | `/admin/dashboard` | KPI stat cards, recent enrollments table, training capacity bars, attendance alerts |
| 04 | Training List | `/admin/trainings` | Card-based training list with search, filter, sort, capacity bars |
| 05 | Add Training | `/admin/trainings` (modal) | Modal form — name, category, duration, slots, timeslots, status |
| 06 | Batch Allocation | `/admin/batches/:id` | Core workflow: FCFS-sorted enrollee table, checkbox selection, bulk approve/waitlist/reject |
| 07 | Allocation Confirm | `/admin/batches/:id` (dialog) | Confirmation dialog listing users and assigned slots with capacity warnings |
| 08 | Attendance | `/admin/trainings/:id/attendance` | Date/slot navigator, present/absent radios, attendance %, streak, low-attendance alerts |
| 09 | User Management | `/admin/users` | Data table with role/status filters, add user modal, bulk delete, CSV export |
| 16 | Notification Log | `/admin/notifications` | Email delivery tracking — type, status, response, resend failed, summary stats |

### User Portal

| # | Screen | Route | Description |
|---|--------|-------|-------------|
| 10 | Browse Trainings | `/user/trainings` | Training cards with slot availability, urgency badges, enroll CTA |
| 11 | Enrollment Form | `/user/trainings/:id` | Slot preference selection (Pref 1 + Pref 2) via large radio cards |
| 12 | Enrollment Success | `/user/trainings/:id/success` | Confirmation with green checkmark animation, "what happens next" steps |
| 13 | User Dashboard | `/user/dashboard` | Current training, attendance/progress rings, attendance calendar, next class |

### Email Response (No Auth)

| # | Screen | Route | Description |
|---|--------|-------|-------------|
| 14 | Email Response | `/respond/:token` | Waitlist decision — "Wait for Next Batch" or "Opt Out" via large action cards |
| 15 | Response Confirm | `/respond/:token/confirm` | Confirmation of saved response |

## Design System

| Token | Value | Usage |
|-------|-------|-------|
| Brand Blue | `#2B5EA7` | Primary actions, active nav, admin interface |
| Brand Green | `#7CB518` | Accent, enroll CTAs, user portal highlights |
| Success | `#16A34A` | Approved, confirmed, good attendance |
| Warning | `#EAB308` | Waitlisted, at-risk attendance |
| Danger | `#DC2626` | Rejected, low attendance, errors |

- **Typography**: Inter (Latin) + Noto Sans (Indic scripts)
- **Grid**: 12-column desktop (1280px max), 4-column mobile
- **Breakpoints**: sm 640px, md 768px, lg 1024px, xl 1280px
- **Touch targets**: Minimum 44x44px (WCAG 2.5.5)
- **Accessibility**: WCAG 2.1 AA — contrast ratios, keyboard nav, ARIA labels, focus management

## Key Workflows

### Enrollment Flow
```
User Login (OTP) -> Browse Trainings -> Select Slot Preferences -> Submit
    -> Status: PENDING
        -> Admin Approves  -> APPROVED  -> Confirmation Email
        -> Admin Waitlists -> WAITLISTED -> Email with Opt Out / Wait links
            -> User: Wait     -> WAITING (queued for next batch)
            -> User: Opt Out  -> OPTED_OUT
        -> Admin Rejects   -> REJECTED
```

### Batch Allocation (Admin)
1. Open training's batch allocation screen
2. Review enrollees sorted by date (FCFS)
3. Select users via checkboxes
4. Approve (assigns to Pref 1; if full, prompts Pref 2), Waitlist, or Reject
5. Send confirmation/waitlist emails in bulk

### Attendance
1. Admin selects date + slot, marks present/absent per user
2. System calculates attendance % with color-coded thresholds (green >80%, yellow 70-80%, red <70%)
3. Admin sends alerts to users below 70%

## Viewing the Mockups

Open any HTML file from `ui_ux_design/mockups/` in a browser. Each mockup is a self-contained HTML file with inline CSS using the project's design system (Inter font, brand colors, proper spacing). They render at 1280px viewport width.

## Documents

- **Requirements**: `RWF Learning Management System_V1.3.docx` — full project requirements
- **UI/UX Specification**: `ui_ux_design/design-document.md` — comprehensive design doc covering information architecture, design system, page-by-page designs with wireframes, interaction flows, responsive behavior, accessibility, and i18n

## Design Principles

- **Low digital literacy first** — large touch targets, simple language, minimal options per screen, visual cues over text
- **Mobile + Desktop equal priority** — responsive layouts with admin tables converting to stacked cards on mobile
- **Admin-controlled access** — no self-registration; users are added by admins to maintain data quality
- **FCFS fairness** — enrollment sorted by date, making allocation decisions transparent and fair
- **Token-based email interactions** — waitlist responses via deep links without requiring login
