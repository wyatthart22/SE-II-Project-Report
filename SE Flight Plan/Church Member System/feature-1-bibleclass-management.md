# Feature: Bible Class Management
 
**Feature ID:** 2
**Branch pattern:** `feature/2-bible-class-management`
**Status:** Draft
**Created:** 2026-09-11
**Input:** Organize bible classes by age group as the church adds more classes, and track who is enrolled in each.
**Depends on:** [Feature 1 — Member & Guest Attendance](feature-1-member-guest-attendance.md)
 
---
 
## User Stories
 
### US-2.1: Create a bible class
**As a** church staff member
**I want to** create a bible class with a name and an age range
**So that** I can organize classes as the church adds more of them
 
**Priority:** P1
**Independent test:** Create a class with a name and age range; it appears in the class list
**Acceptance scenarios:** see ### US-2.1 under Acceptance Criteria
 
### US-2.2: Enroll a person in a class by age
**As a** church staff member
**I want to** assign a member or guest to the bible class matching their age
**So that** people are grouped with the right age group
 
**Priority:** P1
**Independent test:** Enroll a person whose age falls in a class's range; they appear on the class roster
**Acceptance scenarios:** see ### US-2.2 under Acceptance Criteria
 
### US-2.3: View a class roster
**As a** church staff member
**I want to** see everyone enrolled in a given bible class
**So that** teachers know who to expect
 
**Priority:** P1
**Independent test:** Open a class and see the list of enrolled people
**Acceptance scenarios:** see ### US-2.3 under Acceptance Criteria
 
### US-2.4: Edit or delete a class
**As a** church staff member
**I want to** rename, change the age range of, or remove a bible class
**So that** the class list stays accurate as the program grows
 
**Priority:** P2
**Independent test:** Edit a class's age range and confirm it updates
**Acceptance scenarios:** see ### US-2.4 under Acceptance Criteria
 
---
 
## Requirements
 
### Functional Requirements
 
- **FR-001**: System MUST allow staff to create a bible class with a name and a minimum and maximum age.
- **FR-002**: System MUST allow staff to enroll any existing person (member or guest) from Feature 1 into a class.
- **FR-003**: System MUST warn staff, but not block, when enrolling a person whose age falls outside the class's age range, since exceptions happen.
- **FR-004**: System MUST list all people enrolled in a class as that class's roster.
- **FR-005**: System MUST allow staff to remove a person from a class.
- **FR-006**: System MUST allow staff to edit a class's name and age range.
- **FR-007**: System MUST allow staff to delete a class; deleting a class MUST also remove its enrollments.
- **FR-008**: Class name MUST NOT be blank when saving.
---
 
## Initial Data Model
 
### Key Entities
 
- **BibleClass**: a class with a name and target age range (e.g. "Toddlers", ages 1–3).
- **ClassEnrollment**: links one Person (Feature 1) to one BibleClass.
### Data Model Requirements
 
#### `bible_classes` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `name` | STRING(100) | Required |
| `minAge` | INTEGER | Required |
| `maxAge` | INTEGER | Required; MUST be >= `minAge` |
| `createdAt` | DATE | Auto-set |
| `updatedAt` | DATE | Auto-set |
 
#### `class_enrollments` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `classId` | INTEGER FK | Required; references `bible_classes.id` |
| `personId` | INTEGER FK | Required; references `people.id` |
| `createdAt` | DATE | Auto-set |
 
Unique constraint on (`classId`, `personId`) to prevent duplicate enrollment.
 
---
 
## Acceptance Criteria
 
### US-2.1 — Create a bible class
 
#### Scenario: Staff creates a new class
*   **Given** staff is on the add-class form
*   **When** staff enters name `Elementary` with age range `6`–`10`
*   **And** staff saves
*   **Then** a new bible class `Elementary` is created
*   **And** `Elementary` appears in the class list
#### Scenario: Staff attempts to save a class with an invalid age range
*   **Given** staff is on the add-class form
*   **When** staff enters a minimum age greater than the maximum age
*   **And** staff attempts to save
*   **Then** the save is blocked
*   **And** staff sees a message that the age range is invalid
### US-2.2 — Enroll a person in a class by age
 
#### Scenario: Staff enrolls a person whose age matches the class
*   **Given** class `Elementary` has age range `6`–`10`
*   **And** person `Sam Lee`, age `8`, exists in the directory
*   **When** staff enrolls `Sam Lee` in `Elementary`
*   **Then** `Sam Lee` appears on the `Elementary` roster
#### Scenario: Staff enrolls a person whose age falls outside the class range
*   **Given** class `Elementary` has age range `6`–`10`
*   **And** person `Baby Lee`, age `2`, exists in the directory
*   **When** staff enrolls `Baby Lee` in `Elementary`
*   **Then** staff sees a warning that the age is outside the class range
*   **And** staff can confirm the enrollment anyway
### US-2.3 — View a class roster
 
#### Scenario: Roster shows all enrolled people
*   **Given** `Sam Lee` and `Jane Doe` are both enrolled in `Elementary`
*   **When** staff views the `Elementary` class
*   **Then** both `Sam Lee` and `Jane Doe` appear on the roster
### US-2.4 — Edit or delete a class
 
#### Scenario: Staff edits a class's age range
*   **Given** class `Elementary` has age range `6`–`10`
*   **When** staff changes the range to `6`–`11`
*   **And** staff saves
*   **Then** the class's stored age range is `6`–`11`
#### Scenario: Staff deletes a class
*   **Given** class `Elementary` has enrolled people
*   **When** staff deletes `Elementary`
*   **Then** the class no longer appears in the class list
*   **And** its enrollment records are removed
