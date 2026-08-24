# Personal Life App — Users & User Stories

# 1. Purpose

This document defines the users of Personal Life App, their goals, problems, and the actions they should be able to perform within the application.

The purpose of this document is to ensure that product and technical decisions are driven by real user needs rather than simply adding features.

---

# 2. Primary Users

Personal Life App will initially focus on three primary user types.

---

## 2.1 The Busy Professional

### Description

A working adult who manages multiple responsibilities across work and personal life.

### Typical Responsibilities

- Work
- Bills
- Appointments
- Subscriptions
- Documents
- Vehicle maintenance
- Personal finances
- Household responsibilities

### Problems

- Has too many things to remember
- Uses multiple applications to manage life
- Sometimes forgets deadlines
- Has difficulty knowing what deserves attention
- Receives too many disconnected notifications

### Goals

- Know what needs to be done today
- Remember upcoming responsibilities
- Keep important information organized
- Reduce mental workload
- Avoid missing deadlines

---

## 2.2 The Household Manager

### Description

A person responsible for managing household or family responsibilities.

### Typical Responsibilities

- Household bills
- Grocery shopping
- Appointments
- Family schedules
- Important dates
- Household maintenance
- Shared responsibilities

### Problems

- Responsibilities are shared across different people
- Important tasks can be forgotten
- Household information is scattered
- Difficult to track recurring responsibilities

### Goals

- Organize household responsibilities
- Share tasks
- Track bills
- Remember important dates
- Keep family responsibilities organized

---

## 2.3 The Personal Organizer

### Description

A person who wants better control over their personal responsibilities and information.

### Typical Responsibilities

- Personal tasks
- Reminders
- Documents
- Subscriptions
- Bills
- Personal goals
- Important dates

### Problems

- Uses multiple apps
- Relies heavily on memory
- Has difficulty keeping information organized
- Wants a simple system instead of complex productivity tools

### Goals

- Keep important information in one place
- Remember important responsibilities
- Have a simple daily overview
- Stay organized without excessive effort

---

# 3. User Goals

The core user goals are:

1. Know what needs attention today.
2. Remember important deadlines.
3. Track recurring responsibilities.
4. Organize important personal information.
5. Avoid forgetting important tasks.
6. Understand upcoming financial responsibilities.
7. Track document expiration dates.
8. Reduce mental workload.
9. Quickly find important information.
10. Eventually allow the application to proactively help.

---

# 4. Core User Journey

The ideal user journey is:

```text
Download App
     ↓
Create Account
     ↓
Set Up Basic Information
     ↓
Create First Responsibility
     ↓
Set Reminder
     ↓
Open Dashboard
     ↓
See Upcoming Responsibilities
     ↓
Receive Reminder
     ↓
Complete Responsibility
     ↓
Return to Dashboard
```

The application should gradually become part of the user's daily routine.

---

# 5. User Stories

## 5.1 Authentication

### US-AUTH-001 — Create Account

**As a user,**

I want to create an account,

**so that**

my personal information and responsibilities can be securely associated with me.

#### Acceptance Criteria

- User can provide required registration information.
- User can create an account.
- Invalid registration information is rejected.
- User receives appropriate validation messages.
- User is authenticated after successful registration.

---

### US-AUTH-002 — Sign In

**As a user,**

I want to sign in to my account,

**so that**

I can access my personal information.

#### Acceptance Criteria

- User can enter credentials.
- Valid credentials allow access.
- Invalid credentials show an appropriate error.
- User remains authenticated between sessions.

---

### US-AUTH-003 — Sign Out

**As a user,**

I want to sign out,

**so that**

my account remains protected when I am not using the application.

---

# 6. Dashboard User Stories

### US-DASH-001 — View Today's Responsibilities

**As a user,**

I want to see the things I need to take care of today,

**so that**

I immediately know what requires my attention.

#### Acceptance Criteria

- Dashboard displays today's tasks.
- Dashboard displays relevant reminders.
- Completed items are clearly distinguished.
- Overdue items are clearly identified.
- Items are ordered by priority and/or time.

---

### US-DASH-002 — View Upcoming Responsibilities

**As a user,**

I want to see upcoming responsibilities,

**so that**

I can prepare for them in advance.

#### Acceptance Criteria

- Upcoming tasks are displayed.
- Upcoming bills are displayed.
- Upcoming document expirations are displayed.
- User can access the relevant item.

---

### US-DASH-003 — Identify Urgent Items

**As a user,**

I want the application to highlight urgent responsibilities,

**so that**

I know what should be handled first.

---

### US-DASH-004 — View Overdue Items

**As a user,**

I want to see overdue responsibilities,

**so that**

I can address things I have missed.

---

# 7. Task User Stories

### US-TASK-001 — Create Task

**As a user,**

I want to create a task,

**so that**

I don't have to rely on memory.

#### Acceptance Criteria

A task can contain:

- Title
- Description
- Due Date
- Due Time
- Priority
- Category
- Reminder

---

### US-TASK-002 — Edit Task

**As a user,**

I want to edit a task,

**so that**

I can correct or update its information.

---

### US-TASK-003 — Complete Task

**As a user,**

I want to mark a task as completed,

**so that**

I know the responsibility has been handled.

---

### US-TASK-004 — Delete Task

**As a user,**

I want to delete a task,

**so that**

I can remove responsibilities that are no longer relevant.

---

### US-TASK-005 — Create Recurring Task

**As a user,**

I want to create recurring tasks,

**so that**

I don't have to manually recreate the same responsibility repeatedly.

#### Examples

- Pay rent every month
- Change air filter every 3 months
- Service motorcycle every 6 months
- Renew subscription every year

---

# 8. Reminder User Stories

### US-REM-001 — Create Reminder

**As a user,**

I want to create a reminder,

**so that**

the application can notify me before something important happens.

---

### US-REM-002 — Configure Reminder Timing

**As a user,**

I want to choose when I receive a reminder,

**so that**

the notification arrives at a useful time.

#### Examples

- At the time
- 10 minutes before
- 1 hour before
- 1 day before
- 1 week before
- Custom

---

### US-REM-003 — Recurring Reminder

**As a user,**

I want to create recurring reminders,

**so that**

repeating responsibilities are automatically remembered.

---

### US-REM-004 — Receive Notification

**As a user,**

I want to receive a notification when a reminder is triggered,

**so that**

I don't forget the responsibility.

---

# 9. Document User Stories

### US-DOC-001 — Add Document

**As a user,**

I want to add an important document,

**so that**

I can keep track of its information.

---

### US-DOC-002 — Track Expiration Date

**As a user,**

I want to record a document's expiration date,

**so that**

I know when I need to renew it.

---

### US-DOC-003 — Receive Expiration Reminder

**As a user,**

I want to receive reminders before a document expires,

**so that**

I have enough time to renew it.

---

### US-DOC-004 — View Documents

**As a user,**

I want to view my documents,

**so that**

I can quickly find important document information.

---

# 10. Bill User Stories

### US-BILL-001 — Add Bill

**As a user,**

I want to add a bill,

**so that**

I can track when it needs to be paid.

---

### US-BILL-002 — Record Bill Amount

**As a user,**

I want to record the bill amount,

**so that**

I know how much I need to pay.

---

### US-BILL-003 — Track Due Date

**As a user,**

I want to record a bill's due date,

**so that**

I don't miss the payment deadline.

---

### US-BILL-004 — Recurring Bill

**As a user,**

I want recurring bills to automatically repeat,

**so that**

I don't have to create them every month.

---

### US-BILL-005 — Mark Bill as Paid

**As a user,**

I want to mark a bill as paid,

**so that**

I know which bills have already been handled.

---

# 11. Category User Stories

### US-CAT-001 — Categorize Responsibilities

**As a user,**

I want to categorize my tasks and responsibilities,

**so that**

I can organize and find them more easily.

#### Example Categories

- Personal
- Home
- Finance
- Health
- Vehicle
- Work
- Family
- Documents
- Shopping
- Other

---

# 12. Search User Stories

### US-SEARCH-001 — Search Information

**As a user,**

I want to search my responsibilities,

**so that**

I can quickly find information.

---

# 13. Notification User Stories

### US-NOTIF-001 — Notification Preferences

**As a user,**

I want to control my notification preferences,

**so that**

I receive useful notifications without being overwhelmed.

---

### US-NOTIF-002 — Important Notification

**As a user,**

I want important responsibilities to be clearly highlighted,

**so that**

I don't miss critical deadlines.

---

# 14. Future Family User Stories

> These stories are intentionally outside the initial MVP.

### US-FAMILY-001 — Add Family Member

**As a user,**

I want to add a family member,

**so that**

I can manage shared responsibilities.

---

### US-FAMILY-002 — Shared Task

**As a user,**

I want to assign a task to another household member,

**so that**

we can share responsibilities.

---

### US-FAMILY-003 — Shared Bill

**As a user,**

I want to share a bill with a household member,

**so that**

we can coordinate payments.

---

# 15. Future AI User Stories

> These stories are intentionally outside the initial MVP.

### US-AI-001 — Natural Language Task

**As a user,**

I want to describe a task using normal language,

**so that**

I don't have to manually enter every field.

#### Example

```text
Remind me to renew my motorcycle insurance one month before it expires.
```

---

### US-AI-002 — Personal Weekly Summary

**As a user,**

I want the application to summarize my upcoming responsibilities,

**so that**

I can quickly understand what I need to handle.

---

### US-AI-003 — Smart Recommendations

**As a user,**

I want the application to identify important upcoming responsibilities,

**so that**

I can act before they become urgent.

---

# 16. User Story Prioritization

User stories will be prioritized using three levels.

## P0 — Must Have

Required for the MVP to function.

Examples:

- Create Account
- Sign In
- Dashboard
- Create Task
- Edit Task
- Complete Task
- Delete Task
- Create Reminder
- Receive Notification

---

## P1 — Should Have

Important features that improve the MVP experience.

Examples:

- Recurring Tasks
- Recurring Reminders
- Documents
- Document Expiration Reminders
- Bills
- Recurring Bills
- Categories
- Search

---

## P2 — Future

Features that should not delay the initial MVP.

Examples:

- Family Accounts
- Shared Responsibilities
- Vehicle Management
- AI Assistant
- Smart Recommendations
- Bank Integrations
- Advanced Financial Analytics

---

# 17. MVP User Flow

The primary MVP user flow should be:

```text
User
  ↓
Create Account
  ↓
Onboarding
  ↓
Dashboard
  ↓
Create Task
  ↓
Set Due Date
  ↓
Set Reminder
  ↓
Save
  ↓
Dashboard
  ↓
Receive Notification
  ↓
Open Task
  ↓
Complete Task
```

The user should be able to complete this flow without unnecessary complexity.

---

# 18. Core User Experience Principle

The application should always try to answer:

> **"What should I do next?"**

The user should not have to navigate through multiple screens to understand what requires attention.

The application should surface important information automatically.

---

# 19. Phase 2 Goals

By the end of Phase 2, we should have:

- ✅ Defined primary user personas
- ✅ Defined user problems
- ✅ Defined user goals
- ✅ Defined core user journey
- ✅ Defined MVP user stories
- ✅ Defined future user stories
- ✅ Defined user story priorities
- ✅ Defined the primary MVP user flow

---

# 20. Next Phase

## Phase 3 — MVP Specification

The next phase will transform these user stories into a detailed specification of exactly what we will build.

### Phase 3 will define:

- MVP Features
- Functional Requirements
- Non-Functional Requirements
- Screen Requirements
- Navigation
- Data Requirements
- API Requirements
- Authentication Requirements
- Notification Requirements
- Feature Priorities
- MVP Boundaries
