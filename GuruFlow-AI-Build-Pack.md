# GuruFlow — AI Build Pack

**24 prompts. Each under 10,000 characters. No reference images — the AI designs the screens.**
**Backend-swappable. Update-friendly. Premium by brief, not by screenshot.**

---

## 0. How to use this pack

```
BEFORE ANYTHING
   0. Create AGENTS.md (§5) in the repo root and paste it once.
      Every prompt after this inherits it automatically.
        │
        ▼
For each prompt, in order:
   1. Copy the prompt box, paste, send. No images needed.
   2. The AI designs the layout itself — read its one-line
      "Design choice:" note and correct it if it drifted.
   3. YOU run the app and click through it.
   4. Judge it against the seven-point Design QA (§10).
   5. If wrong → "X is wrong because Y. Fix only that."
   6. Only then start the next prompt.
        │
        ▼
   One prompt = one thing you can test with your hands.
   Never queue two prompts. Never say "now build the rest".
```

**There are no reference screenshots in this pack, and that is the point.** You are not asking the tool to copy a picture — you are giving it a brief precise enough that it can design the screen itself and still land on the same product every time. That is what §2 exists for. The design lives in the rules, not in a picture, so it survives even when you add screens that were never designed.

If you later *do* have screenshots of your own, you can attach them to any prompt — nothing in this pack depends on their absence.

Two rules that decide whether this succeeds:

1. **Never accept code you have not run.** The single biggest reason AI-built apps collapse is accepting 2,000 lines you never clicked through.
2. **Reject anything you did not ask for.** AI tools add "nice extras". Every extra screen is a future bug. The rules file forbids it — hold the tool to it.

---

## 1. The stack

| Layer | Choice | Why |
|---|---|---|
| App | Expo Router + React Native + TypeScript | One codebase → Android, iOS, web |
| Styling | NativeWind (Tailwind) | AI writes Tailwind far more reliably than StyleSheet |
| Backend | **Anything** — Supabase today, your choice tomorrow | See §3. This is the point of the pack. |
| State | Zustand + TanStack Query | Small, AI-friendly, no boilerplate |
| Media | Bunny Stream (video), Cloudflare R2 (files) | Cheapest per-GB from the cost analysis |

---

## 2. Design language — the app must look designed, not generated

There are no reference images in this pack. That is deliberate: **you design the screens.** But an AI left alone drifts toward the generic — three equal cards, a gradient on every hero, emoji icons, twelve corner radii, centred everything. This section is the antidote. Treat it as an art director's brief, not a suggestion.

### 2.1 The three laws

```
LAW 1 — ONE FOCAL POINT PER SCREEN
   The eye must land on exactly one thing first.
   Everything else is deliberately quieter than it.
        │
        ▼
LAW 2 — DEPTH FROM BORDERS AND SPACE, NOT SHADOWS
   A 1px hairline and 18pt of padding read as premium.
   A drop shadow on every card reads as a template.
        │
        ▼
LAW 3 — DATA IS THE DECORATION
   On a data screen the biggest, boldest element is a number.
   Never an icon, an illustration, or a gradient blob.
```

### 2.2 Layout grammar

```
Every screen follows one of four shapes. Pick one, do not invent a fifth.

  A. HERO SCREEN        navy gradient header → white stat card overlapping
                        its bottom edge → 2–4 content sections
                        (used for: any role's home, batch detail, fees)

  B. LIST SCREEN        white sticky header (title + 2 icon actions)
                        → filter chips row → rows or a 2-col grid
                        → sticky footer or FAB

  C. FORM SCREEN        back chevron + title + right text action
                        → one white form card → helper line
                        → sticky primary button at the bottom

  D. FOCUS SCREEN       almost no chrome. One task, one big control.
                        (attendance grid, marks entry, video player)
```

Rules that apply to all four:

```
· 8pt grid. Side margin 20. Card padding 18. Section gap 24.
· Every list row is 64–80pt tall. Never 48, never 96.
· Maximum 3 card styles on one screen (hero, white card, tinted card).
· One primary action per screen. Secondary actions are outlined.
  Destructive actions are red text, never a red filled button.
· Sticky footers carry the primary action when the screen scrolls.
· A number that matters is 22–34pt and tabular. A number that doesn't
  is 13pt and secondary. Never in between.
```

### 2.3 Typography — five sizes, four weights, no exceptions

| Role | Size / weight | Used for |
|---|---|---|
| Page title | 34 / 800 | one per screen, top left |
| Section | 22 / 700 | card group headings |
| Card title | 17 / 700 | the name of a thing |
| Body | 15 / 500 | sentences, list subtitles |
| Secondary | 13 / 500 | metadata, helper text |
| Label | 11 / 600, +0.06em, uppercase | stat captions, table headers |

```
· Maximum two weights inside one card.
· Never 400. The lightest body weight in this product is 500.
· Every numeral is tabular. Every currency uses ₹ and Indian grouping
  (₹1,24,500 — never ₹124,500).
· Never centre-align a paragraph. Centre only a single short line in an
  empty state or a splash.
```

### 2.4 Colour discipline

```
NAVY #101B4C — STRUCTURE
  hero gradients, primary buttons, active tab, selected chips, headings.
  This is the skeleton of the app. Use it often.

SAFFRON #FF7A1A — SPOTLIGHT
  Maximum TWO instances per screen. A primary CTA inside a hero, a FAB,
  the single highlighted bar in a chart, the current chapter in a list.
  If you use it a third time, the screen loses its focal point.

TEAL / AMBER / RED — DATA STATES ONLY
  teal = paid, present, submitted, positive delta
  amber = late, pending, warning, needs attention
  red = overdue, absent, destructive
  Never decorative. Never a "brand" colour for a section header.

WHITE #FFFFFF on #F5F6FA — the canvas
  Cards are white. Backgrounds are never white. Tinted cards exist only
  for semantic meaning (#FFF1E6 for a warning, #E4F7F4 for a positive
  confirmation) and never for decoration.
```

### 2.5 Component rules

```
ICONS      one family, 1.75px stroke, 24pt grid, rounded caps.
           Never mix filled and outline in the same row. Colour is ink or
           secondary only — except inside a 44pt soft-tinted square where
           the tint carries the meaning.

AVATARS    circular only. Sizes 40 / 48 / 56 / 72. Photo, or initials on a
           soft tint. Never a square avatar, never a placeholder face.

CHARTS     thin strokes (1.5–2px), no gridlines, no shadows, no 3D.
           Exactly one element highlighted in saffron. Axis labels only at
           the first and last point, in 11pt muted.

SHEETS     bottom sheets reach 60% height maximum, 28pt top radius, drag
           handle, dimmed backdrop. They are never full-screen.

SKELETONS  shimmer blocks in #EFF2F8, shaped like the content they replace.
           Never a centred spinner on a list.

EMPTY      soft tinted circle, 20/800 headline, two lines of 13 secondary,
           one navy button. No illustrations of robots, brains, lightbulbs
           or graduation caps.
```

### 2.6 What makes AI-designed UI look cheap — forbidden

```
✗ gradient text on any heading
✗ glassmorphism, frosted blur, or translucency outside the video player
✗ 3D, isometric, or neon anything
✗ more than one accent colour inside a single card
✗ a different corner radius on every component
✗ a drop shadow on every card
✗ emoji used as icons
✗ "Lorem ipsum", "Text here", "Sample", or any placeholder copy
✗ stock-photo people grinning at the camera
✗ centred everything with uniform padding
✗ a navy gradient hero on every single screen — vary it; sub-screens get
  a white header with a navy title
✗ purple, violet, magenta, or indigo accents of any kind
```

### 2.7 The premium test — five questions before you show me a screen

```
1. Squint at it. Is the hierarchy still obvious?
2. Delete one element. Did anything get worse? If not, delete it.
3. Does any number on screen look invented? Make it plausible.
4. Is there exactly one obvious next action?
5. Would this look out of place next to Linear, Stripe or Airbnb?
   If yes, it is not finished.
```

### 2.8 Your design authority

You have real authority here. Where a prompt says **"you decide"**, choose — and justify the choice in one sentence. What you may never do:

```
· invent a colour outside the palette
· invent a component that is not in src/components/ui/
· add a screen, tab or feature that the prompt did not ask for
· change a layout shape from §2.2
· ship a screen with a spinner where a skeleton belongs
```

When you finish a screen, add one line to your reply: *"Design choice: …"* — so I can see your reasoning and correct it early.

---
## 3. The backend-swap architecture (the 10-minute rule)

The UI must **never** import Supabase. It talks to interfaces. The database sits behind an adapter.

```
┌─────────────────────────────────────────────┐
│  FEATURES (UI)  src/features/students/...   │
│  calls useStudents() — knows nothing of DB  │
└──────────────────┬──────────────────────────┘
                   ▼
┌─────────────────────────────────────────────┐
│  CONTRACTS  src/core/contracts/*.ts         │
│  StudentRepo, AuthPort, StoragePort …       │
│  plain TypeScript interfaces + domain types │
└──────────────────┬──────────────────────────┘
                   ▼
┌─────────────────────────────────────────────┐
│  ADAPTER  src/adapters/supabase/*.ts        │
│  the only place that knows about Postgres    │
└──────────────────┬──────────────────────────┘
                   ▼
┌─────────────────────────────────────────────┐
│  THE SWAP FILE  src/core/config/providers.ts│
│  export const backend = supabaseAdapter     │
│  ← change this ONE line to change backend   │
└─────────────────────────────────────────────┘
```

**To move to a different backend later:**

```
1. Copy src/adapters/supabase/ → src/adapters/yourbackend/
2. Rewrite the internals of each *.repo.ts (keep the signatures identical)
3. Add your env keys to .env and src/core/config/env.ts
4. Change one line in src/core/config/providers.ts
5. Open /dev/contract-check and confirm every method passes
        │
        ▼
   ~10 minutes of work. Zero UI files touched.
```

That is why P2 (contracts) and P24 (contract check) exist. They are the whole trick.

---

## 4. Rules that keep future updates cheap

```
HARD LIMITS
├── No file over 300 lines
├── No screen file contains business logic — only layout
├── Every feature lives in src/features/<name>/ and owns its files
├── Cross-feature imports go through src/features/<name>/index.ts only
├── Navigation tabs defined in a config array, never hardcoded JSX
├── All strings that a user reads come from src/lib/copy.ts
├── Every DB change is a numbered migration file
└── Adding a feature = new folder + one route file + one tab config entry
```

If those hold, adding "Library", "Transport" or "Online classes" in year two is an afternoon, not a rewrite.

---

## 5. AGENTS.md — paste once, at the very start

Create this file in the project root. Every AI tool reads it automatically on every prompt.

```
# GuruFlow — project rules

## What this is
Coaching-management app for India. One Expo app, four roles: owner,
teacher, student, parent. A student can belong to MULTIPLE institutes.

## Architecture — non-negotiable
- Expo Router, file-based routes under app/. TypeScript strict. No `any`.
- UI NEVER imports a backend SDK. UI calls domain hooks only.
- All data access goes through src/core/contracts/ interfaces, implemented
  in src/adapters/<vendor>/. The active adapter is chosen in exactly one
  file: src/core/config/providers.ts.
- Domain types (Student, Batch, …) are plain TS objects. Never expose a
  database row type or an SDK type above the adapter layer.
- All errors are normalised to AppError { code, message } before reaching UI.
- Zustand for session/org state. TanStack Query for all server data.
- NativeWind classes only. Inline styles only for dynamic values.

## Tenancy rule (the most important rule in the project)
A user is NOT tied to one institute. Never store org_id on profiles.
Membership lives in memberships(user_id, org_id, role, status).
status = pending | active | rejected. Pending users see only the waiting
screen. One user may hold 3 active memberships and switch between them.
Every table except profiles has org_id, and every query is org-scoped.

## Design system — never deviate
canvas #F5F6FA · surface #FFFFFF · ink #0B1220 · secondary #5A6684
muted #94A0BC · hairline #E7EBF5 · primary #101B4C · accent #FF7A1A
teal #0D9488 · success #16A34A · warning #F59E0B · danger #DC2626
card radius 20 · button 16 · input 14 · chip full
side margin 20 · card padding 18 · shadow 0 8px 24px rgba(16,27,76,.08)
Headings: Satoshi/General Sans 700–800. Body: Plus Jakarta Sans.
Tabular numerals. ₹ with Indian grouping (₹1,24,500).

Three design laws, applied to every screen:
1. One focal point per screen. Everything else is quieter than it.
2. Depth from 1px hairlines and whitespace, never from stacked shadows.
3. On a data screen the biggest element is a number, not an icon.

Colour roles: navy = structure (hero, primary button, active state).
Saffron = spotlight, maximum two per screen. Teal/amber/red = data state
only, never decoration. White cards on #F5F6FA; never a white background.

Forbidden without exception: gradient text, glassmorphism, 3D, neon,
purple or indigo accents, more than one accent per card, emoji as icons,
a different radius per component, shadows on every card, centred
everything, placeholder copy like "Lorem" or "Sample", stock-photo smiles,
a navy hero on every single screen.

Before showing me any screen, run the premium test: squint (is hierarchy
obvious), delete one element (did anything get worse), check every number
is plausible, confirm exactly one obvious action, and ask whether it would
look out of place next to Linear, Stripe or Airbnb.

## Seed data — the single source of truth
Names and numbers live in supabase/seed.sql only. Never hardcode them in
components. These must match everywhere:
Institute: Sunrise Coaching Classes, Patna, Bihar, code SUNRISE-PAT
Owner: Rahul Sharma
Teachers: Rohit Verma (Maths), Priya Nair (Physics), Meera Iyer (Chemistry),
          Sneha Iyer (English), Arun Kumar (Biology)
Students: Aarav Sharma (Roll 14), Ishita Bose (15), Kabir Singh (16),
          Ananya Gupta (17), Ritesh Mehta (18), Zoya Ahmed (19),
          Kabeer Singh (20), Diya Patel (21), Manav Sethi (22)
Teachers are NEVER students and vice versa.
Attendance: 46 present / 1 late / 3 absent of 50 = 92%
Class 10-A: 42 students, 92% attendance, 12 pending fees
Fees: Term 1 ₹4,999 paid · Term 2 ₹4,999 paid · Term 3 ₹4,999 due
Unit Test 3: Aarav 86, Rohit 91, Ishita 74, Meera 78, Arun 68
  class avg 78%, highest 96, lowest 41, pass 88%, Aarav rank 4 of 42

## Behaviour rules
- Every list: skeleton, empty state, error state with retry.
- Every mutation: optimistic update, toast, rollback on failure.
- Never invent a screen beyond what the prompt describes. Never add a
  feature I did not ask for. If something is unclear, ask me exactly one question.
- After writing code, list: files changed, what I should test, what you
  assumed. Nothing else.
```

---

## 6. The 24 prompts

---

### PHASE 0 — FOUNDATION

> P1–P3 are 40% of the value of this pack. If they are right, everything after is fast and the app stays swappable. If they are rushed, you rewrite in month three.

---

#### P1 — Scaffold, design tokens, UI primitives

> **Design intent —** establish the system before any screen. Every primitive must look like it belongs to a fintech product, not a school project.

```
Create a new Expo Router + TypeScript + NativeWind project called guruflow.
Node 20+, Expo SDK latest stable, TypeScript strict mode on, no `any` anywhere.

CONTEXT
This is the foundation of a coaching-management app for India. Four roles:
owner, teacher, student, parent. It must be built so the backend can be
replaced later without touching UI code — so this prompt creates the folder
skeleton that makes that possible, plus the design system. No features yet.

FOLDER STRUCTURE — create exactly this
app/                          routes only, thin files, no business logic
  (auth)/ (pending)/ (owner)/ (teacher)/ (student)/ (parent)/
  dev/                        developer-only screens
src/
  core/contracts/             interfaces (P2)
  core/config/                env.ts, providers.ts
  core/backend.tsx            context + domain hooks
  adapters/                   one folder per backend
  components/ui/              design system primitives
  features/<feature>/         screens, components, hooks, index.ts
  lib/                        theme.ts, copy.ts, format.ts, nav.ts
supabase/                     migrations/, schema.sql, seed.sql
docs/                         ADDING_A_FEATURE.md etc.

1. src/lib/theme.ts
Export typed constants — colours, radii, spacing, shadows, font sizes —
using the exact values in AGENTS.md. Every component must read from here.
Export also: shadow object, gradient pair, and a `hit` constant of 44.

2. src/lib/format.ts
inr(n) → "₹1,24,500" with Indian digit grouping, no decimals unless asked.
pct(n) → "92%". dateShort(d) → "12 Nov 2026". timeShort → "4:00 PM".
relTime(d) → "2 hours ago". All pure, all unit-testable.

3. src/lib/copy.ts
A flat object of user-facing strings grouped by area (auth, students,
fees…). Every string on screen comes from here. Indian English.

4. src/components/ui/ — NativeWind only. Each component: typed props, a
   `className` passthrough, and no hardcoded colour literals.
   Text (variants: h1 h2 title body caption label; weights; colours)
   Button (primary secondary outline ghost danger; sm md lg; loading;
     disabled; leftIcon; full width)
   Input (label above, error below in danger, leftIcon, secure toggle,
     keyboardType, focus ring 2px primary)
   Card (white, 20 radius, 1px hairline, 18 padding, optional onPress
     with press state)
   Chip · Badge · Avatar (photo + initials fallback) · Divider
   ProgressBar · Ring (SVG circular percentage with centred label)
   Sheet (bottom sheet, 28 top radius, drag handle, backdrop, 60% max height)
   Skeleton (shimmer in #EFF2F8) · EmptyState · ErrorState (retry)
   Screen (SafeAreaView + optional scroll + 20 side margin)
   TabBar (floating pill, 28 radius, shadow, 5 items, active item gets a
     soft navy pill behind the icon, red badge support)
   GradientHeader (LinearGradient #101B4C → #1E3A8A at 135°, 24 radius,
     optional overlapping bottom card slot)

5. app/dev/components.tsx
Render every primitive in every variant and state on one scrollable page,
grouped with section labels. This is my design QA page.

6. app/_layout.tsx
Providers only: SafeArea, QueryClientProvider, BackendProvider (stub for
now, real in P2), Toast host, StatusBar dark content.

ACCEPTANCE
· `npx tsc --noEmit` passes with zero errors
· /dev/components renders every primitive without layout warnings
· No file over 300 lines
· No colour hex outside theme.ts except inside GradientHeader

DO NOT
· Do not build any feature screen or tab bar navigation yet
· Do not add a database, auth, or any SDK
· Do not install packages I did not name
```

---

#### P2 — Contracts: the backend-agnostic layer

> **Design intent —** none — this is pure architecture. Do not touch any UI file.

```
Build the contract layer. This is the most important prompt in the project:
it is what lets me replace the entire backend later by editing one file.

THE RULE
Nothing above this layer may know what database exists. The UI will call
domain hooks. Hooks call interfaces. Interfaces are implemented by an
adapter. One file decides which adapter is live.

FILES TO CREATE

1. src/core/contracts/models.ts — plain domain types, camelCase, no
   database shapes, no SDK types. Export:
   Org { id, name, code, city, state, board, logoUrl, plan, status }
   Profile { id, fullName, phone, avatarUrl }
   Membership { id, userId, orgId, role, status, org?: Org }
   Role = 'owner'|'teacher'|'student'|'parent'
   MembershipStatus = 'pending'|'active'|'rejected'
   Batch { id, orgId, name, subject, teacherMembershipId, days, startTime,
           endTime, monthlyFee, capacity, studentCount? }
   Student { id, orgId, batchId, fullName, rollNo, photoUrl, parentName,
             parentPhone, userId, parentUserId, status,
             attendancePct?, feeDue? }
   Attendance { id, orgId, batchId, studentId, date, status, markedBy }
   AttendanceStatus = 'present'|'absent'|'late'
   Homework { id, orgId, batchId, subject, title, description, dueAt,
              createdBy, submittedCount?, totalCount? }
   Submission { id, homeworkId, studentId, fileUrl, submittedAt, grade }
   Exam { id, orgId, batchId, name, subject, examDate, maxMarks,
          enteredCount?, totalCount? }
   Mark { id, examId, studentId, marks }
   ContentItem { id, orgId, batchIds, type, title, url, storageKey,
                 durationSeconds, thumbnailUrl, createdAt }
   FeePlan { id, orgId, batchId, name, amount, instalments }
   FeePayment { id, orgId, studentId, term, amount, method, reference,
                receiptNo, paidOn, status, recordedBy }
   FeeTotals { collected, pending, overdue, expected }
   Notice { id, orgId, title, body, audience, category, eventDate,
            createdBy, publishedAt }
   AppNotification { id, userId, orgId, title, body, type, isRead, createdAt }
   LeaveRequest { id, orgId, requesterId, studentId, leaveType, fromDate,
                  toDate, reason, status, decidedBy, decidedAt }

2. src/core/contracts/query.ts
   ListQuery { search?: string; filters?: Record<string, string|string[]>;
               sort?: { field: string; dir: 'asc'|'desc' };
               page?: number; pageSize?: number }
   Paginated<T> { rows: T[]; total: number; hasMore: boolean }

3. src/core/contracts/errors.ts
   type AppErrorCode = 'unauthorized'|'not_found'|'conflict'|'validation'
                     |'network'|'unknown'
   class AppError extends Error { readonly code: AppErrorCode; cause?: unknown }
   plus isAppError(e) and toAppError(e, fallbackCode).

4. src/core/contracts/ports.ts — every method async, every method org-scoped.
   AuthPort { signInWithPassword(emailOrPhone, password),
              signInWithOtp(phone), verifyOtp(phone, token),
              signUp(name, email, password), signOut(), getSession(),
              onChange(cb): () => void, signInWithGoogle?() }
   OrgRepo { get(orgId), getByCode(code), search(term), create(input),
             update(orgId, patch) }
   MembershipRepo { listMine(), listPending(orgId), requestJoin(orgId),
                    approve(orgId, id), reject(orgId, id), approveAll(orgId),
                    setRole(orgId, id, role) }
   StudentRepo { list(orgId, q): Paginated<Student>, get(orgId, id),
                 create(orgId, input), update(orgId, id, patch),
                 remove(orgId, id) }
   BatchRepo { list, get, create, update, remove, listForTeacher(orgId, mid) }
   AttendanceRepo { getSheet(orgId, batchId, date): Attendance[],
                    saveSheet(orgId, batchId, date, rows): void,
                    listForStudent(orgId, studentId, range),
                    monthlySummary(orgId, batchId, month) }
   ExamRepo { list, get, create, update, remove,
              getMarks(orgId, examId), saveMarks(orgId, examId, rows),
              analysis(orgId, examId) }
   MarkRepo { listForStudent(orgId, studentId) }
   FeeRepo { getPlan(orgId, batchId), savePlan(orgId, input),
             listPayments(orgId, q), recordPayment(orgId, input),
             totals(orgId, term): FeeTotals, dueList(orgId, term) }
   ContentRepo { list(orgId, q), create(orgId, input), remove(orgId, id),
                 storageUsed(orgId) }
   NoticeRepo { list(orgId, q), create(orgId, input), remove(orgId, id) }
   NotificationRepo { listMine(q), markRead(id), markAllRead(), unreadCount(),
                      create(orgId, userIds, payload) }
   LeaveRepo { list(orgId, q), create(orgId, input), decide(orgId, id, ok) }
   StoragePort { upload(bucket, file, onProgress?): Promise<{url,key}>,
                 remove(bucket, key), signedUrl(bucket, key, ttlSeconds) }
   PaymentPort { createIntent(orgId, paymentId): Promise<{url}> }
   RealtimePort { subscribe(topic, cb): () => void }   // optional
   Backend { auth, orgs, memberships, students, batches, attendance, exams,
             marks, fees, content, notices, notifications, leave,
             storage, payment, realtime? }

5. src/core/config/env.ts
Read EXPO_PUBLIC_* variables, validate them, export a typed env object and
an `assertEnv()` that throws one clear message listing every missing key.

6. src/core/config/providers.ts — THE SWAP FILE, and the only file that
names a vendor:
   import { supabaseAdapter } from '@/adapters/supabase'
   export const backend: Backend = supabaseAdapter

7. src/core/backend.tsx
BackendProvider + useBackend(). Then one hook per domain, all built on
TanStack Query and calling only the contracts:
useStudents(orgId, q) useStudent(orgId,id) useCreateStudent() …
useBatches, useAttendanceSheet, useSaveAttendance, useExams, useMarks,
useFees, useContent, useNotices, useNotifications, useLeave, useMembership.
Each returns { data, loading, error, refetch } and mutations handle
optimistic update, rollback and toast centrally.

8. src/adapters/supabase/
Create index.ts exporting supabaseAdapter, with every method throwing
AppError('unknown','not implemented yet'). Real bodies arrive in P3.
Add src/adapters/README.md explaining that a new backend is a new sibling
folder implementing the same Backend interface.

9. app/dev/contract-check.tsx
A screen that calls every method of every repo with safe dummy input, and
prints a pass/fail table with the error code for each failure. I will use
this to verify a replacement backend in ten minutes.

ACCEPTANCE
· tsc passes. No adapter or SDK import anywhere in src/features or app/
· app/dev/contract-check renders and lists every port method
· Changing providers.ts to a second stub adapter changes nothing in the UI

DO NOT
· Do not implement real database calls yet
· Do not build any feature screen
```

---

#### P3 — Supabase adapter, schema, RLS, seed

> **Design intent —** none — this is data. Do not touch any UI file.

```
Implement the Supabase adapter and the database. Nothing else.

1. supabase/migrations/0001_init.sql
Every table: id uuid pk default gen_random_uuid(), created_at, updated_at
with a shared set_updated_at() trigger. org_id references orgs(id) on
delete cascade — on every table except profiles.

orgs(name, code text unique, city, state, board, logo_url, plan, status)
profiles(id uuid pk references auth.users, full_name, phone, avatar_url)
memberships(user_id, org_id, role enum, status enum,
            unique(user_id, org_id))
batches(name, subject, teacher_membership_id, days text[], start_time time,
        end_time time, monthly_fee numeric, capacity int)
students(batch_id, full_name, roll_no, photo_url, parent_name, parent_phone,
         user_id, parent_user_id, status)
attendance(batch_id, student_id, date, status enum, marked_by,
           unique(student_id, date))
homework(batch_id, subject, title, description, due_at, created_by)
homework_submissions(homework_id, student_id, file_url, submitted_at, grade)
exams(batch_id, name, subject, exam_date, max_marks)
marks(exam_id, student_id, marks numeric, unique(exam_id, student_id))
content(batch_ids uuid[], type enum, title, url, storage_key,
        duration_seconds, thumbnail_url, created_by)
fee_plans(batch_id, name, amount, instalments jsonb)
fee_payments(student_id, term, amount, method, reference, receipt_no,
             paid_on, status enum, recorded_by)
notices(title, body, audience text[], category, event_date, created_by,
        published_at)
notifications(user_id, title, body, type, is_read)
leave_requests(requester_id, student_id, leave_type, from_date, to_date,
               reason, status, decided_by, decided_at)

2. supabase/migrations/0002_rls.sql
Helper functions (security definer, stable):
  auth_org_ids() returns setof uuid — orgs where the caller has an ACTIVE
    membership
  has_role(org uuid, roles text[]) returns boolean
  my_student_ids() returns setof uuid — students where user_id = auth.uid()
    or parent_user_id = auth.uid()
Enable RLS on every table. Policy rules:
  · read: org_id = any(auth_org_ids()) AND role allows that table
  · students/parents: additionally restricted to my_student_ids() for
    attendance, marks, submissions, fee_payments
  · write attendance/marks/homework/content: owner or teacher only
  · write fee_plans/fee_payments/memberships/orgs: owner only
  · pending membership grants nothing at all
Write one comment line above each policy saying what it prevents.

3. supabase/seed.sql
Exactly the data in AGENTS.md and nothing invented:
1 org, 1 owner profile+membership, 5 teacher profiles+memberships,
12 students in Class 10-A with roll numbers 14–22 plus 3 more,
30 days of attendance at 46 present / 1 late / 3 absent,
Unit Test 3 marks exactly as listed, 3 fee terms, 6 content items,
4 notices, 6 pending membership requests from other users.
Use fixed UUIDs so screenshots stay stable across reseeds.

4. Implement src/adapters/supabase/ fully, one file per domain:
   client.ts (createClient with the anon key only), mappers.ts (row →
   domain, domain → row), and <domain>.repo.ts for each port.
   Every method: enforce orgId in the query as well as relying on RLS,
   map snake_case → camelCase, map every error to AppError with the right
   code (PGRST116 → not_found, 23505 → conflict, 42501 → unauthorized).
   auth.adapter.ts wraps Supabase Auth and exposes onChange via
   onAuthStateChange. storage.adapter.ts uses Supabase Storage.
   realtime.adapter.ts subscribes to notifications and memberships.

5. Run app/dev/contract-check and fix until every method passes.

ACCEPTANCE
· Seed loads cleanly; 12 students, 5 teachers, 3 terms visible
· contract-check all green
· No SDK import outside src/adapters/
· A query from a second org returns zero rows

DO NOT
· Do not change any contract signature — if something is missing, add the
  method to the contract first, then implement it
· Do not build UI
```

---

### PHASE 1 — IDENTITY

#### P4 — Auth screens

> **Design intent —** the first ten seconds. Splash and login must feel like a bank app. Calm, dark, confident.

```
Build the auth flow. Screens live in src/features/auth/, routes in
app/(auth)/ and are thin wrappers only.

1. Splash (screen 1)
Navy #101B4C full-bleed, faint radial glow behind the mark, white
logomark, "GuruFlow" 28/700, tagline 13 white 55%, 1px saffron progress
line at 40% width. Minimum display 1.9s, then resolve the session.

2. Onboarding (screen 2)
Three slides — attendance, fees, parent visibility. Skip top-right.
Art card 28 radius, dots, Continue. Persist a "seen" flag in
AsyncStorage so it shows once.

3. Login (screen 3)
Field 1 "Mobile number or email" (auto-detect: digits → phone, else
email). Field 2 password with show/hide. "Forgot password?" opens a reset
sheet. Primary "Sign in". Divider "or continue with". Google button.
Bottom card "TRY A DEMO ACCOUNT" with four pills in a 2×2 grid.
Demo pills are hidden in production builds (__DEV__ only).

4. OTP (screen 4)
Six boxes, auto-advance, backspace goes back, paste a 6-digit code fills
all. Resend timer 30s. "Change number". Verify button.
Phone path uses AuthPort.signInWithOtp → verifyOtp.

5. Role select (screen 5)
Four cards with tinted icon squares (owner #E8EEFF, teacher #FFF1E6,
student #E4F7F4, parent #FEECEC). Selected: 2px navy border + filled navy
check circle. Continue routes to register (owner) or join-by-code (others).

BEHAVIOUR
· Every submit button shows a spinner and disables while pending
· Inline field errors under each input, never a single alert
· Network failure shows a toast and keeps the typed values
· All strings from src/lib/copy.ts
· All auth through AuthPort — no SDK import in these files

ACCEPTANCE
· Wrong password → inline error, form preserved
· OTP wrong code → inline error, boxes clear
· Killing the app mid-OTP returns to login, not a broken state

DO NOT
· Do not build institute search or onboarding in this prompt
· Do not store tokens in AsyncStorage — use expo-secure-store
```

---

#### P5 — Register institute, join by code, approvals

> **Design intent —** onboarding must feel like three taps, not a form. Never more than five fields on one screen; show progress.

```
Build src/features/onboarding/ and src/features/approvals/.

1. Register institute (6)
Logo uploader 88pt dashed ring → StoragePort.upload('avatars'). Fields:
institute name, type segmented control (Coaching/School/Tuition), city,
board dropdown (CBSE/ICSE/State/Other), your name. On submit, one
transaction through the repos: create org → create membership(owner,
active) → create 3 empty fee terms → generate code from name+city
initials, e.g. SUNRISE-PAT, retry with a suffix if taken. Then show the
code in a navy card with a copy button and the saffron line explaining
that students use it to find the institute.

2. Join by code (7)
Search input accepting a name or an exact 11-char code. Debounce 300ms.
Results: logo, name, "Coaching · Patna, Bihar · 240 students", verified
pill, Join button. Joining creates a membership with status=pending.
Show a confirmation sheet before submitting.

3. Request pending (8)
The only screen a pending member may reach. Institute card, requested
timestamp, amber "Waiting for approval" row, cancel request.
Subscribe through RealtimePort so approval flips this screen live without
a refresh — the user should land on their role home within a second.

4. Approvals queue (14) — owner only
Header with count, "Approve all" summary card, request rows with avatar,
name, "Requested Class 10-A · via code · 2 hours ago", Reject/Approve
buttons. Show the amber duplicate warning when the phone matches an
existing student. Empty state when the queue is clear.

BEHAVIOUR
· Approve/reject optimistic, rollback on failure, toast either way
· Pending users never see a tab bar — the route guard in P6 enforces it
· No owner may approve their own membership request

ACCEPTANCE
· Second account joins, owner approves, second device moves to home
· Approving the same request twice does not create two memberships
· Cancel request removes the pending membership

DO NOT
· Do not build role switching yet — that is P6
```

---

#### P6 — Session, role routing, tab bars, org switcher

> **Design intent —** the shell must feel empty and quiet. Placeholders that admit they are placeholders, never fake features.

```
Build the app shell. No feature content.

1. src/store/session.ts (Zustand + persist)
State: user, memberships[], activeOrgId, activeRole, hydrated.
Actions: setSession, setMemberships, switchOrg(orgId), signOut.
Derived selectors: activeMembership, orgsAvailable.

2. Route groups, each with its own _layout.tsx
app/(auth) app/(pending) app/(owner) app/(teacher) app/(student)
app/(parent) app/(dev)
Each role group renders its TabBar from config. Nothing else.

3. src/lib/nav.ts — config-driven tabs. Adding a tab must require only a
config entry plus a route file.
owner:   Home, Students, Batches, Fees, More
teacher: Home, Classes, Homework, Exams, More
student: Home, Attendance, Homework, Results, More
parent:  Home, Attendance, Fees, Notices, More
Each item: { key, label, icon, iconActive, route }.
TabBar reads this array; never hardcode tabs in JSX.

4. Root _layout resolution
no session → (auth)
session + all memberships pending → (pending)
session + ≥1 active → the group of the active membership's role
If a user holds two roles in two orgs, switching org also switches role
group — handle that without a full remount flash.

5. OrgSwitcher bottom sheet (screens 50, 60)
Lists active memberships: logo, name, role, batch/class line, active
check. "Join another institute" dashed row at the bottom. Footer line:
"Your attendance and marks stay separate for each institute."
Trigger: the pill in the student/parent header; a row in More for
owner/teacher when they hold 2+ memberships.

6. Placeholder home screens
One minimal screen per role so navigation is testable: greeting, org name,
a card saying "Built in later prompts". Delete as real screens land.

ACCEPTANCE
· Every role reaches its own tabs and cannot reach another role's routes
· Pending user is locked to the pending screen
· Switching org updates every screen's data without a reload
· Back button never lands on another role's screen

DO NOT
· Do not build any real feature content
· Do not put queries in the layouts
```

---

### PHASE 2 — INSTITUTE CORE

#### P7 — Students

> **Design intent —** density with air. A list of 240 students must feel scannable at a glance, never cramped.

```
Build src/features/students/ for the owner role. Everything through
useStudents/useStudent/useCreateStudent hooks — no SDK imports.

1. Students list (10)
Sticky header: 34/800 "Students", circular search and a navy "+" on the
right. Horizontally scrolling filter chips: All (navy selected), one per
batch, Overdue. Sub-row: "240 students · 18 with dues" + sort menu
(name, roll, dues, attendance). Rows: 48pt avatar, 17/700 name,
"Class 10-A · Roll 14 · ₹1,500 monthly", right status pill (teal Paid /
red ₹1,500 due). Amber dot beside the name when absent today.
Infinite scroll 20/page with a footer spinner. Saffron FAB bottom-right.
Pull to refresh.

2. Add student (11)
Photo capture (camera or library) → StoragePort. Voice-to-name: a mic
button that starts speech recognition and fills the name field; if
unavailable, fall back to a plain input with a hint. Progress ring showing
completed fields of 5. Fields: full name, batch chips, roll number,
parent mobile, monthly fee (prefilled from the batch). Sticky
"Save student" and a "Skip photo" link. "Save & next" keeps the sheet
open, clears name/photo/roll, keeps the batch.

3. Student profile (12)
Navy header with photo, name, "Class 10-A · Roll 14 · Joined Jun 2025",
three chips (attendance, average, paid). Four tabs: Overview (attendance
ring, recent marks with bars, fee status card), Attendance (month list +
percentage), Marks (exam list with scores), Fees (terms with status).
"Message parent" opens WhatsApp via wa.me.

4. Self-enrol QR (13)
Bottom sheet over the dimmed list: QR of the join deep link, the institute
code beneath, "Share link" and "Print QR" buttons, explanatory line.

VALIDATION & STATES
· Name required, min 2 chars; phone exactly 10 digits; roll unique in batch
· Skeleton list on load, empty state with "Add your first student",
  error state with retry
· Delete = confirm dialog → optimistic remove → undo toast for 5s

ACCEPTANCE
· Adding a student appears in the list immediately and survives a reload
· Search and each filter chip return correct results
· A student created without a photo shows initials, not a broken image

DO NOT
· Do not build fee recording or marks entry here
```

---

#### P8 — Batches and timetable

> **Design intent —** batches are the skeleton of the product. Each card must answer who, when and how many in one look.

```
Build src/features/batches/ and src/features/timetable/.

1. Batches list (15)
Two-column grid of cards: 44pt tinted subject square, 17/700 batch name,
13 secondary "Mathematics · Mon Wed Fri", thin attendance bar with a
tabular percentage, 13 muted student count, amber dues pill when unpaid
fees exist. "Archived" section as one muted row at the bottom. Navy "+"
in the header, not a FAB.

2. Create/edit batch (16)
Name, subject dropdown, teacher picker (sheet listing memberships where
role=teacher, with avatars), day chips M T W T F S S, start and end time
pickers, fee plan segmented control (Monthly/Quarterly/Term) with an
amount input, capacity. Helper line: "Students will see this in their Fees
tab." Validation: end time after start time, at least one day, amount ≥ 0.

3. Batch detail (17)
Navy header: "Class 10-A", "Mathematics · Batch A", schedule line,
teacher name, three white chips (students, attendance %, pending fees).
Tabs Students / Attendance / Content. Students tab: three soft action
buttons (Take attendance navy, Assign homework saffron, Share material
teal), then the roster with per-student attendance % in teal or red.
Sticky "Take attendance".

4. Timetable (21)
Week strip with the selected day filled navy. Period rows 64pt: time chip,
subject, teacher, room, drag handle. Drag to reorder with optimistic save.
Conflict detection: if the same teacher is booked in two batches whose
times overlap on that day, show the amber warning card naming the clash,
and block save until resolved or dismissed.

ACCEPTANCE
· Creating a batch makes it appear on the batch list and in the student
  form's batch chips
· Reordering periods persists after reload
· A deliberate teacher clash shows the warning; removing it clears it

DO NOT
· Do not build attendance marking here — P9 owns it
```

---

#### P9 — Attendance (the hero screen)

> **Design intent —** speed is the design. Nothing may stand between the teacher and a saved sheet — no modals, no confirmation dialogs.

```
Build src/features/attendance/. This screen decides whether the product
feels fast. Target: mark a class of 42 in under 60 seconds.

1. Take attendance (22)
Compact header: back, "Class 10-A · Mathematics", date chip. Summary
strip: live counts "46 Present · 1 Late · 3 Absent" plus "Mark all
present". Three-column grid of student tiles, 10pt gaps, 108pt tall:
photo, name (one line, ellipsis), roll number, 4pt status strip at the
bottom, badge on the avatar (check / cross / clock). Absent tiles show
the photo at 60% opacity and a red strip. Late tiles amber. One tile may
carry a subtle navy ring to indicate the just-tapped state.
Tap cycles present → absent → late. Long-press opens a small action menu
(call parent, mark all after this). Haptic light impact on each tap.
Sticky footer: "Save attendance · 46/50 present" with the count in
saffron, disabled when nothing changed.

2. Existing sheet
If attendance already exists for that date, load it in edit mode, show a
"Editing saved sheet" pill, and change the button to "Update attendance".

3. Save
One batched upsert via AttendanceRepo.saveSheet. Optimistic: update local
cache, close to the batch, toast "Attendance saved". On failure: roll
back, keep the sheet open, offer retry.
After a successful save, create notification rows for every absent
student's parent, gated by an org setting (attendance alerts on/off).

4. Register (23)
Month selector, legend, calendar heatmap of the batch (7-column grid of
rounded squares, teal tints, amber and red exceptions, today ringed navy),
summary bars for present/late/absent, "Download monthly sheet" → CSV.

5. Offline
Detect connectivity. If offline, save to an AsyncStorage queue, show a
calm banner, and flush on reconnect. Display "Last synced X min ago".

ACCEPTANCE
· 42 students marked and saved in under 60 seconds by a human
· Reopening the same date shows the saved states, not a fresh sheet
· Airplane mode save queues, and syncs automatically on reconnect
· Absent parents receive a notification row after save

DO NOT
· Do not add biometric, QR or face attendance
· Do not add a modal in the main marking path
```

---

#### P10 — Exams, marks, analysis

> **Design intent —** a spreadsheet that feels like an app. Numbers are the loudest thing on the screen; chrome is nearly invisible.

```
Build src/features/exams/.

1. Exams list (24)
Segmented All / Upcoming / Completed. Cards: subject tint square, 17/700
exam name, "Mathematics · Class 10-A · 42 students", two pills (date navy,
marks-entered state teal or amber), chevron. When partly entered, show a
thin saffron progress bar with "18 of 42 entered".

2. Create exam (25)
Name, subject, batch picker, date, max marks (large tabular input), and an
"Enter marks" choice: Now (opens marks entry) or Later, remind me (creates
a reminder). Info card: "Students and parents will be notified when
results are published."

3. Marks entry (26)
Sticky two-column header STUDENT / MARKS. Rows 56pt: avatar, name, and a
72×44 numeric input, right-aligned tabular. Active row: white box with a
2px navy ring. KeyboardAvoidingView + auto-scroll so the focused row is
never hidden. Progress bar "18 of 42 entered". Validation: numeric only,
0 ≤ marks ≤ max. Save all in one batched upsert. Show a warning before
leaving with unsaved rows.

4. Result analysis (27)
Hero: class average as a big tabular number, delta vs the previous exam,
and a six-bar mini chart with the tallest bar in saffron, no gridlines.
Three stat cards: highest, lowest, pass rate. Top 5 with rank badges
(1st filled saffron). Subject-wise horizontal bars. Export CSV.

DATA RULE
Every number must come from the seeded marks in AGENTS.md so that the
exam list, marks entry, analysis and the student's results screen all
agree. Add a dev-only assertion that logs a warning when the top-5 totals
do not match the marks rows.

ACCEPTANCE
· Entering marks updates the analysis immediately after save
· Marks above max are rejected with an inline error
· Export CSV opens the share sheet with correct Indian-formatted values

DO NOT
· Do not add grading schemes, weightage or report-card templates
```

---

#### P11 — Fees

> **Design intent —** money must look trustworthy. Large tabular figures, calm colour, and red only for true overdue — never for decoration.

```
Build src/features/fees/.

1. Dashboard (28)
Navy hero: "Collected this term" label, large tabular amount, "of ₹X
expected", 6pt progress bar with the saffron fill. Two overlapping white
mini cards beneath: pending (amber) and overdue (red). "By batch" rows
with a thin bar and right-aligned amount. "Recent payments" rows with
avatar, name, "Term 2 · UPI · 12 Nov", teal +amount, receipt icon.
Saffron FAB with a ₹ icon.

2. Fee plan setup (29)
Segmented Monthly / Quarterly / Term. Instalment cards: step badge, name,
due date, amount input, calendar icon. "+ Add instalment" dashed row.
"Applies to" batch chips. Helper: "Students see this instantly in their
Fees tab."

3. Record payment (30)
Student card with a red due pill. Amount input prefilled with the due
amount. Method chips UPI / Cash / Card. UTR reference (required for UPI,
hidden for cash). Date. Live receipt preview card with a dashed top edge.
Confirm button showing the amount. After save, offer "Send receipt to
parent on WhatsApp".

4. Due list (31)
Navy strip: overdue count and total, "Remind all". Segmented
Overdue / Pending / Cleared. Rows: avatar, name, "Class 10-A · Term 2 ·
42 days overdue", red amount, Remind button. Bulk remind opens wa.me links
sequentially with a progress toast.

5. Receipt (64)
Success tick, dashed receipt card (institute name, RECEIPT label, big
amount, then Student / Term / Receipt no / Date / Method), Download PDF
via expo-print, Share on WhatsApp, "Back to fees".

MONEY RULES
· All amounts are integers in paise internally; format with inr() for display
· Receipt numbers are sequential per org: SCB/26-27/NNNN
· FeeRepo.totals() must return collected + pending + overdue = expected.
  Add a dev assertion that throws in __DEV__ when they do not reconcile.
· Never compute totals in a screen from raw rows — always use totals()

ACCEPTANCE
· Recording a payment updates the dashboard, the due list and the student
  profile fee tab together
· A rejected payment returns the amount to pending
· Receipt PDF renders with the correct institute name and city

DO NOT
· Do not integrate a payment gateway in this prompt — PaymentPort is
  wired in P17
```

---

#### P12 — Content library and upload

> **Design intent —** a library, not a file manager. Content should feel watchable, with thumbnails doing the work.

```
Build src/features/content/.

1. Library (32)
Storage card: "18.4 GB of 50 GB used", thin progress bar, "Upgrade" link.
Horizontally scrolling folder cards (140×88): tinted folder icon, name,
"24 files". Two-column grid of content cards 160pt tall: 16:9 thumbnail
with a centred play circle, title on two lines, "Class 10-A · 12 Nov",
type pill (VIDEO navy tint, PDF red tint). Search and a navy "+" in the
header. Long-press a card for delete/rename.

2. Upload (33)
Large dashed drop zone with "Choose a file", supported sizes, Browse
button. Divider "or". "Paste a link" card with a URL input and the teal
helper "Links play inside the app — no ads, no distractions." Settings
card: title, visibility batch chips (multi-select), publish Now or
Schedule (date picker). Upload progress with a cancel action.

3. Storage layer
All uploads go through StoragePort and VideoPort — never a vendor SDK in
the screen. Adapter behaviour: PDFs and images to R2 via a signed URL from
an edge function; video to Bunny Stream via TUS with a progress callback.
Persist only url/key/thumbnail in the content row.

4. Limits
Read the org's plan allowance. When used ≥ allowance, block the upload
and show an upgrade sheet with the plan comparison. Increment the counter
after a successful upload, not before.

ACCEPTANCE
· Uploading a PDF appears in the grid with a correct thumbnail/icon
· A pasted YouTube link opens in the in-app player, not an external browser
· Cancelling an upload removes the partial record
· Exceeding the plan limit blocks upload with a clear message

DO NOT
· Do not build the player here — P16 owns it
· Do not generate thumbnails on device
```

---

### PHASE 3 — TEACHER

#### P13 — Teacher home, classes, leave, profile

> **Design intent —** the teacher's morning. One screen must answer what do I do next, in under three seconds.

```
Build src/features/teacher/.

1. Teacher home (41)
Navy hero: date, "Good morning, Priya", "3 classes · 2 homework due",
avatar. Overlapping white card with two columns: classes today, submissions
to grade. "Today's schedule" rows 72pt: navy time chip, subject and batch,
room and student count, right "Attendance" button that deep-links to P9
with the batch prefilled. "Needs attention" card with a saffron left
accent when submissions are pending. Four quick actions (Take attendance,
Assign homework, Enter marks, Share material).

2. My classes (42)
Cards: subject tint, batch name, schedule, room, attendance bar with
percentage, two outlined buttons Attendance and Homework, amber
"12 submissions pending" pill when applicable. Header search.

3. Apply for leave (47)
Leave type chips (Casual / Sick / Personal), from and to date fields with
a computed "2 days" line, reason textarea, "Affected classes" list derived
from the timetable for those dates with amber "Substitute needed" pills.
Submit → status card. History list of previous requests with status.

4. Teacher profile (48)
Navy header with photo, name, subject, employee ID, three chips
(batches, submission rate, rating). List rows: personal details, bank and
salary, my batches, notification preferences, change password. "This
month" card with three tabular stats. Sign out.

SCOPING
A teacher sees only batches where teacher_membership_id equals their own
membership id. Enforce in the repo query, not only in the UI. Attempting
another teacher's batch id must return not_found.

ACCEPTANCE
· A second teacher account sees a different class list
· Leave request appears in the owner's approvals queue
· Attendance deep-link opens the correct batch and date

DO NOT
· Do not build payroll or salary calculations
```

---

#### P14 — Homework

> **Design intent —** make homework feel finished rather than pending. Progress must be readable without reading a single label.

```
Build src/features/homework/.

1. Assign homework (43)
Batch picker (teacher's own batches only), subject, title, description,
due date and time, attach a file or paste a link, "notify students"
toggle. Footer: "Send to 42 students" with the live count.
On send, create the homework row and one notification per student and
parent, gated by the org's homework-alert setting.

2. Tracker (44)
Segmented Active / Closed. Cards: title, "Class 10-A · Due today, 8:00 PM",
a two-tone progress bar (submitted teal, pending amber) with the counts on
each side, percentage on the right. Two text buttons: Remind pending,
View submissions. Red "Overdue" pill when past due. Header filter.

3. Grade submissions (45)
Header progress "28 of 42 submitted". Rows 80pt: avatar, name,
"Submitted 11 Nov, 9:14 PM", grade input (e.g. 8/10) on the right.
Expanding a row reveals the attachment thumbnail and Approve / Ask again
buttons. "Save 28 grades" in a sticky footer. Validation: grade ≤ max.

4. Share material (46)
Same uploader as content, scoped to a single batch, with a notify toggle.

DATA RULES
submitted + pending must equal the batch student count. Add a dev
assertion that logs a warning when it does not, and use the seeded counts
from AGENTS.md so every screen agrees.

ACCEPTANCE
· Assigning homework appears instantly in the student's homework list
· Grading a submission updates the tracker percentage
· Reminding pending students creates notification rows only for those who
  have not submitted

DO NOT
· Do not build plagiarism checks, rubrics or auto-grading
```

---

### PHASE 4 — STUDENT & PARENT

#### P15 — Student home and records

> **Design intent —** a student's home should feel encouraging, never like a dashboard of failures. Lead with what is done, not what is missing.

```
Build src/features/student/.

1. Home (49)
Compact navy header: avatar, "Class 10-A", "Hello, Aarav", bell with an
unread dot. Institute switcher pill under the name when the student holds
more than one membership. Overlapping white card: attendance ring and
"Next test in 3 days". "Due today" task cards with a subject tint, title,
"Mathematics · Due 8:00 PM", and either Submit or a teal Submitted pill.
"Today's classes" rows with time chips and a join action.
"New material" horizontal thumbnails.

2. My institutes switcher (50)
Bottom sheet: both institutes with logos, class line, active check, and a
fee-due line on the inactive one. Dashed "Join another institute" row.
Footer: "Your attendance and marks stay separate for each institute."

3. My attendance (51)
Large teal ring with "46 of 50 days" centred. Present / Absent / Late rows
with dots. Month heatmap. Subject-wise bars. Download report.

4. Homework list (52)
Segmented Due / Submitted / Overdue. Cards: subject square, title,
"Mathematics · Priya Nair", due chip, Submit button or Submitted pill.
Red left accent and "Overdue by 1 day" for late items.

5. Homework detail + submit (53)
Instructions, attachment row with download, "Your submission" card with a
photo upload zone and "or attach a file". Submit → optimistic update with
an undo window of 10 seconds.

6. Exam results (56)
Navy hero: exam name, big score, "Class average 78% · Rank 4 of 42",
Report card button. Trend line chart across five tests. Subject bars.
Exam history rows.

SCOPING
A student may read only their own rows. Enforce in the adapter query AND
rely on RLS. Add a dev test that requests another student's marks and
expects zero rows.

ACCEPTANCE
· Switching institute in the sheet swaps all data on this screen
· Submitting homework reflects on the teacher's tracker
· Marks shown match the teacher's marks entry exactly

DO NOT
· Do not build chat, doubts or comments
```

---

#### P16 — Study material and video player

> **Design intent —** the player is the product. Remove everything that is not the lesson — the emptiness is the feature.

```
Build src/features/player/.

1. Study material (54)
Subject chips. Two-column video grid: thumbnail with a play circle, title,
"24 min · Chapter 6", VIDEO pill. Notes & PDFs list: red-tinted icon,
filename, "2.4 MB · 12 Nov", download icon. Offline download toggle per
item with progress and a downloaded tick.

2. Video player (55)
Full-bleed dark screen #0B1220. Video occupies the top 56% with a 16:9
frame, thin 2pt saffron progress line, back chevron and a class pill
overlaid. Below on the dark surface: title 22/800 white, "Mathematics ·
Priya Nair · 24 min" in white 60%, three circular outline buttons
(bookmark, notes, download). Chapter list: completed chapters get a teal
tick, the current chapter a saffron left bar, durations right-aligned in
white 50%. "Up next" card in #16213C.
No ads, no comments, no recommended videos. The emptiness is the product.

3. Playback
expo-video with the HLS URL plus a signed token from VideoPort. Remember
watch position per user per video and resume on open. Chapters come from
the content record, never hardcoded. Handle offline-downloaded files by
preferring the local URI.

ACCEPTANCE
· Leaving mid-video and returning resumes at the saved position
· A downloaded video plays in airplane mode
· Chapter tap seeks to that timestamp and marks the previous ones complete

DO NOT
· Do not add comments, ratings or social features
· Do not build live classes
```

---

#### P17 — Parent

> **Design intent —** reassurance. A parent should understand their child's whole month in ten seconds without tapping anything.

```
Build src/features/parent/.

1. Parent home (59)
Greeting, avatar, child switcher card with a "2 children" pill. Three
stats in one card split by hairlines: attendance, average, dues. Performance
line chart. Recent updates rows (homework, an amber attendance alert, a
notice). Soft saffron fee-due card with Pay now. Tab bar with Fees.

2. Child switcher (60)
Bottom sheet: both children with photos, class, three mini stats each,
active check, amber due pill on the inactive child.

3. Child attendance (61)
Ring, present/absent/late rows, month heatmap, absences list with subject
and a status pill, "Message the teacher".

4. Child performance (62)
Average with delta, trend chart, three stat cards (rank, best subject,
needs help), subject bars with the weakest in amber, recent tests,
download report card.

5. Fees and pay (63)
Navy hero with amount due and a saffron Pay now. Term breakdown with
paid/due pills and receipt icons on paid rows. Payment history with
download icons. Trust card: "Payments are verified by the institute."

6. Receipt (64)
Success tick, dashed receipt card, Download PDF, Share on WhatsApp.

7. Apply for leave (65)
Child context strip, leave type chips, dates with a computed day count,
reason, affected classes with an amber count pill, submit.

PAYMENT
"Pay now" calls PaymentPort.createIntent(). The Supabase adapter returns a
Razorpay payment-link URL and, on return, creates a fee_payments row with
status=pending and the gateway reference. The institute verifies it in P11.
No card details ever touch this app. Define everything behind PaymentPort
so switching to PhonePe or Stripe later is an adapter change only.

ACCEPTANCE
· A parent sees only their own children
· Switching child updates every card without a reload
· A payment attempt creates exactly one pending row, never two on retry

DO NOT
· Do not store or transmit card numbers
· Do not build a chat feature
```

---

### PHASE 5 — CROSS-CUTTING

#### P18 — Notices and notifications

> **Design intent —** notices feel official but light; notifications must be scannable in three seconds while walking.

```
Build src/features/notices/ and src/features/notifications/.

1. Notice composer (34) — owner and principal only
Title input, body textarea with a 500-char counter, "Send to" toggles
(Students, Parents, Teachers, All batches), an amber card explaining that
parents also get a WhatsApp summary, a live preview strip, and Publish.
On publish: create the notice and one notification per recipient in the
selected audience, honouring the org's notice-alert setting.

2. Notices list (57) — every role
Segmented All / Academic / Events. Cards: category icon square, title,
body on three lines, footer with a category pill, date, and "Read more".
Event cards show a date chip. Owner sees a delete action on long-press.

3. Notification centre (35)
Segmented All / Unread / Announcements. Cards with a tinted icon per type
(announcement, homework, fee, exam, leave, attendance), title, body,
relative time, unread dot and a soft navy tint for unread. "Mark all read".
Swipe to dismiss with undo.

4. Badge
Unread count on the More tab, updated through RealtimePort so it changes
without a refresh. Cap the display at "9+".

ACCEPTANCE
· Publishing a notice increments every recipient's badge within a second
· Marking all read clears the badge everywhere
· A notice for "Students only" never notifies parents

DO NOT
· Do not add push notifications in this prompt — that is an adapter-level
  change later and must not touch these screens
```

---

#### P19 — Reports

> **Design intent —** reports are for a principal with forty seconds. One headline number, one chart, one table. Nothing else.

```
Build src/features/reports/.

1. Hub (36)
Six cards in a two-column grid, each with a tinted icon square, title, a
one-line description and a chevron: Student strength, Fee collection,
Attendance, Academic performance, Teacher performance, Custom report.
Footer: "Schedule monthly email".

2. Detail (37)
Filter row: period chip, batch chip, "Change". Hero card with a total, a
delta, and a clean bar chart (no gridlines, no shadows, tallest bar
saffron). Data table with a sticky header, hairline rows, tabular
right-aligned amounts, and small teal/red status dots. "Export as CSV".

3. Data
Aggregation lives in the repos, not the screen. Add report methods to the
relevant ports (getReport(orgId, type, params)) and implement them in the
adapter. The screen only renders.

4. Export
CSV via expo-file-system + expo-sharing, with a header row, Indian number
formatting and the period in the filename.

ACCEPTANCE
· The fee report total matches the fees dashboard total for the same period
· Exporting produces a CSV that opens correctly in Excel and Google Sheets
· Changing a filter refetches without a full screen flash

DO NOT
· Do not build PDF report cards here
· Do not add charts libraries beyond what is already installed
```

---

#### P20 — Settings, billing, backup

> **Design intent —** settings should feel boring and safe. Billing should feel like a real subscription, not a mock-up.

```
Build src/features/settings/.

1. Settings (38)
Institute profile card with logo, name, city, code and an Edit button.
Grouped list with icon rows and hairlines: institute profile, academic
year, branding colour, notification preferences, staff & permissions,
backup & data. Second group: subscription (with an Active pill), help &
support, about. Danger row: sign out.

2. Subscription (39)
Navy plan card: plan name, price and renewal date, Upgrade button. Usage
card: "240 of 300 students used" with a bar and an amber "60 left" helper.
Payment method: UPI row with autopay state and a Change link. Invoice list
with dates, amounts and download icons. Cancel plan at the bottom.

3. Backup and data (40)
Teal backup-status card with the last backup time. Four export rows:
students CSV, attendance CSV, fee records CSV, full JSON backup. Danger
zone: "Clear all data" with a typed confirmation (type the institute name
to enable the button). Help rows: watch videos, WhatsApp support, call us.

4. Plan enforcement
Add a guard in StudentRepo.create: when the org's active student count has
reached the plan cap, throw AppError('conflict','plan_limit'). The student
form catches it and opens the upgrade sheet. Never enforce only in the UI.

ACCEPTANCE
· Adding the 301st student is blocked with a clear upgrade path
· Exporting the full JSON produces a file that can be re-imported later
· Clearing data requires typing the institute name

DO NOT
· Do not implement real billing or card capture
```

---

#### P21 — Search, system states, polish

> **Design intent —** the states nobody designs. Empty and error screens are where premium products prove themselves — stay calm, never alarming.

```
Build the finishing layer. Nothing new should appear — only states and
polish.

1. Global search (66)
Search field with a clear button and Cancel. Results grouped by Students,
Batches, Content, with matched characters highlighted and recent searches
as chips when the query is empty. Debounce 250ms. Empty and no-results
states are distinct.

2. Empty states (67)
One EmptyState component used in all four variants: soft tinted circular
illustration, 20/800 headline, two-line 13 secondary text, one navy
button. Apply to: no students, no homework due, no payments, nothing to
approve. Audit every list screen and replace ad-hoc empties with it.

3. Offline and error (68)
Connectivity banner that appears calmly (never red, never a modal).
Retry queue for attendance and homework submissions. "Last synced X min
ago". Error boundary rendering the "This is on us, not on you" state with
Try again and Contact support. Skeleton shimmer everywhere data loads, with
no layout shift.

4. Polish pass
Haptics on attendance taps and payment success. Press states on every
tappable card. Keyboard dismissal on scroll. Accessibility labels on
icon-only buttons. Then audit every screen against the AGENTS.md tokens
and list every deviation you fixed.

ACCEPTANCE
· Airplane mode on every major screen shows a calm state, not a crash
· No screen shows a bare spinner where a skeleton is expected
· The accessibility audit lists labels for all icon-only buttons

DO NOT
· Do not add features, screens or navigation entries in this prompt
```

---

### PHASE 6 — HARDENING

#### P22 — Security and tenancy audit

> **Design intent —** none — audit only.

```
Audit the app and report findings as a table with columns: check, result,
action taken.

1. RLS: every table has RLS enabled and at least one policy per operation
   it needs. List any table missing one.
2. Tenancy: as a student in org A, attempt to read org B's students,
   attendance, marks, fees and content. Every attempt must return zero
   rows. Show the exact queries and their row counts.
3. Pending membership: confirm such a user can read nothing at all.
4. Role escalation: confirm a teacher cannot change a role, edit fee
   structures, manage memberships, or update another org's rows. Confirm
   a user cannot update their own membership role.
5. Client secrets: confirm no service-role key exists in the bundle and
   that only the anon key is used. Search the built output, not the source.
6. Layer purity: run `grep -rn "supabase" src/features app/` and show the
   output. Only src/adapters/ may match. Fix any leak by moving the call
   into the adapter and calling it through a repo hook.
7. Storage: avatars public-read; everything else private with signed URLs
   and a short TTL.
8. Auth: confirm sessions live in expo-secure-store, never AsyncStorage.

Fix every failure, then re-run all checks and show them passing.

DO NOT
· Do not restructure anything else in this prompt
```

---

#### P23 — Performance and deploy

> **Design intent —** none — performance and release only.

```
1. Indexes — add a migration with: attendance(student_id,date),
   attendance(batch_id,date), marks(exam_id), fee_payments(student_id),
   memberships(user_id,org_id), notifications(user_id,is_read),
   students(batch_id), content(org_id).
2. Replace any client-side aggregation over more than 1000 rows with a
   Postgres view or RPC, exposed through a repo method. List what you moved.
3. Measure and report three numbers: cold start, students list first
   render, attendance save round-trip. State the device or emulator used.
4. Web deploy: produce a build for Vercel, document the env vars in
   .env.example, and write the deploy steps in docs/DEPLOY.md.
5. Native: create eas.json with development, preview and production
   profiles. Write the exact commands for an Android APK, an Android AAB
   for Play Store, and an iOS build.
6. Store readiness: write the Play Store listing (title, short and long
   description), the data-safety answers, and the children's-data
   declaration. Note that under India's DPDP Act, a user under 18 needs
   verifiable parental consent — add the consent checkbox to the student
   self-enrol flow if it is missing.

ACCEPTANCE
· The three measurements are recorded in docs/PERF.md
· A production web build completes without warnings
· eas.json is valid and documented

DO NOT
· Do not add new features in this prompt
```

---

#### P24 — Extensibility pass

> **Design intent —** none — restructure and document only.

```
Make the codebase cheap to extend. Report every change as a table.

1. Split any file over 300 lines. Report the before/after list.
2. Move any business logic found inside a screen into a feature hook.
   List each move.
3. Confirm every feature folder exposes an index.ts and that nothing
   outside a feature imports its internals directly. Fix violations.
4. Move every user-visible string into src/lib/copy.ts. Report the count.
5. Confirm navigation is fully config-driven in src/lib/nav.ts. Adding a
   tab must require only a config entry plus a route file — prove it by
   temporarily adding a disabled "Library" tab and removing it.
6. Write docs/ADDING_A_FEATURE.md: the six-step recipe — contract method,
   adapter implementation, repo hook, feature folder, route file, nav
   entry — with a worked example.
7. Write docs/ADDING_A_BACKEND.md: the ten-minute swap checklist,
   including copying the supabase adapter, keeping signatures identical,
   editing providers.ts, and running /dev/contract-check.
8. Write docs/DATA_MODEL.md: every table, its org scoping and its RLS rule
   in one line each.
9. Write docs/ARCHITECTURE.md: a short diagram of UI → hooks → contracts →
   adapter → database, and the rule that the UI never imports a vendor SDK.
10. Create CHANGELOG.md with an entry for this build.

ACCEPTANCE
· No file over 300 lines
· `grep -rn "supabase" src/features app/` returns nothing
· The three docs exist and are accurate enough for a new developer to
  follow without asking me anything

DO NOT
· Do not add features or change behaviour
```

---

## 7. Build order

```
PHASE 0   P1  P2  P3          ← foundation, do not rush
PHASE 1   P4  P5  P6          ← you can log in and switch roles
PHASE 2   P7  P8  P9  P10  P11  P12
PHASE 3   P13 P14
PHASE 4   P15 P16 P17
PHASE 5   P18 P19 P20 P21
PHASE 6   P22 P23 P24
        │
        ▼
   Ship after P17 if you want a demo. Do P18–P21 before a real pilot.
   Never skip P22.
```

---

## 8. Adding a backend later (the 10-minute test)

```
1. cp -r src/adapters/supabase src/adapters/yourbackend
2. Rewrite each *.repo.ts internals. Keep every signature identical.
3. Add env keys → .env and src/core/config/env.ts
4. Edit ONE line in src/core/config/providers.ts
5. Open /dev/contract-check → every method must pass
        │
        ▼
   If step 5 is green, the whole app runs on the new backend.
   No screen, hook, or contract file changes.
```

If a prompt ever makes the UI import a backend SDK, that is a bug. Tell the tool: *"The UI must not import the SDK. Move this into the adapter and call it through the repo hook."*

---

## 9. Correction phrases that actually work

```
"Screen 22 does not match. The status strip is missing and the tiles are
2 columns instead of 3. Fix only those two things."

"AttendanceRepo.list returns database rows. It must return Student[] from
src/core/contracts/models.ts. Fix the mapping, nothing else."

"Use only tokens from src/lib/theme.ts. You used #1E40AF which is not in
the palette. Replace every occurrence."

"The UI imported supabase directly. Move that call into the adapter and
reach it through the repo hook."

"This file is 412 lines. Split it by responsibility and tell me the new
file list."
```

Two more, for when the design drifts:

```
"Screen 11 is generic. It has three equal cards in a row and a gradient
header — that is the default AI layout. Rebuild it using layout shape C
from section 2.2 and make the fee amount the focal point."

"You used saffron four times on this screen. Section 2.4 allows two.
Decide which two matter and neutralise the rest."

"This card has two accent colours and a shadow. Hairline only, one accent,
and re-check against section 2.6."
```

One issue per correction. Vague feedback produces vague fixes.

---

---

## 10. Design QA — how to judge a screen without a reference image

You have no mockups to compare against, so judge by rules instead. Run this after every prompt that produces a screen.

```
FOR EACH NEW SCREEN, TICK ALL SEVEN
[ ] Does it match one of the four layout shapes in §2.2?
[ ] Is there exactly one focal point, and can I name it in three words?
[ ] Are there at most two saffron elements?
[ ] Is every number tabular and plausible?
[ ] Is there exactly one primary action?
[ ] Does it have a skeleton, an empty state and an error state?
[ ] Would it look out of place next to Linear or Stripe? (must be "no")

ANY TICK MISSING → fix before the next prompt.
```

Red flags that mean "regenerate, don't patch":

```
· three identical cards in a row with equal visual weight
· a gradient hero on a sub-screen that only has a form
· an icon doing the job a number should do
· the same corner radius on every element and no variation in card weight
· a screen where you cannot tell what to tap first
· text smaller than 11pt anywhere
```

Keep a `docs/DESIGN-LOG.md`. After each screen, ask the AI to append one line:

```
P9 Attendance — shape D (focus) · focal point: the save button count ·
saffron: save button + current tile · decisions: tiles 108pt so 9 fit
without scrolling on a 6.1" screen
```

Six months from now that file is the only record of why the app looks the way it does — and it is what keeps screen 60 consistent with screen 6.

---

*Pack v2.0 · 24 prompts · every prompt under 10,000 characters · no reference images required · backend-swappable by design*