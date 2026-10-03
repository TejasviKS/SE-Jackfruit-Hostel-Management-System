# Jackfruit Phase-1 
 
## Software Requirements Specification 
### for 
### Hostel Management System 
 
**Version 1.0** 
**Prepared by:** <Member 1 Tejasvi K S – PES1UG24CS499> / <Member 2 Prathiksha – SRN> / <Member 3 Vilohith – SRN> / <Member 4 Varsha – SRN> 
**Group Number:** <G-03>  |  **Project ID:** <HMS-2026-XX> 
**Course:** UE24CS341A – Software Engineering, V Semester 
**Organization:** PES University, Bangalore — Dept. of CSE 
**Date Created:** <16/9/26> 
 
--- 
 
## Table of Contents 
 
- 1. Introduction 
- 2. Overall Description 
- 3. External Interface Requirements 
- 4. Analysis Models 
- 5. System Features 
  - 5.1 Student & Access Management *(Owner: P1)* 
  - 5.2 Room & Allocation Management *(Owner: P2)* 
  - 5.3 Fee Management *(Owner: P3)* 
  - 5.4 Complaints & Leave Management *(Owner: P4)* 
- 6. Other Nonfunctional Requirements 
- 7. Other Requirements 
- Appendix A: Glossary 
- Appendix B: Field Layouts 
- Appendix C: Requirement Traceability Matrix 
 
--- 
 
| Name | Date | Reason For Changes | Version | 
|Tejasvi K S| 16/9/26 | Initial SRS scope and document | 0.1 | 
 
 
--- 
 
# 1. Introduction 
**Owner: P1 (Student & Access Management)** 
 
## 1.1 Purpose 
- State what this document is: the SRS for the Hostel Management System, version 1.0. 
- 2–3 lines on what the product is — e.g. *"a web application (Django) that manages student 
  information, hostel room allocation, fee payment records, complaints, and leave requests for a 
  single hostel."* 
 
## 1.2 Intended Audience and Reading Suggestions 
- Readers: your team (developers), subject teacher/evaluator, testers. 
- One line on how the rest of the SRS is organized (Overall Description → Interfaces → Analysis 
  Models → System Features → Nonfunctional Requirements → Appendices). 
 
## 1.3 Product Scope 
- Short paragraph: what the software does, who it's for, and why — e.g. replacing a manual 
  register/notebook system for room allocation and fee tracking with a single web application. 
- Explicitly note: two user roles only (Student, Admin);  
 
## 1.4 References 
- Course guidelines document (Jackfruit — SE Project Guidelines 2026). 
- Django/Flask documentation, if referenced for design constraints. 
- If nothing else, write "Not applicable" — don't leave blank. 
 
--- 
 
# 2. Overall Description 
**Owner: P3 (Fee Management)** 
 
## 2.1 Product Perspective 
The Hostel Management System is a new, self-contained, standalone web application 
developed specifically for this course project. It is not a follow-on to, or 
replacement of, any existing production system, and it does not integrate with any 
larger institutional system. It is being built as a mini-project for the Software 
Engineering course (UE24CS341A) and is scoped as a demonstration system, not a 
production deployment. 
 
## 2.2 Product Functions 
- Student registration, login, and profile management 
- Room and block setup, and room allocation/deallocation 
- Fee structure setup and payment record tracking 
- Complaint filing/tracking and leave request/approval 
 
## 2.3 User Classes and Characteristics 
| User Class | Privileges | Characteristics | 
|---|---|---| 
| Student | Register, view/edit own profile, view room, view fee dues, file complaints, submit leave requests | Non-technical, occasional use | 
| Admin | Verify student accounts, manage rooms, allocate/deallocate rooms, define fee structure, record payments, manage complaints, approve/reject leave | Higher privilege, trained user | 
 
*(Only two user classes.)* 
 
## 2.4 Operating Environment 
The system is a web application accessed through any modern browser (Chrome, 
Firefox, Edge). It is built using Python 3.x with the Django web framework, and 
uses PostgreSQL as the backend database. It is developed and demonstrated locally (and later via Docker), with no additional client-side installation required beyond a 
browser. 
 
## 2.5 Design and Implementation Constraints 
- Must be built using Python (Django), per the team's chosen tech stack. 
- Single hostel/single campus scope — no multi-hostel support. 
- Two user roles only: Student and Admin. No payment gateway integration — fee 
  payments are admin-recorded entries, not processed transactions. 
- Agile/Scrum methodology mandatory; GitHub used for backlog, issues, and pull 
  requests, with CI running tests on every PR 
 
## 2.6 Assumptions and Dependencies 
- Assumes an admin verifies new student accounts before a room can be allocated to 
  them. 
- Assumes fee payments happen outside the system (cash/bank transfer) and are 
  simply recorded by the admin — the system does not process, authorize, or 
  validate any actual payment transaction. 
- Depends on Django's built-in authentication and ORM; no other major third-party 
  service is required for this scope. 
 
--- 
 
# 3. External Interface Requirements 
**Owner: P2 (Room & Allocation Management)** 
 
## 3.1 User Interfaces 
- Describe main screens: Student dashboard, Room/allocation screen, Fee details screen, 
  Complaint/Leave screen, Admin dashboard. 
- Formatting conventions to keep consistent across the app (error message style, status badges, 
  form validation messages). 
- Optional: a simple text/wireframe mock-up of one key screen. 
 
## 3.2 Software Interfaces 
- Backend framework + version, DB engine + version, ORM used (Django ORM). 
- Confirm: no external payment gateway library/SDK is used in this version. 
 
## 3.3 Communications Interfaces 
- REST API / HTTP between frontend and backend if applicable, or plain Django templates 
  (state which architecture your team is using). 
- Email notification interface, if implemented — otherwise state "Not applicable." 
 
## 3.4 Hardware Interfaces 
- Standard: browser + keyboard/mouse only, no special hardware. State this briefly. 
 
--- 
 
# 4. Analysis Models 
**Owner: P2** 
 
- Include a use case diagram covering both actors (Student, Admin) and the use cases listed in 
  Section 5 / the Use Cases table below. 
- Include an ER diagram covering core entities: `Student`, `Room`, `Block`, `Fee`, 
  `PaymentRecord`, `Complaint`, `LeaveRequest`. No `Visitor` entity, no `RoomChangeRequest` entity. 
- Draw.io/PlantUML export as an image, inserted here, is fine. 
 
--- 
 
# 5. System Features 
 
> **Shared vocabulary — do not rename these across sections, and do not reintroduce removed 
> terms:** 
> `Student`, `Admin`, `Room`, `Block`, `Fee`, `PaymentRecord`, `Complaint`, `LeaveRequest`, `RBAC`. 
 
 
## 5.1 Student & Access Management 
**Owner: P1** 
 
### Description and Priority 
Student registration (with admin verification), login for both roles, role-based access control, 
student profile management. **Priority: High** — every other feature depends on this. 
 
### Stimulus/Response Sequences 
- *"Student submits registration form → account created in 'Pending' state → Admin verifies → 
  account activated → student can log in."* 
- Cover invalid input: *"User submits invalid/duplicate email → validation error shown → form 
  redisplayed."* 
 
### Functional Requirements 
| ID | Requirement | Priority | Verification | 
|---|---|---|---| 
| REQ-1 | The system shall allow a student to register using a unique email/SRN. | High | Functional test | 
| REQ-2 | The system shall keep newly registered accounts in a "Pending" state until admin-verified. | High | Functional test | 
| REQ-3 | The system shall allow an admin to approve or reject a pending registration. | High | Functional test | 
| REQ-4 | The system shall enforce role-based access so students cannot access admin-only views. | High | Authorization test | 
| REQ-5 | The system shall allow a student to view and edit their own profile (contact, guardian info). | Medium | Functional test | 
 
--- 
 
## 5.2 Room & Allocation Management 
**Owner: P2** 
 
### Description and Priority 
Room/block setup, capacity and availability tracking, direct allocation and deallocation by the 
admin. **Priority: High.** *(No room-change-request workflow in this version — if a student needs 
a different room, the admin deallocates and reallocates directly.)* 
 
### Stimulus/Response Sequences 
- *"Admin allocates an available room to a verified student → room status updates to 'Occupied' → 
  student sees their room on their dashboard."* 
- *"Admin deallocates a student from a room (e.g. on checkout) → room status updates to 
  'Vacant'."* 
 
### Functional Requirements 
| ID | Requirement | Priority | Verification | 
|---|---|---|---| 
| REQ-6 | The system shall allow an admin to define blocks and rooms with a maximum capacity. | High | Functional test | 
| REQ-7 | The system shall prevent allocation of a room beyond its defined capacity. | High | Negative test | 
| REQ-8 | The system shall allow an admin to allocate an available room to a verified student. | High | Functional test | 
| REQ-9 | The system shall allow an admin to deallocate a student from their room. | High | Functional test | 
| REQ-10 | The system shall update room occupancy status automatically on allocation/deallocation. | High | Integration test | 
 
--- 
 
## 5.3 Fee Management 
**Owner: P3** 
 
### Description and Priority 
Fee structure setup and payment record tracking, expressed purely as amounts (no gateway, no 
receipts). **Priority: High.** 
 
### Stimulus/Response Sequences 
- *"Admin sets a fee amount for a student/room type → student sees the amount due → admin records 
  a payment entry → system recalculates and displays the outstanding balance."* 
 
### Functional Requirements 
| ID | Requirement | Priority | Verification | 
|---|---|---|---| 
| REQ-11 | The system shall allow an admin to define a fee amount for a student or room type. | High | Functional test | 
| REQ-12 | The system shall allow an admin to record a fee payment entry (amount, date) against a student. | High | Functional test | 
| REQ-13 | The system shall calculate the outstanding fee amount as fee due minus total recorded payments. | High | Unit test | 
| REQ-14 | The system shall prevent a recorded payment amount from exceeding the outstanding balance. | High | Negative test | 
| REQ-15 | The system shall allow a student to view their fee amount, payment history, and outstanding balance. | Medium | Functional test | 
| REQ-16 | The system shall allow an admin to view a report of outstanding dues across all students. | Medium | Functional test | 
 
--- 
 
## 5.4 Complaints & Leave Management 
**Owner: P4** 
 
### Description and Priority 
Maintenance complaint filing/tracking, and student leave/outing request with admin approval. 
**Priority: Medium** — supporting operational features. 
 
### Stimulus/Response Sequences 
- *"Student files a complaint → complaint enters 'Open' → admin updates status to 
  'In-progress'/'Resolved' → student sees updated status."* 
- *"Student submits a leave request → admin approves/rejects → student sees the decision."* 
 
### Functional Requirements 
| ID | Requirement | Priority | Verification | 
|---|---|---|---| 
| REQ-17 | The system shall allow a student to file a maintenance complaint tied to their room. | High | Functional test | 
| REQ-18 | The system shall allow an admin to update complaint status (Open/In-progress/Resolved). | High | Functional test | 
| REQ-19 | The system shall allow a student to submit a leave/outing request with a date range. | High | Functional test | 
| REQ-20 | The system shall allow an admin to approve or reject a leave request. | High | Functional test | 
 
--- 
 
# 6. Other Nonfunctional Requirements 
**Owner: P4** 
 
## 6.1 Performance Requirements 
- The performance requirements will be finalized during the design phase 
 
## 6.2 Safety Requirements 
- The safety requirements will be finalized during the design phase 
 
## 6.3 Security Requirements 
| ID | Security Requirement | Validation Approach | 
|---|---|---| 
| SEC-1 | Passwords shall not be stored in plaintext. | Code/config review | 
| SEC-2 | Role-based access shall prevent students from accessing admin-only endpoints. | Authorization tests | 
| SEC-3 | The system shall validate and sanitize all user-supplied input before processing. | Negative/security tests | 
| SEC-4 | Authenticated APIs shall reject expired/invalid session tokens. | Security test | 
 
 
## 6.4 Software Quality Attributes 
- Pick 2–3 and make them specific: Usability, Reliability (invalid input must never crash the 
  system), Maintainability (business logic separated into modules with unit tests). 
 
## 6.5 Business Rules 
- *"Only verified students can be allocated a room."* 
- *"A room cannot be allocated beyond its defined capacity."* 
- *"Only an admin can approve or reject a leave request."* 
- *"A recorded payment cannot exceed a student's outstanding fee balance."* 
- *"Only an admin can allocate or deallocate a room."* 
 
--- 
 
# 7. Other Requirements 
**Owner: P4** 
 
- Database requirements, legal/data-retention notes, anything else not covered above. "Not 
  applicable" is fine if there's genuinely nothing extra. 
 
--- 
 
## Appendix A: Glossary 
**Owner: P4** 
 
Define every term used across the document — check with all teammates for terms that need 
defining. 
 
| Term | Definition | 
|---|---| 
| Student | A registered user who lives in or has applied to live in the hostel. | 
| Admin | A privileged user who manages rooms, fees, complaints, and leave requests. | 
| Room | A physical hostel room with a defined capacity, belonging to a block. | 
| Block | A group of rooms (e.g. a building or wing) within the hostel. | 
| Fee | The amount due from a student for their hostel stay. | 
| PaymentRecord | An admin-entered record of an amount paid by a student toward their fee. | 
| Complaint | A maintenance/issue report filed by a student against their room. | 
| LeaveRequest | A student's request to be away from the hostel for a date range. | 
| RBAC | Role-Based Access Control — restricting actions based on user role (Student/Admin). | 
 
 
## Appendix B: Field Layouts 
**Owner: P4** 
 
Document exact field layouts for key entities once finalized, e.g.: 
 
| Field | Length | Data Type | Description | Mandatory | 
|---|---|---|---|---| 
| student_id | 36 | UUID/String | Unique student identifier | Y | 
| email | 100 | String | Unique login email | Y | 
| room_id | 36 | UUID/String | Unique room identifier | Y | 
| room_status | 20 | Enum/String | Vacant / Occupied | Y | 
| fee_amount | 10,2 | Decimal | Total fee due for the student | Y | 
| payment_amount | 10,2 | Decimal | Amount recorded in a single payment entry | Y | 
| payment_date | 8 | Date | Date the payment was recorded | Y | 
| complaint_status | 20 | Enum/String | Open / In-progress / Resolved | Y | 
| leave_status | 20 | Enum/String | Pending / Approved / Rejected | Y | 
 
 
## Appendix C: Requirement Traceability Matrix 
**Owner: P4 — fill in last, after all 4 PRs are merged and Phase 2/3 begin** 
 
| Sl. No | Req ID | Brief Description | Architecture Ref | Design Ref | Code File Ref | Test Case ID | System Test ID | 
|---|---|---|---|---|---|---|---| 
| 1 | REQ-1 | | | | | | | 
| 2 | REQ-6 | | | | | | | 
| 3 | REQ-11 | | | | | | | 
| 4 | REQ-17 | | | | | | | 
 
--- 
 
## Use Cases (reference list for the Analysis Models section) 
 
| Use Case ID | Use Case | Primary Actor | Precondition | Outcome | 
|---|---|---|---|---| 
| UC-01 | Register / Login | Student | Application available | Authenticated session | 
| UC-02 | Manage Student Profile | Student | Logged in | Profile updated | 
| UC-03 | Verify Student Registration | Admin | Admin authenticated | Account activated/rejected | 
| UC-04 | Manage Rooms & Blocks | Admin | Admin authenticated | Room/block created or updated | 
| UC-05 | Allocate Room | Admin | Room available, student verified | Room assigned | 
| UC-06 | Deallocate Room | Admin | Student currently allocated | Room vacated | 
| UC-07 | View Fee Details | Student | Logged in | Fee amount + outstanding balance shown | 
| UC-08 | Record Fee Payment | Admin | Admin authenticated | Payment recorded, balance updated | 
| UC-09 | Submit Complaint | Student | Logged in, allocated a room | Complaint created | 
| UC-10 | Manage Complaint Status | Admin | Admin authenticated | Complaint status updated | 
| UC-11 | Submit Leave Request | Student | Logged in | Leave request created | 
| UC-12 | Approve / Reject Leave | Admin | Admin authenticated | Leave request status updated | 
 
---  
