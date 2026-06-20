# 🎓 Vidyanidhi University — Examination Management System
### Database Management System Design Project

---

## 📋 Project Overview

This project presents a complete **Database Management System (DBMS)** designed for **Vidyanidhi University** to automate and streamline its day-to-day examination activities — including scheduling, student registration, result declaration, fee management, and finance reporting.

The university conducts examinations across several affiliated colleges in a wide variety of subjects. Managing exam seat numbers, subject scheduling across multiple centres, section-wise marks, result declaration, and income/expenditure tracking is a highly complex operation. This system provides a normalized relational database design that addresses all of these needs in a scalable and flexible manner.

---

## 🏫 Academic Details

| Detail | Info |
|---|---|
| **University** | Vidyanidhi Institute of Technology |
| **Course Name** | MySQL and MongoDB |
| **Course Type** | Credit Transfer Course |
| **Project Type** | Final Project |

---

## 👨‍💻 Team Members

| Name | Roll No |
|---|---|
| Ali Mirza | 24102C0054 |
| Kasim Ghanchi | 24102C0015 |

---

## 📁 Repository Contents

```
📦 vidyanidhi-exam-dbms/
├── 📄 README.md                          ← You are here
├── 📊 Vidyanidhi_University_Database_Design.xlsx   ← Full table structure (all 22 tables)
├── 📑 Vidyanidhi_University_ER_Diagram.pdf         ← Entity Relationship Diagram
└── 📄 Vidyanidhi_Mermaid_Table_Definitions.pdf     ← Table definitions reference sheet
```

---

## 🗂️ System Scope

The DBMS covers the following functional areas of the university's examination system:

- **Academic Structure** — Colleges, Degrees, Subjects, Subject Sections (Theory/Practical), and the curriculum mapping between them
- **Examination Infrastructure** — Examination Centres, Exam Sessions, and Subject Scheduling (not every subject is held at every centre)
- **Student Management** — Student master, exam registration, seat number generation, and subject-wise enrollment
- **Marks & Results** — Section-wise marks entry, overall result declaration, grade and division computation
- **Answer Script Workflow** — Examiner assignment, script evaluation tracking, moderation flags, and revaluation requests
- **Staffing** — Invigilator duty assignment per exam schedule
- **Finance** — Fee structure definition, student fee payments (income), and exam-related expenditure tracking per session/centre

---

## 🗃️ Database Design

### Normalization
The schema is fully normalized to **Third Normal Form (3NF)**:
- **1NF** — All columns hold atomic values; no repeating groups (e.g. marks are stored row-wise per section, not as multiple columns)
- **2NF** — Every non-key column depends on the entire primary key (composite keys in bridge tables are correctly designed)
- **3NF** — No transitive dependencies; master data (college name, subject name, etc.) is never duplicated across transaction tables — only foreign keys are stored

### Entities (22 Tables)

| # | Table | Purpose |
|---|---|---|
| 1 | `COLLEGE` | Master of all affiliated colleges |
| 2 | `DEGREE` | Degree/programme master (B.Sc, M.A, B.Com, etc.) |
| 3 | `COLLEGE_DEGREE` | Bridge — which colleges offer which degrees (M:M) |
| 4 | `SUBJECT` | Master of all examinable subjects |
| 5 | `SUBJECT_SECTION` | Up to 2 sections per subject (Theory / Practical) with independent max marks |
| 6 | `DEGREE_SUBJECT` | Curriculum map — subjects per degree per semester (M:M) |
| 7 | `EXAMINATION_CENTRE` | Master of all exam centres |
| 8 | `EXAM_SESSION` | A named exam cycle (e.g. Summer 2026, Winter 2026 Backlog) |
| 9 | `EXAM_SCHEDULE` | Subject + Centre + Date + Time for a session (not every subject at every centre) |
| 10 | `INVIGILATOR` | Staff who invigilate at exam centres |
| 11 | `EXAMINER` | Staff who evaluate answer scripts |
| 12 | `STUDENT` | Master of every student under the university |
| 13 | `EXAM_FEE_STRUCTURE` | Fee rules per degree per session (base fee, per-subject fee, late fee) |
| 14 | `STUDENT_EXAM_REGISTRATION` | Student registering for a session — generates Exam Seat No |
| 15 | `STUDENT_SUBJECT_REGISTRATION` | Subjects a registered student opts to appear for |
| 16 | `SUBJECT_SECTION_MARKS` | Section-wise marks obtained by each student |
| 17 | `STUDENT_RESULT` | Declared overall result (%, division, pass/fail) per student per session |
| 18 | `FEE_PAYMENT` | Actual fee payment transactions (Finance — income) |
| 19 | `EXAM_EXPENDITURE` | Exam costs — remuneration, rent, printing (Finance — expenditure) |
| 20 | `EXAM_INVIGILATOR_ASSIGNMENT` | Invigilator duty roster per exam schedule slot |
| 21 | `ANSWER_SCRIPT_EVALUATION` | Examiner-wise evaluation record per section marks entry |
| 22 | `REVALUATION_REQUEST` | Student revaluation requests and decision tracking |

### Key Relationships
- `COLLEGE` ↔ `DEGREE` — **Many-to-Many** (resolved via `COLLEGE_DEGREE`)
- `DEGREE` ↔ `SUBJECT` — **Many-to-Many** (resolved via `DEGREE_SUBJECT`)
- `STUDENT_EXAM_REGISTRATION` → `STUDENT_RESULT` — **One-to-One**
- `SUBJECT_SECTION_MARKS` → `ANSWER_SCRIPT_EVALUATION` — **One-to-One**
- All other relationships — **One-to-Many**

---

## 📊 Deliverables

### 1. ER Diagram 
- Visual relational schema of all 22 entities
- Entities grouped into logical zones: Academic Structure, Exam Infrastructure, Students & Registration, Performance & Results, Finance, and Staffing
- PK (Primary Key), FK (Foreign Key) annotations on every table
- Cardinality labels (1 and M) on every relationship line

### 2. Table Structure Excel 
The Excel workbook contains four sheets:

| Sheet | Contents |
|---|---|
| **Overview** | Project scope, database summary, and table list with purpose |
| **Table Structures** | Column-level detail — data type, size, key type, nullable, FK reference, and description for all 22 tables |
| **Relationships** | Every parent-child relationship with cardinality and the business rule behind it (29 relationships total) |
| **Normalization** | How 1NF, 2NF, and 3NF were applied, plus key design decisions explained |

### 3. Mermaid Table Definitions
- Ready-to-use table definitions reference document
- Colour-coded: PK in red, FK in blue, Unique Keys in green
- Companion to the Mermaid ER diagram code

---

## 🧩 Mermaid ER Diagram Code

The relationships of the entire system can be visualized using the Mermaid diagram tool at [mermaid.live](https://mermaid.live). Paste the following code:

```
erDiagram
    COLLEGE ||--o{ COLLEGE_DEGREE : "offers"
    DEGREE ||--o{ COLLEGE_DEGREE : "offered by"
    DEGREE ||--o{ DEGREE_SUBJECT : "has curriculum"
    SUBJECT ||--o{ DEGREE_SUBJECT : "part of"
    SUBJECT ||--o{ SUBJECT_SECTION : "divided into"
    COLLEGE ||--o{ STUDENT : "enrolls"
    DEGREE ||--o{ STUDENT : "pursued by"
    STUDENT ||--o{ STUDENT_EXAM_REGISTRATION : "registers for"
    EXAM_SESSION ||--o{ STUDENT_EXAM_REGISTRATION : "has"
    STUDENT_EXAM_REGISTRATION ||--o{ STUDENT_SUBJECT_REGISTRATION : "opts subjects"
    EXAM_SCHEDULE ||--o{ STUDENT_SUBJECT_REGISTRATION : "assigned to"
    STUDENT_SUBJECT_REGISTRATION ||--o{ SUBJECT_SECTION_MARKS : "has marks"
    SUBJECT_SECTION ||--o{ SUBJECT_SECTION_MARKS : "records"
    STUDENT_EXAM_REGISTRATION ||--|| STUDENT_RESULT : "produces"
    STUDENT_EXAM_REGISTRATION ||--o{ FEE_PAYMENT : "pays"
    EXAM_SESSION ||--o{ EXAM_SCHEDULE : "schedules"
    EXAMINATION_CENTRE ||--o{ EXAM_SCHEDULE : "hosts"
    SUBJECT ||--o{ EXAM_SCHEDULE : "examined at"
    EXAM_SESSION ||--o{ EXAM_FEE_STRUCTURE : "defines"
    DEGREE ||--o{ EXAM_FEE_STRUCTURE : "applies to"
    EXAM_SESSION ||--o{ EXAM_EXPENDITURE : "incurs"
    EXAMINATION_CENTRE ||--o{ EXAM_EXPENDITURE : "charged to"
    EXAM_SCHEDULE ||--o{ EXAM_INVIGILATOR_ASSIGNMENT : "assigned"
    INVIGILATOR ||--o{ EXAM_INVIGILATOR_ASSIGNMENT : "duties"
    COLLEGE ||--o{ INVIGILATOR : "supplies"
    COLLEGE ||--o{ EXAMINER : "supplies"
    SUBJECT_SECTION_MARKS ||--|| ANSWER_SCRIPT_EVALUATION : "evaluated by"
    EXAMINER ||--o{ ANSWER_SCRIPT_EVALUATION : "evaluates"
    SUBJECT_SECTION_MARKS ||--o{ REVALUATION_REQUEST : "requested for"
```

---

## 📈 Reporting Coverage

The system is designed to support reports for all departments:

| Department | Reports Supported |
|---|---|
| **Academic** | Subject-wise pass %, topper list, grade distribution, individual student marksheet |
| **Administration** | Students per centre per subject, college-wise distribution, invigilator duty roster |
| **Finance** | Fee income per session/degree, expenditure per centre, income vs. expenditure summary |
| **Examination Cell** | Pending revaluation requests, script evaluation status, session-wise result summary |

---

## 🛠️ Technologies Referenced

| Area | Technology |
|---|---|
| Relational Database | MySQL |
| NoSQL Database | MongoDB |
| Diagram Tool | Graphviz / Mermaid |
| Documentation | Microsoft Excel, PDF |
