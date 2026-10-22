# Feature: User Access and Permissions

**Feature ID:** 1
**Branch pattern:** `feature/1-user-access`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Give the owner, team managers, referees, and public visitors access appropriate to their responsibilities.

## User Stories

### US-1.1: Sign in and sign out
**As a** authorized user
**I want to** sign in and end my session
**So that** I can perform only the work assigned to me
**Priority:** P1
**Independent test:** Sign in, access an allowed action, sign out, and confirm that protected actions are unavailable.
**Acceptance scenarios:** See US-1.1 under Gherkin AC.

### US-1.2: Enforce responsibilities
**As a** league owner
**I want to** restrict changes by user role and assignment
**So that** only authorized people can change league information
**Priority:** P1
**Independent test:** Compare owner, manager, referee, and public access to schedule editing and score entry.
**Acceptance scenarios:** See US-1.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST authenticate users before allowing protected changes and MUST end protected access when they sign out.
- **FR-002:** System MUST allow public visitors to read published information without requiring an account.
- **FR-003:** System MUST identify Professor North as the owner account authorized to create, generate, publish, or change schedules. A manager or referee MUST NOT gain schedule-editing permission merely by having an account.
- **FR-004:** System MUST restrict a team manager to score and statistic entry for games involving a team assigned to that manager; Feature 5 defines the score-entry policy.
- **FR-005:** System MUST permit referees to view their own assignments and maintain their own future availability. Only the owner may assign, withdraw, or replace an official.
- **FR-006:** System MUST validate permission on each protected operation, including a direct attempt to invoke an action hidden by the screen.
- **FR-007:** System MUST keep player and referee contact information off public screens; authorized contact use is defined in Features 3, 6, and 8.
- **FR-008:** System MUST allow only the owner to create or deactivate protected user accounts and assign roles. Users MUST sign in with a unique username and password. A user may hold manager and referee roles together, but only the designated Professor North account has owner authority.

## Key Entities

- **User:** a person with protected access; has one or more assigned responsibilities.
- **Role:** owner, manager, or referee responsibility. Public visitor is an access context, not a required stored user.
- **ManagerTeamAssignment:** links a manager to the team and season they manage.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- User(user_id PK, display_name, login_identifier UNIQUE, password_hash, active). Authentication secrets, if any, are never returned on public views.
- UserRole(user_id FK User, role); unique (user_id, role). Allowed roles are OWNER, MANAGER, REFEREE with owner authority restricted to Professor North.
- ManagerTeamAssignment(assignment_id PK, user_id FK User, season_team_id FK SeasonTeam); unique (user_id, season_team_id).
- Owner authority is tied to the designated Professor North account; role data cannot be supplied by a caller to authorize its own request.

## Gherkin AC

### US-1.1: Sign in and sign out

#### Scenario: Authenticated access ends at logout
- **Given** a valid manager account
- **When** the manager signs in and then signs out
- **Then** protected score entry is denied until the manager signs in again

#### Scenario: Invalid credentials do not establish a session
- **Given** an invalid sign-in attempt
- **When** the user submits it
- **Then** access is denied without revealing which credential was incorrect

### US-1.2: Enforce responsibilities

#### Scenario: Only Professor North changes a schedule
- **Given** a published game and an authenticated team manager
- **When** the manager attempts to change the kickoff time
- **Then** the change is denied and the saved schedule remains unchanged

#### Scenario: Public visitor can read published data
- **Given** a published schedule
- **When** a visitor opens the public schedule without signing in
- **Then** the schedule is displayed without private contact details

#### Scenario: Owner creates a user with two roles
- **Given** Professor North and a new username
- **When** the owner creates an account with manager and referee roles
- **Then** the user can sign in with a password and gains both roles without owner authority

## Screen Requirements

- Sign in: login identifier, password, Sign in, and clear failure feedback. Sign out is available in every authenticated view.
- Navigation: show owner maintenance, manager score entry, or referee assignments according to current permissions. Public browsing remains available without sign-in.
- Unauthorized operations display access-denied feedback and do not change data. Sign-out clears private information from the visible workspace.
