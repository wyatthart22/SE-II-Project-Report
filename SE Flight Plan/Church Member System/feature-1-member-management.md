# Feature: Member & Guest Management
 
**Feature ID:** 1
**Branch pattern:** `feature/1-member-guest-management`
**Status:** Draft
**Created:** 2026-09-11
**Input:** Replace paper attendance cards with digital headcount tracking and a searchable directory of member and guest data.
 
---
 
## User Stories
 
### US-1.1: Record service attendance
**As a** church staff member
**I want to** check in members and guests at each service
**So that** the church has an accurate headcount for every service
 
**Priority:** P1
**Independent test:** Open a service, check in a person, and see the headcount increase by one
**Acceptance scenarios:** see ### US-1.1 under Acceptance Criteria
 
### US-1.2: Register a new member or guest
**As a** church staff member
**I want to** add a new person's name, family, address, member status, birthday, and gender
**So that** the church has a record for everyone who attends, not just paper cards
 
**Priority:** P1
**Independent test:** Fill out and save a new-person form; the person appears in the directory
**Acceptance scenarios:** see ### US-1.2 under Acceptance Criteria
 
### US-1.3: View headcount per service
**As a** church staff member
**I want to** see the total headcount, and the member vs. guest breakdown, for a given service
**So that** leadership can track attendance trends over time
 
**Priority:** P1
**Independent test:** After checking in several people to one service, the service's headcount total is correct
**Acceptance scenarios:** see ### US-1.3 under Acceptance Criteria
 
### US-1.4: Search the member and guest directory
**As a** church staff member
**I want to** search and filter people by name, family, or member status
**So that** I can find someone's information quickly instead of digging through paper cards
 
**Priority:** P1
**Independent test:** Search by a known name and see the matching person's record
**Acceptance scenarios:** see ### US-1.4 under Acceptance Criteria
 
### US-1.5: Edit a person's information
**As a** church staff member
**I want to** update a person's address, birthday, gender, family, or member status
**So that** records stay accurate as people's information changes
 
**Priority:** P2
**Independent test:** Edit an existing person's address and see the change persist
**Acceptance scenarios:** see ### US-1.5 under Acceptance Criteria
 
---
 
## Requirements
 
### Functional Requirements
 
- **FR-001**: System MUST allow staff to create a "service" record identified by a date and service type (e.g. Sunday morning, Wednesday night).
- **FR-002**: System MUST allow staff to check a person into a service, creating one attendance record per person per service.
- **FR-003**: System MUST NOT allow duplicate attendance records for the same person at the same service.
- **FR-004**: System MUST capture, at minimum, first name, last name, family name, address, member status (member or guest), birthday, and gender when a person is created.
- **FR-005**: System MUST distinguish members from guests via a persistent `isMember` flag on the person record.
- **FR-006**: System MUST compute and display total headcount, member count, and guest count for any given service.
- **FR-007**: System MUST allow staff to search people by name and filter by member status.
- **FR-008**: System MUST allow staff to edit an existing person's stored fields.
- **FR-009**: Required fields (first name, last name) MUST NOT be blank when saving a person.
---
 
## Initial Data Model
 
### Key Entities
 
- **Person**: a member or guest; has a name, family grouping, address, birthday, gender, and member status. Shared entity referenced by Bible Classes (Feature 2) and Van Routes (Feature 3).
- **Service**: a single church gathering on a specific date (e.g. "Sunday AM, 2026-09-13").
- **Attendance**: a record linking one Person to one Service, representing that they were checked in.
### Data Model Requirements
 
#### `people` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `firstName` | STRING(100) | Required |
| `lastName` | STRING(100) | Required |
| `familyName` | STRING(100) | Optional; groups people into a household |
| `address` | STRING(255) | Optional |
| `birthday` | DATE | Optional |
| `gender` | STRING(20) | Optional |
| `isMember` | BOOLEAN | Required; default `false` (guest) |
| `createdAt` | DATE | Auto-set |
| `updatedAt` | DATE | Auto-set |
 
#### `services` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `date` | DATE | Required |
| `serviceType` | STRING(50) | Required (e.g. "Sunday AM", "Wednesday PM") |
| `createdAt` | DATE | Auto-set |
 
#### `attendance` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `personId` | INTEGER FK | Required; references `people.id` |
| `serviceId` | INTEGER FK | Required; references `services.id` |
| `checkedInAt` | DATE | Auto-set on check-in |
 
Unique constraint on (`personId`, `serviceId`) to prevent duplicate check-ins.
 
---
 
## Acceptance Criteria
 
### US-1.1 — Record service attendance
 
#### Scenario: Staff checks a member into a service
*   **Given** a service exists for today
*   **And** a member `Jane Doe` exists in the directory
*   **When** staff checks `Jane Doe` into today's service
*   **Then** an attendance record is created linking `Jane Doe` to today's service
*   **And** today's headcount increases by one
#### Scenario: Staff attempts to check the same person in twice
*   **Given** `Jane Doe` is already checked into today's service
*   **When** staff attempts to check `Jane Doe` into today's service again
*   **Then** no duplicate attendance record is created
*   **And** staff sees a message indicating she is already checked in
### US-1.2 — Register a new member or guest
 
#### Scenario: Staff registers a new guest
*   **Given** staff is on the add-person form
*   **When** staff enters first name `Sam`, last name `Lee`, family `Lee Family`, address `123 Main St`, birthday, gender, and leaves member status as guest
*   **And** staff saves the form
*   **Then** a new person record is created with `isMember = false`
*   **And** `Sam Lee` appears in the directory
#### Scenario: Staff attempts to save a person with no name
*   **Given** staff is on the add-person form
*   **When** staff leaves first and last name blank
*   **And** staff attempts to save
*   **Then** the save is blocked
*   **And** staff sees a message that name is required
### US-1.3 — View headcount per service
 
#### Scenario: Headcount reflects checked-in people
*   **Given** 3 members and 2 guests are checked into today's service
*   **When** staff views today's service
*   **Then** the total headcount shows `5`
*   **And** the breakdown shows `3` members and `2` guests
### US-1.4 — Search the member and guest directory
 
#### Scenario: Staff searches by name
*   **Given** a person named `Sam Lee` exists in the directory
*   **When** staff searches `Lee`
*   **Then** `Sam Lee` appears in the search results
#### Scenario: Staff filters by member status
*   **Given** the directory contains both members and guests
*   **When** staff filters by "Members only"
*   **Then** only people with `isMember = true` are shown
### US-1.5 — Edit a person's information
 
#### Scenario: Staff updates a person's address
*   **Given** `Sam Lee` exists with address `123 Main St`
*   **When** staff edits `Sam Lee`'s address to `456 Oak Ave`
*   **And** staff saves
*   **Then** `Sam Lee`'s record shows the updated address
