# Feature: Referee Availability and Assignments

**Feature ID:** 6
**Branch pattern:** `feature/6-referee-assignments`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Maintain referee contact details, referee types, and availability; assign referees to games and replace unavailable referees.

## User Stories

### US-6.1: Maintain referee availability
**As a** league owner
**I want to** record referee types and when each referee can work
**So that** I can select suitable officials for games
**Priority:** P1
**Independent test:** Create a referee with email, a referee type, and an availability interval, then retrieve eligible officials for a game.
**Acceptance scenarios:** See US-6.1 under Gherkin AC.

### US-6.2: Assign and replace an official
**As a** league owner
**I want to** assign an available referee to a game and replace one who cannot work
**So that** games retain the officials they need
**Priority:** P1
**Independent test:** Assign a qualified available referee, mark that assignment unavailable, and choose a valid replacement.
**Acceptance scenarios:** See US-6.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST maintain referees with names, private email addresses when available, supported referee types, and availability intervals.
- **FR-002:** System MUST associate a referee assignment with a game, a referee, and the role/type the referee will perform.
- **FR-003:** System MUST prevent assignment outside the referee's recorded availability, assignment to an unsupported referee type, and overlapping active assignments for the same referee.
- **FR-004:** System MUST let the owner record that an assigned referee cannot work, retain the old assignment as withdrawn, and select an eligible replacement without changing the game's schedule.
- **FR-005:** System MUST show unfilled roles and withdrawn assignments as requiring attention; withdrawal MUST NOT imply that a replacement has already been found.
- **FR-006:** System MUST expose current and changed assignments for the owner's email workflow in Feature 8, but MUST NOT send mail merely because an assignment is changed.
- **FR-007:** System MUST retain assignment history so a replacement does not erase who was originally assigned.
- **FR-008:** System MUST require one center referee and two assistant referees for each soccer game. Professor North maintains referee qualifications and assignments. Referees may edit their own future availability, but a change overlapping an assignment MUST flag it for owner review rather than withdraw it automatically. Only Professor North may withdraw and replace an official. An assignment MUST fit the availability interval and leave at least thirty minutes between that referee's games. Official requirements for other sports are owner-configured before assigning their games.

## Key Entities

- **Referee:** a person who can officiate with recorded contact and supported types.
- **RefereeType:** an officiating qualification or role applicable to a sport.
- **RefereeAvailability:** an interval in which a referee can work.
- **RefereeAssignment:** a role assigned to a referee for a game, retaining withdrawal/replacement history.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Referee(referee_id PK, user_id FK User NULL, display_name, email NULL, active).
- RefereeType(referee_type_id PK, sport_id FK Sport, name). RefereeQualification(referee_id FK Referee, referee_type_id FK RefereeType); unique pair.
- RefereeAvailability(availability_id PK, referee_id FK Referee, starts_at DATETIME, ends_at DATETIME); ends_at > starts_at.
- RefereeAssignment(assignment_id PK, game_id FK Game, referee_id FK Referee, referee_type_id FK RefereeType, state ACTIVE|WITHDRAWN, replaces_assignment_id FK RefereeAssignment NULL, assigned_by FK User, changed_at DATETIME). Add role_slot CENTER|ASSISTANT_1|ASSISTANT_2 for soccer; unique active (game_id, role_slot). Each slot maps to its required referee type.
- Assignment times are read from the game's authoritative published schedule; a game-time change invalidates conflicting assignments for review rather than silently treating them as valid.

## Gherkin AC

### US-6.1: Maintain referee availability

#### Scenario: Find an eligible official
- **Given** a referee qualified for the required type and available for the entire game interval
- **When** the owner views eligible referees for that game
- **Then** the referee appears as eligible when no other active assignment overlaps

#### Scenario: Reject an unavailable or unqualified referee
- **Given** a referee outside the game interval or lacking its required type
- **When** the owner attempts the assignment
- **Then** the assignment is rejected with the reason

### US-6.2: Assign and replace an official

#### Scenario: Replace a referee who cannot work
- **Given** an active assignment whose referee reports being unable to work
- **When** the owner withdraws it and selects an eligible replacement
- **Then** the replacement becomes active and the withdrawn assignment remains in history

#### Scenario: Prevent a double booking
- **Given** a referee already assigned to an overlapping game
- **When** the owner attempts another active assignment
- **Then** the conflict is shown and the second assignment is not saved

#### Scenario: Flag a newly conflicting game time
- **Given** an assigned referee who is unavailable at a proposed changed game time
- **When** Professor North saves the otherwise valid schedule change
- **Then** the affected assignment is flagged for owner attention rather than presented as confirmed

#### Scenario: Enforce all three soccer official slots
- **Given** a soccer game with one center referee and one assistant assigned
- **When** the owner reviews its officiating coverage
- **Then** the second assistant slot is shown as unfilled

## Screen Requirements

- Referee directory: name, contact email visible only to authorized users, supported types, active status, and availability editor.
- Game officials panel: one center and two assistant referee slots for soccer, current assignments, eligible referee picker, conflict feedback, and Withdraw/Replace actions.
- Attention list: unfilled or affected assignments, game date/time/location, and replacement history. Referee personal view lists their own assignments and a future availability editor; changes affecting assignments display an owner-review warning.
