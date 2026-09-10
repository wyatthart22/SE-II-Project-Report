# Career Prep Checklist — System Features

## Parent User Story

**As a** student,  
**when** I want to do activities that prepare me to get a job when I graduate,  
**I want** a checklist of tasks and events I can do each semester I am in school,  
**so that** I am prepared to successfully apply for a job.

---

## Decisions (from clarification)

| Topic | Decision |
| --- | --- |
| Users | Students, plus career services / instructors who maintain the official checklist, plus academic advisors who review progress |
| Completing items | Students check items off themselves. Staff can **verify selected official items** (for example, resume review or mock interview) |
| Advisor access | An advisor sees **only students assigned to them**. Career services / instructors create and change those assignments |

---

## System Overview

The system helps students plan and complete career-preparation work across every semester until graduation. It presents official tasks and events (resume work, career fairs, internships, networking, interviews) as a per-semester checklist, lets students add their own items, tracks self-completion, verifies selected high-impact items, and shows how ready the student is to apply for jobs.

Career services / instructors own the official catalog, campus events, resources, verification flags, and advisor–student assignments. Academic advisors coach **their assigned students** using the same progress and readiness views.

---

## Roles

| Role | Can do | Cannot do |
| --- | --- | --- |
| **Student** | Maintain academic profile; use semester checklists; check off items; add evidence; add custom items; see own progress, readiness, advisor name, and verification status | Edit the official catalog; verify items; view other students |
| **Career services / instructor** | Maintain official tasks, events, timing, and resources; mark items as requiring verification; verify or return any student’s flagged items; assign students to advisors | Use a student’s checklist as if they were that student |
| **Academic advisor** | View roster, checklists, and readiness for **assigned students only**; verify or return flagged items for those students; leave notes the student can see | View or advise students not assigned to them; change the official catalog; change advisor assignments |

---

## Feature 1: Student Profile and Academic Timeline

Students set up who they are and where they are in school so checklists match their remaining semesters.

### User Story 1.1 — Create academic profile

**As a** student,  
**I want** to enter my major, expected graduation term, and current year/semester,  
**so that** the system can show a plan that fits my remaining time in school.

### User Story 1.2 — Update profile when plans change

**As a** student,  
**I want** to update my graduation term or major,  
**so that** my checklist stays accurate if I change plans.

### User Story 1.3 — See remaining semesters

**As a** student,  
**I want** to see how many semesters I have left,  
**so that** I know how much time I have to complete career-prep activities.

---

## Feature 2: Semester Checklist

The core of the product: a checklist of official and custom tasks and events for each semester.

### User Story 2.1 — View checklist for the current semester

**As a** student,  
**I want** to see a checklist of tasks and events for my current semester,  
**so that** I know what to work on right now.

### User Story 2.2 — View checklists for other semesters

**As a** student,  
**I want** to browse past and future semester checklists,  
**so that** I can plan ahead and review what I have already done.

### User Story 2.3 — See recommended activities by academic stage

**As a** freshman, sophomore, junior, or senior,  
**I want** recommended tasks that match my year (for example, explore careers early, internships mid-program, applications near graduation),  
**so that** I am not overwhelmed with activities that are too early or too late.

### User Story 2.4 — Distinguish tasks from events

**As a** student,  
**I want** tasks (things I complete on my own) and events (career fairs, workshops, info sessions) shown separately or clearly labeled,  
**so that** I can plan both independent work and calendar-based activities.

### User Story 2.5 — See official vs custom items

**As a** student,  
**I want** official catalog items and items I added myself to be easy to tell apart, including which official items require verification,  
**so that** I know what the school expects and what is optional personal tracking.

---

## Feature 3: Task and Event Completion and Verification

Students mark work done. Career services / instructors choose which official items require verification. Those items are confirmed by career services, an instructor, or the student’s assigned advisor.

### User Story 3.1 — Mark an item complete

**As a** student,  
**I want** to check off a task or event when I finish it,  
**so that** I can see what I have already accomplished.

### User Story 3.2 — Undo a completion I marked

**As a** student,  
**I want** to uncheck an item I marked complete by mistake, as long as it has not been verified,  
**so that** my checklist stays accurate.

### User Story 3.3 — Add notes or evidence to a completed item

**As a** student,  
**I want** to attach a short note or link (for example, “uploaded resume to Handshake” or a workshop I attended),  
**so that** staff can review what I did and I remember it when I apply for jobs later.

### User Story 3.4 — See which items need staff verification

**As a** student,  
**I want** official items that require verification to be clearly labeled, along with whether they are not submitted, pending, verified, or returned,  
**so that** I know a checkmark alone is not enough for those activities.

### User Story 3.5 — Mark selected official items as requiring verification

**As a** career services staff member or instructor,  
**I want** to mark specific official tasks or events as “requires verification” (for example, resume review or mock interview),  
**so that** high-impact activities are confirmed, not only self-reported.

### User Story 3.6 — Verify any student’s flagged item (career services)

**As a** career services staff member or instructor,  
**I want** to verify or return (with a short reason) a student-completed item that requires verification,  
**so that** job-readiness reflects confirmed work.

### User Story 3.7 — Verify an assigned student’s flagged item (advisor)

**As an** academic advisor,  
**I want** to verify or return (with a short reason) flagged items for students assigned to me,  
**so that** I can confirm work I reviewed with that student and cannot act on students who are not mine.

### User Story 3.8 — Know when an item is verified or returned

**As a** student,  
**I want** to see on my checklist when staff verifies or returns an item,  
**so that** I can fix it or move on.

---

## Feature 4: Career-Prep Activity Catalog

A library of official tasks and events, maintained by career services / instructors, that the system places on semester checklists.

### User Story 4.1 — Browse recommended career-prep activities

**As a** student,  
**I want** to see a catalog of common activities (resume, LinkedIn, career fair, mock interview, internship search, thank-you notes),  
**so that** I understand the full set of things that help me get a job.

### User Story 4.2 — See why an activity matters

**As a** student,  
**I want** a short description of why each task or event helps my job search,  
**so that** I am motivated to complete it and can explain it on applications or in interviews.

### User Story 4.3 — See suggested timing

**As a** student,  
**I want** each activity to suggest which semester or year it belongs in,  
**so that** I do the right things at the right time.

### User Story 4.4 — Maintain the official catalog

**As a** career services staff member or instructor,  
**I want** to add, edit, archive, and assign official tasks and events to academic stages or semesters,  
**so that** every student sees a current, school-approved plan.

### User Story 4.5 — Publish campus events onto checklists

**As a** career services staff member or instructor,  
**I want** to add dated campus events (career fairs, workshops, employer info sessions) to the relevant semester,  
**so that** students see real opportunities on the same list as their tasks.

### User Story 4.6 — Attach resources to official items

**As a** career services staff member or instructor,  
**I want** to attach links, tips, and campus contacts to each official item,  
**so that** students can complete the work without hunting for information.

---

## Feature 5: Custom Tasks and Events

Students can add their own items, not only official catalog items. Custom items are self-checked only (they do not require staff verification).

### User Story 5.1 — Add a custom task to a semester

**As a** student,  
**I want** to add my own task to a semester checklist,  
**so that** I can track major-specific or personal goals (for example, a department poster session).

### User Story 5.2 — Add a custom event

**As a** student,  
**I want** to add an event with a date (career fair, company visit, club meeting),  
**so that** those opportunities appear on the same checklist as recommended items.

### User Story 5.3 — Edit or remove custom items

**As a** student,  
**I want** to edit or delete items I created,  
**so that** my checklist stays useful and uncluttered.

---

## Feature 6: Progress and Job-Readiness Overview

A summary of how prepared the student is to apply. Items that require verification count as ready only after they are verified.

### User Story 6.1 — See semester progress

**As a** student,  
**I want** to see how many checklist items I have completed this semester, including how many flagged items are still pending verification,  
**so that** I know if I am on track.

### User Story 6.2 — See overall progress toward graduation

**As a** student,  
**I want** to see progress across all semesters,  
**so that** I can tell whether I am building a strong record before I graduate.

### User Story 6.3 — See a job-application readiness summary

**As a** student,  
**I want** a simple readiness view (for example, resume done, internship experience, interview practice, applications started) that treats “requires verification” items as ready only after they are verified,  
**so that** I know whether I am prepared to successfully apply for a job.

---

## Feature 7: Advisor Progress Review

Academic advisors can see **only students assigned to them**. Career services / instructors manage those assignments.

### User Story 7.1 — Assign students to an advisor

**As a** career services staff member or instructor,  
**I want** to assign (and later reassign or unassign) students to an academic advisor,  
**so that** each student has someone who can review their progress.

### User Story 7.2 — See a roster of assigned students

**As an** academic advisor,  
**I want** to see only the students assigned to me and a snapshot of each student’s checklist progress (including pending verifications),  
**so that** I know who may need a conversation this semester and I cannot open other students’ records.

### User Story 7.3 — Open an assigned student’s semester checklists

**As an** academic advisor,  
**I want** to view an assigned student’s current and past semester checklists (official items, self-completion, and verification status),  
**so that** I can advise them on what to do next.

### User Story 7.4 — See an assigned student’s job-readiness summary

**As an** academic advisor,  
**I want** to see the same job-readiness summary the student sees,  
**so that** we can talk about gaps before they apply for jobs.

### User Story 7.5 — Leave advising notes on progress

**As an** academic advisor,  
**I want** to leave a short note on an assigned student’s plan (visible to that student),  
**so that** recommended next steps are recorded after a meeting.

### User Story 7.6 — See who my advisor is

**As a** student,  
**I want** to see the name of the academic advisor assigned to me,  
**so that** I know who can view my progress and who to talk to.

### User Story 7.7 — Know who can see my progress

**As a** student,  
**I want** to know that only my assigned academic advisor, plus career services / instructors who verify items, can view my career-prep checklist and progress,  
**so that** I am not surprised about who can see my work.

---

## Feature 8: Reminders and Upcoming Events

Help students act during the semester, not only at the end.

### User Story 8.1 — See upcoming events for this semester

**As a** student,  
**I want** a list of upcoming career events with dates,  
**so that** I can register and attend them on time.

### User Story 8.2 — Get reminded about incomplete priority items

**As a** student,  
**I want** reminders for incomplete high-priority tasks before the semester ends, including flagged items still pending verification,  
**so that** I do not miss important preparation steps.

---

## Feature 9: Resources Linked to Checklist Items

Official tasks point to help (templates, campus offices, how-tos) supplied in the catalog.

### User Story 9.1 — Open resources from a task

**As a** student,  
**I want** links or tips on a checklist item (resume template, career center hours, Handshake),  
**so that** I can complete the task without hunting for information.

### User Story 9.2 — Find campus career services from the app

**As a** student,  
**I want** to know how to contact career services or related campus offices from relevant tasks,  
**so that** I can get in-person help when I need it.

---

## Feature 10: Account Access and Roles

Users save work across sessions and only see what their role allows.

### User Story 10.1 — Create an account and sign in

**As a** student, career services staff member / instructor, or academic advisor,  
**I want** to create an account (or be given one) and sign in,  
**so that** my work and the correct permissions are saved.

### User Story 10.2 — Sign out securely

**As a** user,  
**I want** to sign out,  
**so that** others using the same device cannot see student progress or staff tools.

### User Story 10.3 — Recover access to my account

**As a** user,  
**I want** to reset my password if I forget it,  
**so that** I can get back to the system.

### User Story 10.4 — Use only the tools for my role

**As a** user,  
**I want** to see only the screens my role is allowed to use (student checklist, catalog admin, or advisor roster),  
**so that** students cannot change the official catalog, advisors cannot see unassigned students, and staff cannot act as the wrong role.

---

## Feature Traceability

| Feature | Supports parent goal |
| --- | --- |
| Student profile and academic timeline | Checklists match each semester the student is in school |
| Semester checklist | Tasks and events to do each semester |
| Completion and verification | Students work the list; selected official items are confirmed by staff |
| Activity catalog (staff-maintained) | Official, current, job-oriented activities—not a stale or blank list |
| Custom tasks and events | Student can include their own prep work |
| Progress and job-readiness | Confidence they are ready to apply; verified items count only when confirmed |
| Advisor progress review | Assigned advisors coach students; unassigned students stay private |
| Reminders and upcoming events | Activities happen during the semester, not after it is too late |
| Resources | Students can actually complete the items |
| Account access and roles | Progress persists; catalog, verification, and advising stay limited to the right people |

---

## Out of Scope (for this story)

The following are not required to fulfill the parent user story:

- Employer or recruiter accounts
- Automatic job applications or resume submission to employers
- Guaranteed job placement
- Degree planning, course registration, or full academic advising outside career-prep progress
- Advisors browsing or searching the entire student body
- Social networking between students (beyond optional event attendance)
- Career services or advisors applying to jobs on a student’s behalf
