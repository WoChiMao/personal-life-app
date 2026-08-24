# Personal Life App — UX / UI Design

## 1. Purpose

This document defines the user experience, navigation, screen structure, interaction patterns, and visual design direction for the Personal Life App MVP.

The purpose of this phase is to answer:

> **How should users experience the application?**

The UX should make personal life management feel simple, calm, and actionable.

The application should not feel like another complicated productivity tool.

---

# 2. UX Design Goals

The MVP UX should follow these principles:

### 1. Simple

Users should understand the application without needing instructions.

### 2. Fast

Common actions should require as few steps as possible.

### 3. Actionable

The application should tell users what requires attention.

### 4. Calm

The UI should avoid unnecessary visual noise.

### 5. Consistent

Similar actions should behave similarly throughout the application.

### 6. Mobile First

The experience should be designed primarily for mobile devices.

### 7. Accessible

The application should remain usable for people with different visual, motor, and cognitive needs.

---

# 3. Core UX Philosophy

The application should always try to answer:

> **"What should I do next?"**

When a user opens the application, they should not need to browse through multiple screens to understand their responsibilities.

The dashboard should provide an immediate overview.

The experience should follow:

```text
Open App
   ↓
Understand Situation
   ↓
Identify Priority
   ↓
Take Action
   ↓
Complete
```

---

# 5. Application Information Architecture

The MVP will use the following structure:

```text
Personal Life App
│
├── Authentication
│   ├── Welcome
│   ├── Sign Up
│   └── Login
│
└── Main Application
    │
    ├── Home
    │
    ├── Tasks
    │   ├── Task List
    │   ├── Task Details
    │   └── Create/Edit Task
    │
    ├── Documents
    │   ├── Document List
    │   ├── Document Details
    │   └── Create/Edit Document
    │
    ├── Bills
    │   ├── Bill List
    │   ├── Bill Details
    │   └── Create/Edit Bill
    │
    └── Settings
        ├── Profile
        └── Notifications
```

---

# 6. Primary Navigation

The recommended navigation is a bottom tab navigation.

```text
┌───────────────────────────────────┐
│                                   │
│           Application             │
│                                   │
│                                   │
│                                   │
│                                   │
├───────────────────────────────────┤
│  🏠      📋      📄      💰      ⚙️ │
│ Home   Tasks   Docs    Bills Settings
└───────────────────────────────────┘
```

The five primary areas are:

- Home
- Tasks
- Documents
- Bills
- Settings

---

# 7. Navigation Principles

Navigation should follow these principles:

- Important areas should be reachable quickly.
- Users should always know where they are.
- Back navigation should behave consistently.
- Destructive actions should not be hidden inside confusing navigation.
- Creating new items should be easy from the relevant section.

---

# 8. Global Create Action

The application should provide a quick way to create a responsibility.

A floating action button or prominent "+" action can be used.

### Example

```text
                    ┌───────────────┐
                    │               │
                    │      +        │
                    │               │
                    └───────────────┘
```

When pressed:

```text
What would you like to add?

📋 Task
💰 Bill
📄 Document
```

The exact UI pattern will be finalized during implementation.

---

# 9. Authentication UX

## 9.1 Welcome Screen

### Purpose

Introduce the application to new users.

### Example

```text
        🏠

   Personal Life App

Never forget the
important things in life.

        [ Get Started ]

        [ Sign In ]
```

The screen should be simple and focused.

---

# 10. Sign Up

The registration screen should contain only necessary information.

### Recommended Fields

- Name
- Email
- Password
- Confirm Password

### Primary Action

```text
[ Create Account ]
```

### Secondary Action

```text
Already have an account?
Sign In
```

---

# 11. Login

The login screen should contain:

- Email
- Password

```text
[ Sign In ]
```

```text
Forgot password?
```

```text
Don't have an account?
Create one
```

---

# 12. Onboarding

The onboarding process should be short.

The goal is to teach the user the core concept rather than collect large amounts of data.

## Screen 1 — Introduction

```text
Your life has a lot to remember.

Let's put the important things
in one place.
```

## Screen 2 — Responsibilities

```text
Tasks
Bills
Documents
Reminders

Everything important,
organized in one place.
```

## Screen 3 — Dashboard

```text
See what needs your attention
today and what's coming next.
```

## Screen 4 — Create First Item

The user should be encouraged to create their first responsibility.

```text
Let's create your first task.

[ Create Task ]
```

---

# 13. Home Dashboard

The dashboard is the most important screen.

### Primary Objective

Show the user what requires attention.

## 13.1 Dashboard Layout

```text
┌──────────────────────────────┐
│ Good morning, Arvi 👋        │
│                              │
│ Here's what needs attention. │
│                              │
├──────────────────────────────┤
│                              │
│ 🔴 OVERDUE                   │
│                              │
│ Electricity Bill             │
│ Due yesterday                │
│                              │
├──────────────────────────────┤
│                              │
│ TODAY                        │
│                              │
│ 📋 Call Insurance            │
│ 🛒 Buy Groceries             │
│                              │
├──────────────────────────────┤
│                              │
│ UPCOMING                     │
│                              │
│ 📄 License expires           │
│    In 14 days                │
│                              │
│ 💰 Internet bill             │
│    Due in 5 days             │
│                              │
└──────────────────────────────┘
```

---

# 14. Dashboard Sections

The dashboard should contain:

## Greeting

A simple personalized greeting.

Example:

```text
Good morning 👋
```

## Priority / Overdue

Items requiring immediate attention.

## Today

Items due today.

## Upcoming

Important upcoming responsibilities.

## Quick Add

A convenient way to create a new item.

---

# 15. Dashboard Priority

The dashboard should prioritize information using:

1. Overdue
2. Due Today
3. High Priority
4. Upcoming

The application should avoid showing every piece of information equally.

---

# 16. Empty Dashboard

A new user should see a friendly empty state.

### Example

```text
              ✨

        You're all caught up!

    Add your first responsibility
    and we'll help you remember it.

          [ Add Something ]
```

The empty state should feel encouraging rather than empty or broken.

---

# 17. Task List

The task list should allow users to see active responsibilities.

### Example

```text
Tasks

Today

○ Call Insurance
  Today · High

○ Buy Groceries
  Today · Medium

Upcoming

○ Service Motorcycle
  Aug 30 · Medium

○ Renew Registration
  Sep 15 · High
```

---

# 18. Task Creation

The task creation screen should be simple.

### Recommended Layout

```text
Create Task

Title
[________________________]

Description
[________________________]
[________________________]

Due Date
[ Aug 30, 2026 ]

Time
[ 6:00 PM ]

Priority
[ Medium ▼ ]

Category
[ Personal ▼ ]

Reminder
[ 1 day before ▼ ]

        [ Save Task ]
```

---

# 19. Task Details

Task details should show:

```text
Call Insurance

Due:
August 30, 2026

Time:
6:00 PM

Priority:
High

Category:
Finance

Reminder:
1 day before

Description:
Call insurance company to confirm renewal.

[ Mark Complete ]

[ Edit ]

[ Delete ]
```

---

# 20. Task Completion

When a user completes a task:

```text
○ Call Insurance
```

becomes:

```text
✓ Call Insurance
```

The completed state should be visually clear.

The task should move to completed history or disappear from active tasks depending on the screen.

---

# 21. Task Deletion

Deleting a task should require confirmation.

### Example

```text
Delete Task?

Are you sure you want to delete
"Call Insurance"?

This action cannot be undone.

[ Cancel ]    [ Delete ]
```

---

# 22. Recurring Tasks

When creating a recurring task, the user should be able to select:

```text
Repeat

Never
Daily
Weekly
Monthly
Yearly
```

### Example

```text
Pay Rent

Repeat:
Monthly

Next:
September 1
```

Advanced recurrence rules are not required for MVP.

---

# 23. Document List

The document screen should focus on expiration awareness.

### Example

```text
Documents

⚠️ Expiring Soon

Driver's License
Expires in 14 days

Insurance
Expires in 28 days

Valid

Passport
Expires in 2 years
```

---

# 24. Document Creation

### Recommended Fields

```text
Add Document

Document Name
[________________________]

Category
[ Identification ▼ ]

Document Number
[________________________]

Issue Date
[________________________]

Expiration Date
[________________________]

Notes
[________________________]

Reminder
[ 30 days before ▼ ]

        [ Save Document ]
```

---

# 25. Document Details

### Example

```text
Driver's License

Category:
Identification

Document Number:
XXXX-XXXX

Issued:
January 15, 2022

Expires:
January 15, 2027

Reminder:
30 days before

Status:
Valid

[ Edit ]

[ Delete ]
```

---

# 26. Document Expiration Status

The application should visually communicate document status.

### Possible States

- Valid
- Expiring Soon
- Expired

The exact visual treatment should not rely solely on color.

Text and icons should also communicate the state.

---

# 27. Bill List

The bill screen should show financial responsibilities.

### Example

```text
Bills

Due Soon

Internet
₱1,800
Due in 5 days

Electricity
₱2,450
Due in 8 days

Paid

Water
₱850
Paid Aug 10
```

---

# 28. Bill Creation

### Recommended Fields

```text
Add Bill

Bill Name
[________________________]

Amount
[________________________]

Due Date
[________________________]

Category
[ Finance ▼ ]

Repeat
[ Monthly ▼ ]

Reminder
[ 3 days before ▼ ]

        [ Save Bill ]
```

---

# 29. Bill Details

### Example

```text
Internet

Amount:
₱1,800

Due:
August 30, 2026

Repeat:
Monthly

Reminder:
3 days before

Status:
Upcoming

[ Mark as Paid ]

[ Edit ]

[ Delete ]
```

---

# 30. Mark Bill as Paid

When a bill is marked as paid:

```text
Internet

Status:
✓ Paid

Paid:
August 27, 2026
```

For recurring bills, the next occurrence should eventually be generated automatically.

---

# 31. Settings

Settings should remain simple.

### Example

```text
Settings

Account
────────────
Profile
Notifications

Preferences
────────────
Theme
Date Format
Time Zone

Security
────────────
Change Password
Log Out
```

---

# 32. Profile

The profile screen should allow the user to manage basic information.

### Example

```text
Profile

Name
Arvi

Email
example@email.com

[ Edit Profile ]
```

---

# 33. Notification Settings

Users should have control over reminders.

### Example

```text
Notifications

Task Reminders       ON
Bill Reminders       ON
Document Reminders   ON

Quiet Hours
OFF

Default Reminder
1 day before
```

---

# 34. Quick Add Experience

The user should be able to quickly add a responsibility.

### Example

```text
                 + Add

                   ↓

       ┌─────────────────────┐
       │ 📋 Task             │
       │ 💰 Bill             │
       │ 📄 Document         │
       └─────────────────────┘
```

The goal is to reduce the friction of adding information.

---

# 35. Interaction Patterns

The application should use consistent interaction patterns.

## Buttons

Primary actions should be visually prominent.

### Examples

```text
[ Save ]
[ Create Task ]
[ Mark Complete ]
```

### Secondary Actions

```text
[ Cancel ]
[ Edit ]
```

### Destructive Actions

```text
[ Delete ]
```

---

# 36. Forms

Forms should:

- Clearly label fields.
- Display validation errors.
- Preserve entered information when possible.
- Avoid unnecessary fields.
- Use appropriate keyboard types.
- Use native date/time selectors where appropriate.

---

# 37. Form Validation

Validation should happen at the appropriate time.

### Example

```text
Title
[                         ]

⚠ Please enter a task title.
```

The user should understand:

- What is wrong.
- Why it is wrong.
- How to fix it.

---

# 38. Loading States

The application should provide feedback while loading data.

### Examples

```text
Loading tasks...
Loading bills...
Loading documents...
```

For larger content areas, skeleton loading may be used.

---

# 39. Error States

Errors should be understandable.

### Example

```text
Something went wrong.

We couldn't load your tasks.

[ Try Again ]
```

Technical errors should not be exposed to the user.

---

# 40. Offline / Poor Connection Experience

The application should eventually handle poor network conditions gracefully.

### For MVP

- Display connection errors clearly.
- Avoid losing user-entered form data.
- Allow retrying failed requests.

Advanced offline-first functionality can be introduced later.

---

# 41. Confirmation Patterns

Confirmation should be used for destructive actions.

### Examples

- Delete Task
- Delete Bill
- Delete Document
- Logout if unsaved changes exist

Normal actions such as saving or completing a task should not require unnecessary confirmation.

---

# 42. Visual Design Direction

The visual identity should feel:

- Clean
- Modern
- Calm
- Trustworthy
- Minimal
- Friendly

The application should feel like a personal assistant rather than an enterprise management system.

---

# 43. Color Strategy

The final color palette will be defined during implementation.

The design should use a restrained palette.

### Potential Semantic States

- Primary
- Success
- Warning
- Danger
- Neutral

Status should never rely on color alone.

### Examples

```text
⚠ Expiring Soon
✓ Valid
! Overdue
```

---

# 44. Typography

Typography should prioritize readability.

The system should establish consistent levels:

- Large Heading
- Section Heading
- Card Title
- Body
- Secondary Text
- Caption

The final font family will be selected during implementation.

---

# 45. Spacing

The UI should use a consistent spacing system.

A base spacing unit should be established.

### Example Scale

```text
4
8
12
16
20
24
32
40
48
```

Components should use the spacing system rather than arbitrary values.

---

# 46. Cards

Cards may be used for:

- Dashboard Responsibilities
- Bills
- Documents
- Important Reminders

### Example

```text
┌───────────────────────────┐
│ 💰 Electricity            │
│                           │
│ ₱2,450                    │
│ Due today                 │
│                           │
│ [ Mark as Paid ]          │
└───────────────────────────┘
```

Cards should not be overused.

---

# 47. Icons

Icons should reinforce meaning rather than replace labels.

### Examples

- 🏠 Home
- 📋 Tasks
- 📄 Documents
- 💰 Bills
- ⚙ Settings

The production application should use a consistent icon library rather than emoji.

---

# 48. Touch Targets

Interactive elements should have sufficiently large touch targets.

Buttons and controls should be easy to tap without accidentally activating adjacent elements.

---

# 49. Accessibility

The application should consider:

## Visual

- Adequate contrast
- Readable font sizes
- Scalable text
- Non-color status indicators

## Motor

- Large touch targets
- Appropriate spacing
- Avoid tiny buttons

## Cognitive

- Simple language
- Predictable navigation
- Clear confirmation
- Consistent terminology

## Screen Readers

Important UI elements should have accessible labels.

---

# 50. Dark Mode

Dark mode is desirable but not required for the initial MVP.

If implemented, all core components should support both:

- Light Theme
- Dark Theme

The theme should be applied consistently across the application.

---

# 51. Responsive Design

Although the primary target is mobile, the UI should account for different device sizes.

The application should support:

- Small Phones
- Standard Phones
- Large Phones
- Tablets where applicable

Layouts should avoid fixed dimensions that break on smaller screens.

---

# 52. Notification UX

Notifications should be useful and actionable.

### Example

```text
🔔 Personal Life App

Driver's License expires in 14 days.

[ View ]
```

Notifications should take the user directly to the relevant item when possible.

---

# 53. Notification Principles

The application should avoid notification overload.

Notifications should be:

- Relevant
- Timely
- Actionable
- User-Controlled

The system should not send notifications simply because an event exists.

---

# 54. User Flow — Create Task

```text
Dashboard
    ↓
Tap +
    ↓
Select Task
    ↓
Create Task
    ↓
Enter Title
    ↓
Set Due Date
    ↓
Optional Reminder
    ↓
Save
    ↓
Task Created
    ↓
Dashboard Updated
```

---

# 55. User Flow — Create Bill

```text
Dashboard
    ↓
Tap +
    ↓
Select Bill
    ↓
Enter Bill Name
    ↓
Enter Amount
    ↓
Set Due Date
    ↓
Set Recurrence
    ↓
Set Reminder
    ↓
Save
    ↓
Bill Appears
    ↓
Reminder Scheduled
```

---

# 56. User Flow — Create Document

```text
Dashboard
    ↓
Tap +
    ↓
Select Document
    ↓
Enter Document Information
    ↓
Set Expiration Date
    ↓
Set Reminder
    ↓
Save
    ↓
Document Appears
    ↓
Expiration Tracked
```

---

# 57. User Flow — Complete Task

```text
Dashboard
    ↓
Tap Task
    ↓
Task Details
    ↓
Mark Complete
    ↓
Task Status = Completed
    ↓
Dashboard Updated
```

---

# 58. User Flow — Pay Bill

```text
Dashboard
    ↓
Tap Bill
    ↓
Bill Details
    ↓
Mark as Paid
    ↓
Payment Date Recorded
    ↓
Bill Status = Paid
    ↓
Dashboard Updated
```

---

# 59. User Flow — Document Expiration

```text
Document Created
      ↓
Expiration Date Stored
      ↓
Reminder Scheduled
      ↓
Reminder Triggered
      ↓
User Receives Notification
      ↓
User Opens Document
      ↓
User Takes Action
```

---

# 60. UX Copy Guidelines

The application should use simple, human language.

### Prefer

```text
What needs your attention?
```

Instead of:

```text
Pending Responsibilities
```

### Prefer

```text
You're all caught up.
```

Instead of:

```text
No records found.
```

### Prefer

```text
Add your first task.
```

Instead of:

```text
Create new task record.
```

---

# 61. Terminology

The application should use consistent terminology.

### Preferred Terms

- Task
- Reminder
- Bill
- Document
- Due Date
- Expiration Date
- Complete
- Upcoming
- Overdue
- Paid

Avoid switching between different terms for the same concept.

For example, do not use:

- Task
- Activity
- Job
- Action
- Responsibility

interchangeably.

**Task** should be the standard MVP term.

---

# 62. UX Anti-Patterns to Avoid

The application should avoid:

### Too Many Notifications

Users should not feel spammed.

### Too Many Required Fields

Users should be able to create something quickly.

### Complex Navigation

Important features should not be buried.

### Excessive Popups

The application should not constantly interrupt the user.

### Overly Dense Dashboard

The dashboard should prioritize rather than display everything.

### Unnecessary Animations

Animations should support understanding rather than distract.

### Technical Language

Users should not need to understand technical concepts.

---

# 63. MVP Design Priority

When making design decisions, prioritize:

1. Usability
2. Clarity
3. Speed
4. Accessibility
5. Consistency
6. Visual Polish

A beautiful interface that is difficult to use is not successful UX.

---

# 64. UX Testing Plan

Before development is considered complete, the core flows should be tested with real users.

### Test 1

Create a task.

### Test 2

Create a bill.

### Test 3

Create a document.

### Test 4

Set a reminder.

### Test 5

Find an upcoming responsibility.

### Test 6

Complete a task.

### Test 7

Mark a bill as paid.

The user should be able to complete these flows without needing detailed instructions.

---

# 65. Design Deliverables

The following deliverables should eventually be created:

- ✅ Application Information Architecture
- ✅ User Flows
- ✅ Low-Fidelity Wireframes
- ✅ High-Fidelity UI Designs
- ✅ Design System
- ✅ Component Definitions
- ✅ Accessibility Guidelines
- ✅ Interaction Specifications

---

# 66. UX / UI Design Completion Criteria

Phase 4 is considered complete when:

- ✅ Information Architecture is defined
- ✅ Navigation is defined
- ✅ Primary Screens are identified
- ✅ Core User Flows are defined
- ✅ Dashboard Structure is defined
- ✅ Task UX is defined
- ✅ Document UX is defined
- ✅ Bill UX is defined
- ✅ Settings UX is defined
- ✅ Form Behavior is defined
- ✅ Error States are defined
- ✅ Empty States are defined
- ✅ Notification UX is defined
- ✅ Accessibility Principles are defined
- ✅ Visual Design Direction is defined

---

# 67. Next Phase

## Phase 5 — Technical Architecture

The next phase will define how the application will actually be built.

### We will decide:

- Mobile Technology
- Frontend Architecture
- Backend Architecture
- API Architecture
- PostgreSQL Architecture
- Authentication
- Authorization
- Notifications
- File Storage
- Environment Configuration
- Security
- Deployment
- Development Environments
- Project Structure

### Major Question

> **"How are we going to build this application properly so it can grow beyond the MVP?"**
