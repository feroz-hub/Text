Below is a **clear, structured set of Special Rules & Constraints** for **EduConnect**.  
These are **non‑functional rules, business constraints, and system boundaries** that keep the app **secure, correct, auditable, and scalable**.

Think of this as a **“School App Rulebook”** you can use for:

*   Architecture decisions
*   API validation
*   Security reviews
*   Product documentation

***

# EduConnect – Special Rules & Constraints

## 1. Identity & Access Rules (Hard Constraints)

### 1.1 Role-Based Access Is Mandatory

*   Every user **must have exactly one primary role**:
    *   `Parent`
    *   `Teacher`
    *   `Admin`
*   No API endpoint is accessible without authentication.
*   Authorization is enforced **server-side only**.

✅ **Constraint**  
Frontend checks are only for UX. **Backend is the source of truth.**

***

### 1.2 Parent–Student Binding Is Immutable

*   Each parent account is mapped to **only their child(ren)**.
*   Parents **cannot access**:
    *   Other students
    *   Other classes
    *   Teacher-only data

✅ **Rule**  
Parent data visibility is always filtered by `StudentId` linked in DB.

***

### 1.3 Roll Number Cannot Be a Global Login

*   Roll Number **cannot be used alone** for authentication.
*   Roll Number is **only an identifier**, not a credential.

✅ **Constraint**
OTP verification is mandatory for parent login.

***

## 2. Attendance (Absence Entry) Rules

### 2.1 One Absence Per Student Per Day

*   A student can have **only one attendance record per date**.

✅ Prevents duplicate or conflicting entries.

***

### 2.2 No Future Absence Entry

*   Parents **cannot mark absence for future dates**.
*   Admin can override only with justification.

***

### 2.3 Audit Trail Is Mandatory

Every absence entry must store:

*   Entered by (Parent/Admin)
*   Timestamp
*   Reason (optional for parent, mandatory for admin)

✅ **Constraint**
Attendance records are **append-only**, not deleted.

***

## 3. Homework Rules (Teacher Module)

### 3.1 Teacher Scope Limitation

*   Teachers can update homework **only for assigned classes/subjects**.
*   Teacher cannot view or modify:
    *   Other teachers’ homework
    *   Admin notices

✅ Prevents accidental data cross-over.

***

### 3.2 Homework Is Read-Only After Publish Window

*   Homework can be edited:
    *   Until end of same day
*   After that → **read-only**

✅ Ensures consistency for parents.

***

### 3.3 Homework Visibility Rule

*   Homework is visible only to:
    *   Parents of students in that class
    *   Admin

***

## 4. School Notices & Broadcasting Rules

### 4.1 Notices Are Immutable Once Published

*   Admins can:
    *   Edit **before publish**
    *   Not edit **after publish**
*   Corrections must be issued as a **new notice**

✅ Ensures trust & prevents silent changes.

***

### 4.2 Targeted Broadcasting

Each notice must define:

*   Target audience:
    *   All parents
    *   Specific class/section
*   Expiry date (optional)

✅ Prevents irrelevant notifications.

***

### 4.3 Guaranteed Delivery Recording

*   System must record:
    *   Published timestamp
    *   View/read status (optional enhancement)

✅ Enables accountability.

***

## 5. Authentication & Security Constraints

### 5.1 Token Rules

*   Access Token:
    *   Short-lived (≤ 15 minutes)
*   Refresh Token:
    *   HttpOnly
    *   Secure cookie
    *   Rotated on refresh

❌ **Never allowed**

*   JWT in LocalStorage
*   Long-lived access tokens

***

### 5.2 OTP Rules

*   OTP expiry: max 5 minutes
*   Max retry attempts: 3–5
*   Rate limiting mandatory

✅ Prevents brute force & abuse.

***

### 5.3 Secure Data Exposure

*   APIs must never return:
    *   Password hashes
    *   OTP values
    *   Internal identifiers not needed by UI

***

## 6. Data Integrity & Ownership Rules

### 6.1 Strong Ownership Enforcement

*   Every record must have an **owner context**:
    *   Student-based
    *   Teacher-based
    *   Admin-managed

✅ Ownership is validated in backend handlers, not controllers.

***

### 6.2 Soft Deletes for Critical Data

*   Attendance
*   Homework
*   Notices

✅ Data is never permanently deleted without admin intervention.

***

## 7. Architectural Constraints

### 7.1 No Business Logic in Frontend

*   All validations that matter must exist in backend.
*   Frontend validations are **cosmetic only**.

***

### 7.2 Feature Isolation Rule

*   Each feature:
    *   Owns its validation
    *   Owns its authorization checks
    *   Does not directly access other feature internals

✅ Supports Vertical Slice Architecture.

***

### 7.3 No Cross-Module Direct DB Access

*   Only Application layer may coordinate data.
*   Infrastructure is replaceable.

***

## 8. Performance & Reliability Constraints

### 8.1 Graceful Degradation

*   OTP failure → retry advice
*   Notification failure → logged but core action succeeds

✅ School operations must never be blocked.

***

### 8.2 Read Optimization

*   Homework & notices must be cached (short TTL).
*   Attendance always read from source of truth.

***

## 9. Compliance & Privacy Rules (Very Important)

### 9.1 Minimal Personal Data

*   Store only:
    *   Name
    *   Roll number
    *   Contact info
*   No unnecessary personal details.

***

### 9.2 Child Data Protection Principle

*   No public identifiers
*   No open URLs
*   No sharing outside authenticated context

✅ Parents trust is critical.

***

## 10. Operational Constraints

### 10.1 Manual Overrides Require Reason

*   Admin overrides must log:
    *   Reason
    *   Timestamp

***

### 10.2 System Clock Is Authority

*   All timestamps use server time.
*   Client time is ignored.

***

## 11. One-Line Summary (For Documentation)

> **EduConnect enforces strict role-based access, immutable academic records, secure identity verification, and auditable school communication to ensure trust, privacy, and operational efficiency.**

***

## ✅ If You Want Next

I can:

*   Convert this into a **“Non‑Functional Requirements (NFR)” document**
*   Map rules → API validations
*   Create a **Security & Compliance checklist**
*   Add **exception-handling rules**

Just tell me 👍

