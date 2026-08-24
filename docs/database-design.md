# Personal Life App — Database Design

## 1. Purpose

This document defines the PostgreSQL database architecture for the Personal Life App.

It translates the product requirements and technical architecture into a concrete relational data model.

The database must support the MVP while providing a clean foundation for future capabilities such as:

- Recurring responsibilities
- Bills and payments
- Documents
- Notifications
- Household management
- Family members
- Vehicles
- Subscriptions
- Personal goals
- AI-powered recommendations
- External integrations

The database should remain intentionally simple during the MVP.

> **Do not design the database for features we are not building yet.**

---

## 2. Database Technology

The application will use:

```text
PostgreSQL
```

PostgreSQL is the primary persistent data store for the application.

The backend will communicate with PostgreSQL through a TypeScript-compatible ORM/database layer.

The mobile application must never connect directly to PostgreSQL.

```text
Mobile App
    ↓
Backend API
    ↓
Database Layer / ORM
    ↓
PostgreSQL
```

---

# 3. Database Design Principles

The database should follow these principles:

- Every user-owned record must belong to a user.
- Relationships should use foreign keys.
- Data integrity should be enforced at the database level where appropriate.
- Business logic should primarily live in the backend.
- Dates and timestamps must be handled consistently.
- Tables should avoid unnecessary duplication.
- Indexes should support actual query patterns.
- Sensitive information should be minimized.
- Soft deletion should be used only where it provides value.
- Future features should not complicate the MVP unnecessarily.

---

# 4. Entity Overview

The initial MVP will contain the following core entities:

- User
- Category
- Task
- Reminder
- Bill
- Document
- Notification

### High-Level Relationship

```text
                         ┌──────────────┐
                         │     User     │
                         └──────┬───────┘
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
             ▼                  ▼                  ▼
          Category            Task               Bill
             │                  │                  │
             │                  ▼                  │
             │              Reminder ◄─────────────┘
             │                  ▲
             │                  │
             └────────────┐     │
                          │     │
                          ▼     │
                       Document
                          │
                          ▼
                     Notification
```

> Not every relationship shown above represents a direct foreign key. Some relationships are logical relationships handled by the application.

---

# 5. Entity Relationship Overview

The core ownership model is:

```text
User
 │
 ├── Categories
 ├── Tasks
 ├── Bills
 ├── Documents
 ├── Reminders
 └── Notifications
```

Every user must only be able to access their own records.

---

# 6. Naming Conventions

Database naming will use:

```text
snake_case
```

### Examples

```text
user_id
created_at
updated_at
due_date
expiration_date
is_completed
```

### Table Names

```text
users
tasks
bills
documents
reminders
notifications
categories
```

---

# 7. Primary Key Strategy

All primary keys will use UUIDs.

### Example

```sql
id UUID PRIMARY KEY
```

### Example UUID

```text
550e8400-e29b-41d4-a716-446655440000
```

UUIDs are preferred because they:

- Avoid predictable sequential IDs.
- Work well across distributed systems.
- Can be generated independently.
- Make future scaling easier.
- Avoid exposing simple record counts through URLs.

---

# 8. UUID Generation

The database should use a PostgreSQL-supported UUID generation mechanism.

The exact implementation will depend on the migration/ORM tooling selected during development.

The application should not depend on sequential integer IDs.

---

# 9. Standard Timestamp Fields

Most entities should contain:

```text
created_at
updated_at
```

### Example

```sql
created_at TIMESTAMPTZ NOT NULL
updated_at TIMESTAMPTZ NOT NULL
```

These timestamps represent when the record was created and last updated.

---

# 10. Timestamp Standard

The database should use:

```text
TIMESTAMPTZ
```

for timestamps that represent a specific point in time.

### Examples

```text
created_at
updated_at
scheduled_at
completed_at
```

The application should store these values consistently in UTC.

---

# 11. Date vs Timestamp

Use:

```text
DATE
```

when only the calendar date matters.

### Examples

```text
birthday
due_date
expiration_date
issue_date
```

Use:

```text
TIMESTAMPTZ
```

when the exact time matters.

### Examples

```text
created_at
updated_at
reminder_time
completed_at
```

This distinction is important for avoiding timezone bugs.

---

# 12. Users Table

### Table

```text
users
```

### Purpose

Stores application users.

### Proposed Structure

```text
users
-----------------------------------------
id                  UUID PRIMARY KEY
email               VARCHAR
password_hash       VARCHAR
first_name          VARCHAR
last_name           VARCHAR
timezone            VARCHAR
is_active           BOOLEAN
created_at          TIMESTAMPTZ
updated_at          TIMESTAMPTZ
```

---

# 13. Users — Field Definitions

### id

```text
UUID
PRIMARY KEY
```

Unique identifier.

### email

```text
VARCHAR
NOT NULL
UNIQUE
```

User's login email.

The backend should normalize email addresses before storage.

### password_hash

```text
VARCHAR
NOT NULL
```

Hashed password.

The original password must never be stored.

### first_name

```text
VARCHAR
NOT NULL
```

### last_name

```text
VARCHAR
NULLABLE
```

### timezone

Example:

```text
Asia/Manila
```

```text
VARCHAR
NOT NULL
```

### is_active

```text
BOOLEAN
NOT NULL
DEFAULT TRUE
```

Indicates whether the account is active.

---

# 14. Categories Table

### Table

```text
categories
```

### Purpose

Provides a way to organize tasks, bills, and documents.

### Example Categories

- Personal
- Family
- Finance
- Health
- Vehicle
- Home
- Work
- Documents
- Other

### Proposed Structure

```text
categories
-----------------------------------------
id                  UUID PRIMARY KEY
user_id             UUID
name                VARCHAR
color               VARCHAR
icon                VARCHAR
created_at          TIMESTAMPTZ
updated_at          TIMESTAMPTZ
```

---

# 15. Category Ownership

Categories are user-specific.

### Relationship

```text
users
  │
  └── categories
```

### Foreign Key

```text
categories.user_id
    ↓
users.id
```

This allows each user to customize their categories.

---

# 16. Category Name

Required field:

```sql
name VARCHAR NOT NULL
```

Example:

```text
Finance
```

---

# 17. Category Appearance

Optional UI-related fields:

- color
- icon

These values should not be required for core business logic.

---

# 18. Tasks Table

### Table

```text
tasks
```

### Purpose

Stores personal responsibilities and actionable items.

### Examples

- Renew Driver's License
- Call Insurance Company
- Buy Groceries
- Schedule Dentist Appointment
- Clean Air Conditioner

### Proposed Structure

```text
tasks
-----------------------------------------
id                  UUID PRIMARY KEY
user_id             UUID
category_id         UUID
title               VARCHAR
description         TEXT
status              VARCHAR
priority            VARCHAR
due_date            DATE
due_time            TIMESTAMPTZ
is_completed        BOOLEAN
completed_at        TIMESTAMPTZ
recurrence_rule     VARCHAR
created_at          TIMESTAMPTZ
updated_at          TIMESTAMPTZ
```

---

# 19. Task Ownership

```text
tasks.user_id
    ↓
users.id
```

This relationship is mandatory.

---

# 20. Task Category

```text
tasks.category_id
    ↓
categories.id
```

The category may be nullable.

This allows users to create tasks without assigning a category.

---

# 21. Task Title

Required field:

```sql
title VARCHAR NOT NULL
```

Example:

```text
Renew Driver's License
```

---

# 22. Task Description

Optional field:

```sql
description TEXT
```

Example:

```text
Bring passport and proof of address.
```

---

# 23. Task Status

Supported values:

- ACTIVE
- COMPLETED
- CANCELLED

Example:

```sql
status VARCHAR NOT NULL
```

A PostgreSQL enum may be introduced later if appropriate.

---

# 24. Task Priority

Supported values:

- LOW
- MEDIUM
- HIGH

Example:

```sql
priority VARCHAR NOT NULL DEFAULT 'MEDIUM'
```

---

# 25. Task Due Date

```sql
due_date DATE
```

Optional.

Some tasks may not have deadlines.

---

# 26. Task Due Time

```sql
due_time TIMESTAMPTZ
```

The product should distinguish between:

```text
Due Date:
August 30
```

and

```text
Due Date & Time:
August 30 at 9:00 AM
```

---

# 27. Task Completion

When a task is completed:

```text
is_completed = true
completed_at = current timestamp
status = COMPLETED
```

The backend should ensure consistency.

---

# 28. Task Recurrence

Supported values:

- DAILY
- WEEKLY
- MONTHLY
- YEARLY

Stored in:

```text
recurrence_rule
```

The MVP should avoid a complex recurrence engine.

---

# 29. Bills Table

### Table

```text
bills
```

### Purpose

Stores recurring or one-time financial obligations.

### Examples

- Electricity
- Internet
- Rent
- Insurance
- Credit Card
- Phone
- Subscription

### Proposed Structure

```text
bills
-----------------------------------------
id                  UUID PRIMARY KEY
user_id             UUID
category_id         UUID
name                VARCHAR
description         TEXT
amount              NUMERIC
currency            VARCHAR
due_date            DATE
status              VARCHAR
recurrence_rule     VARCHAR
is_autopay          BOOLEAN
paid_at             TIMESTAMPTZ
created_at          TIMESTAMPTZ
updated_at          TIMESTAMPTZ
```
---

# 51. Notification Ownership

Every notification belongs to a user.

### Relationship

```text
notifications.user_id
    ↓
users.id
```

---

# 52. Notification Types

Initial notification types may include:

- TASK_REMINDER
- BILL_REMINDER
- DOCUMENT_EXPIRATION
- SYSTEM

Future types can be added later.

---

# 53. Notification Read State

The application should support:

```sql
is_read BOOLEAN
```

### Default

```text
FALSE
```

When the user opens the notification:

```text
is_read = TRUE
read_at = current timestamp
```

---

# 54. Foreign Key Strategy

Relationships should use explicit foreign keys.

### Example

```sql
tasks.user_id
    REFERENCES users(id)
```

Foreign keys should prevent orphaned records.

---

# 55. Delete Behavior

Deletion behavior must be intentional.

For user-owned data:

```text
User
 └── Tasks
```

If a user is permanently deleted, related records may eventually need to be deleted or anonymized.

The exact production strategy will be finalized when account deletion is implemented.

---

# 56. Soft Delete Strategy

The MVP should **NOT** automatically add:

```text
deleted_at
```

to every table.

Soft deletion should only be used when it provides meaningful product value.

Potential candidates:

- documents
- tasks
- bills

may eventually use soft deletion.

For the MVP, normal deletion is acceptable where data recovery is not required.

---

# 57. Account Deletion

The application should eventually support account deletion.

### Future Process

```text
User requests account deletion
        ↓
Verify request
        ↓
Delete / anonymize personal data
        ↓
Remove associated data
        ↓
Deactivate account
```

The exact retention policy must be defined before production launch.

---

# 58. Cascading Deletes

Cascading deletes should be used carefully.

### Example

```text
User
 ↓
Task
```

may use:

```sql
ON DELETE CASCADE
```

for user-owned records.

However, cascading deletes should not be used blindly because some records may require retention or auditing.

---

# 59. Unique Constraints

### Required Unique Constraints

```text
users.email
```

A user's email must be unique.

Category names may **NOT** necessarily be globally unique because different users can have the same category name.

Example:

```text
User A → Finance
User B → Finance
```

Both are valid.

---

# 60. Nullability

Fields should only be nullable when the information is genuinely optional.

### Example

```sql
tasks.title         NOT NULL
tasks.description   NULLABLE
tasks.due_date      NULLABLE
```

Avoid making every field nullable.

---

# 61. Default Values

Defaults should be used for predictable values.

### Examples

```text
users.is_active = TRUE

tasks.status = ACTIVE

tasks.priority = MEDIUM

bills.is_autopay = FALSE

notifications.is_read = FALSE
```

---

# 62. Check Constraints

The database should enforce basic data integrity where appropriate.

### Example

```sql
amount >= 0
```

for bills.

The backend should also validate these rules.

Database constraints provide an additional layer of protection.

---

# 63. Monetary Precision

Bill amounts should use:

```sql
NUMERIC(12,2)
```

### Example

```text
1500.00
```

This provides predictable decimal precision.

Avoid:

```text
FLOAT
REAL
```

for monetary values.

---

# 64. Indexing Strategy

Indexes should primarily support:

- User Lookups
- Due Dates
- Reminder Scheduling
- Expiration Dates
- Status Filtering

### Potential Indexes

```text
users(email)

tasks(user_id)
tasks(user_id, due_date)
tasks(user_id, status)

bills(user_id)
bills(user_id, due_date)
bills(user_id, status)

documents(user_id)
documents(user_id, expiration_date)

reminders(user_id)
reminders(reminder_at, status)

notifications(user_id)
notifications(user_id, is_read)
```

---

# 65. Composite Indexes

Composite indexes may improve common queries.

### Example

```text
tasks(user_id, status, due_date)
```

This can support queries such as:

> Get active tasks for a user ordered by due date.

Indexes should be based on actual query patterns rather than created unnecessarily.

---

# 66. Query Example — Today's Tasks

### Conceptually

```sql
SELECT *
FROM tasks
WHERE user_id = current_user
AND due_date = today
AND status = 'ACTIVE'
ORDER BY due_date;
```

An appropriate index can make this query efficient.

---

# 67. Query Example — Upcoming Bills

### Conceptually

```sql
SELECT *
FROM bills
WHERE user_id = current_user
AND status IN ('UPCOMING', 'DUE')
ORDER BY due_date;
```

---

# 68. Query Example — Expiring Documents

### Conceptually

```sql
SELECT *
FROM documents
WHERE user_id = current_user
AND expiration_date IS NOT NULL
ORDER BY expiration_date;
```

---

# 69. Query Example — Pending Reminders

The notification scheduler will need to find reminders that are due.

### Conceptually

```sql
SELECT *
FROM reminders
WHERE status = 'PENDING'
AND reminder_at <= NOW()
ORDER BY reminder_at;
```

An index should support this query.

---

# 70. User Data Isolation

Every query involving user-owned data must include user ownership filtering.

### Incorrect

```sql
SELECT *
FROM tasks
WHERE id = task_id;
```

### Correct

```sql
SELECT *
FROM tasks
WHERE id = task_id
AND user_id = authenticated_user_id;
```

The backend must never rely solely on the client to provide a valid user ID.

---

# 71. Never Trust Client User IDs

The mobile application should not be allowed to determine the owner of a record.

### Incorrect

```json
{
  "userId": "some-user-id",
  "title": "My Task"
}
```

The backend should determine the user from the authenticated session.

### Conceptually

```text
Authenticated Token
        ↓
Backend identifies User
        ↓
Backend creates record
        ↓
user_id = authenticated user
```

---

# 72. Database Transactions

Transactions should be used when multiple database operations must succeed or fail together.

### Example

```text
Create Bill
    +
Create Reminder
```

### Conceptually

```sql
BEGIN;

Create Bill;

Create Reminder;

COMMIT;
```

If an error occurs:

```sql
ROLLBACK;
```

---

# 73. Data Integrity

The system should enforce important relationships at multiple levels.

```text
Mobile Validation
        ↓
Backend Validation
        ↓
Database Constraints
```

No single layer should be treated as the only protection.

---

# 74. Database Migrations

All database schema changes must be managed through migrations.

### Example

```text
001_create_users
002_create_categories
003_create_tasks
004_create_bills
005_create_documents
006_create_reminders
007_create_notifications
```

Migrations must be committed to Git.

---

# 75. Migration Principles

Migrations should:

- Be repeatable
- Be version controlled
- Be reviewed before production
- Avoid destructive changes where possible
- Preserve existing production data

---

# 76. Seed Data

Development environments may contain seed data.

### Example

```text
Demo User
    │
    ├── Tasks
    ├── Bills
    ├── Documents
    └── Reminders
```

Seed data must never contain real user information.

---

# 77. Database Diagram

### Initial Logical Model

```text
┌─────────────────────┐
│       users         │
├─────────────────────┤
│ id PK               │
│ email               │
│ password_hash       │
│ first_name          │
│ last_name           │
│ timezone            │
│ is_active           │
│ created_at          │
│ updated_at          │
└──────────┬──────────┘
           │
     ┌─────┼──────────────────────────────┐
     │     │              │               │
     ▼     ▼              ▼               ▼
┌──────────┐ ┌────────┐ ┌────────────┐ ┌──────────────┐
│categories│ │ tasks  │ │   bills    │ │  documents   │
└──────────┘ └────┬───┘ └─────┬──────┘ └──────┬───────┘
                  │            │               │
                  └────────────┼───────────────┘
                               │
                               ▼
                         ┌───────────┐
                         │ reminders │
                         └─────┬─────┘
                               │
                               ▼
                       ┌────────────────┐
                       │ notifications  │
                       └────────────────┘
```

---

# 78. Initial Schema Summary

The MVP database consists of:

- users
- categories
- tasks
- bills
- documents
- reminders
- notifications

---

# 79. Future Database Entities

The following entities are intentionally excluded from the MVP.

### Potential Future Tables

- households
- household_members
- vehicles
- vehicle_maintenance
- subscriptions
- goals
- calendar_events
- attachments
- document_files
- payments
- recurring_transactions
- integrations
- external_accounts
- ai_recommendations
- audit_logs

These should only be introduced when their corresponding product features are implemented.

---

# 80. Household / Family Expansion

Future architecture may introduce:

```text
users
   │
   ▼
households
   │
   ├── household_members
   │
   ├── shared_tasks
   ├── shared_bills
   └── shared_documents
```

The MVP remains strictly personal.

---

# 81. Vehicle Expansion

Future vehicle functionality may use:

```text
vehicles
    │
    ├── registration
    ├── insurance
    ├── maintenance
    └── reminders
```

This should integrate with the same reminder infrastructure.

---

# 82. Subscription Expansion

Future subscriptions may use:

```text
subscriptions
    │
    ├── name
    ├── amount
    ├── billing_cycle
    ├── next_billing_date
    └── reminder
```

This may eventually share concepts with bills but should remain a separate domain if the product requirements justify it.

---

# 83. Goal Expansion

Future personal goals may use:

```text
goals
    │
    ├── title
    ├── description
    ├── target_date
    ├── progress
    └── status
```

Goals can later connect with tasks.

---

# 84. AI Expansion

Future AI functionality may introduce:

```text
ai_recommendations

User Data
    ↓
AI Processing
    ↓
Recommendation
```

### Example Recommendation

> "Your car insurance expires in 45 days. Would you like me to create a reminder?"

AI-related tables should not be created during the MVP unless required.

---

# 85. Audit Logging

Sensitive operations may eventually require audit logs.

### Potential Table

```text
audit_logs
-----------------------------------------
id
user_id
action
entity_type
entity_id
created_at
```

### Examples

- DOCUMENT_CREATED
- DOCUMENT_DELETED
- BILL_UPDATED
- ACCOUNT_DELETED

This is a future capability.

---

# 86. Privacy Considerations

The database may eventually contain highly personal information.

### Potential Sensitive Data

- Document Numbers
- Financial Information
- Personal Responsibilities
- Household Information
- Important Dates

Therefore:

- Minimize collected data.
- Never store unnecessary information.
- Restrict access.
- Encrypt data in transit.
- Use secure infrastructure.
- Protect database credentials.
- Avoid exposing sensitive information through APIs.

---

# 87. Data Retention

Data retention policies will be defined before production launch.

The product should eventually define:

- What data is retained?
- How long is it retained?
- What happens after account deletion?
- What happens to backups?

The MVP does not need a complete retention system yet.

---

# 88. Database Backup Strategy

Production PostgreSQL must eventually have:

```text
Automated Backups
+
Backup Retention
+
Recovery Testing
```

The exact provider and retention period will be determined during deployment planning.

---

# 89. Database Performance Principles

The MVP database should prioritize:

- Correctness
- Data Integrity
- Appropriate Indexes
- Efficient Queries
- Pagination
- Avoiding Unnecessary Joins
- Avoiding Duplicate Data

Do not prematurely optimize.

---

# 90. Database Scaling Strategy

### Initial Architecture

```text
Mobile
   ↓
Backend
   ↓
PostgreSQL
```

### Future

```text
Mobile
   ↓
Load Balancer
   ↓
Multiple API Instances
   ↓
PostgreSQL
   +
Read Replicas
   +
Cache
```

Scaling should only occur when actual usage requires it.

---

# 91. Database Security

The database should:

- Require Authentication
- Use Encrypted Connections where supported
- Restrict Network Access
- Use Separate Credentials per Environment
- Never expose PostgreSQL directly to mobile clients
- Use Least-Privilege Database Users
- Keep Credentials outside Git

---

# 92. Development Database

Developers may use:

> Local PostgreSQL

or a disposable development database.

Development data must never contain real personal information.

---

# 93. Production Database

Production PostgreSQL must be:

- Managed
- Backed Up
- Secured
- Isolated
- Monitored

Production credentials must never be stored in the repository.

---

# 94. Example Task Record

```json
{
  "id": "uuid",
  "user_id": "uuid",
  "category_id": "uuid",
  "title": "Renew driver's license",
  "description": "Prepare required documents",
  "status": "ACTIVE",
  "priority": "HIGH",
  "due_date": "2027-01-15",
  "is_completed": false
}
```

---

# 95. Example Bill Record

```json
{
  "id": "uuid",
  "user_id": "uuid",
  "name": "Internet Bill",
  "amount": 2500.00,
  "currency": "PHP",
  "due_date": "2026-09-05",
  "status": "UPCOMING",
  "is_autopay": false
}
```

---

# 96. Example Document Record

```json
{
  "id": "uuid",
  "user_id": "uuid",
  "name": "Driver's License",
  "document_number": "REDACTED",
  "issue_date": "2022-01-15",
  "expiration_date": "2027-01-15",
  "status": "ACTIVE"
}
```

---

# 97. Example Reminder Record

```json
{
  "id": "uuid",
  "user_id": "uuid",
  "document_id": "uuid",
  "title": "Driver's license expires soon",
  "reminder_at": "2026-12-16T09:00:00Z",
  "status": "PENDING"
}
```

---

# 98. Example Notification Record

```json
{
  "id": "uuid",
  "user_id": "uuid",
  "type": "DOCUMENT_EXPIRATION",
  "title": "Document Expiring Soon",
  "message": "Your driver's license expires in 30 days.",
  "is_read": false
}
```

---

# 99. MVP Database Scope

The MVP database should implement only the following:

- Users
- Categories
- Tasks
- Bills
- Documents
- Reminders
- Notifications

### Explicitly Out of Scope

- Bank Integrations
- Payment Processing
- Family Sharing
- Vehicle Management
- AI
- Advanced Analytics
- External Calendar Integrations
- Actual Document File Storage
- Subscription Integrations

---

# 100. Database Design Decisions

| Decision | Choice |
|-----------|---------|
| Database | PostgreSQL |
| Primary Key | UUID |
| Timestamp | TIMESTAMPTZ |
| Date-only Values | DATE |
| Monetary Values | NUMERIC |
| Naming | snake_case |
| Tables | Plural Nouns |
| User Ownership | Required |
| Direct Mobile DB Access | Not Allowed |
| Database Migrations | Required |
| Production Backups | Required |
| Soft Delete | Selective |
| File Storage | Future |
| Search Engine | Not Required |
| Redis | Not Required |
| Microservices | Not Required |

---

# 101. Important Implementation Notes

The schema described in this document is the logical design.

Before creating the actual database, the implementation phase must still determine:

- Exact ORM
- Exact Migration Syntax
- Exact Enum Implementation
- Exact Foreign Key Deletion Behavior
- Exact Reminder Relationship Strategy
- Exact Validation Constraints
- Exact Index Definitions

These decisions should be reviewed during implementation rather than blindly copied.

---

# 102. Phase 6 Completion Criteria

Phase 6 is considered complete when:

- ✅ Core Entities are identified
- ✅ Users Table is designed
- ✅ Categories Table is designed
- ✅ Tasks Table is designed
- ✅ Bills Table is designed
- ✅ Documents Table is designed
- ✅ Reminders Table is designed
- ✅ Notifications Table is designed
- ✅ Primary Key Strategy is defined
- ✅ Foreign Key Strategy is defined
- ✅ Timestamp Strategy is defined
- ✅ Date Handling is defined
- ✅ Monetary Data Handling is defined
- ✅ User Data Isolation is defined
- ✅ Indexing Strategy is defined
- ✅ Migration Strategy is defined
- ✅ Backup Strategy is defined
- ✅ Security Considerations are defined
- ✅ Future Expansion is documented

---

# 103. Next Phase

## Phase 7 — API Specification

The next phase will translate the database and product requirements into a concrete API contract.

### We will define:

- Authentication Endpoints
- User Endpoints
- Task Endpoints
- Bill Endpoints
- Document Endpoints
- Reminder Endpoints
- Notification Endpoints
- Request Payloads
- Response Payloads
- HTTP Status Codes
- Error Responses
- Pagination
- Filtering
- Sorting
- Authorization Requirements

### Objective

> **"Exactly how will our React Native application communicate with our backend?"**
