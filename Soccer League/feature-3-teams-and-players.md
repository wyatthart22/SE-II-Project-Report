# Feature: Teams Players and Rosters

**Feature ID:** 3
**Branch pattern:** `feature/3-teams-and-players`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Maintain teams, season participation, player rosters, manager assignments, and player email addresses.
**Depends on:** [Feature 1](feature-1-user-access.md), [Feature 2](feature-2-league-maintenance.md)


## User Stories

### US-3.1: Maintain teams and season rosters
**As a** league owner
**I want to** place teams and players into a season
**So that** scheduling and statistics refer to the correct participants
**Priority:** P1
**Independent test:** Register a team in a season, add a player, and retrieve the player through the team roster.
**Acceptance scenarios:** See US-3.1 under Gherkin AC.

### US-3.2: Record player contact information
**As a** owner or assigned team manager
**I want to** record player email addresses
**So that** league communications can reach players
**Priority:** P1
**Independent test:** Save a valid player email and verify that it is available for an authorized recipient preview but absent from a public roster.
**Acceptance scenarios:** See US-3.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST maintain teams and players and MUST associate teams with seasons through season participation records.
- **FR-002:** System MUST record a player name and a valid email address for active roster membership, and MUST associate each roster entry with a player and a season team.
- **FR-003:** System MUST distinguish a season roster from a player's identity so changing seasons does not overwrite past participation or statistics.
- **FR-004:** System MUST associate a team manager with the season team they manage, using the access model in Feature 1.
- **FR-005:** System MUST prevent duplicate membership records for the same player and season team.
- **FR-006:** System MUST preserve players and teams referenced by recorded results or statistics, using inactive records or ended roster membership rather than erasing history.
- **FR-007:** System MUST validate a supplied email address and identify missing addresses to an authorized user preparing email; missing email MUST NOT silently imply successful delivery.
- **FR-008:** System MUST allow the owner to maintain all teams and rosters and managers to maintain only their assigned team rosters. A player MUST have a name and valid email address before joining an active season roster and MUST belong to no more than one team in the same season. A player may participate in different seasons or leagues. 

## Key Entities

- **Team:** a named team that may participate in multiple seasons.
- **SeasonTeam:** one team's entry in one season.
- **Player:** an individual with a name and private contact address.
- **RosterEntry:** links a player to a season team for a participation period.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Team(team_id PK, name, active).
- SeasonTeam(season_team_id PK, season_id FK Season, team_id FK Team); unique (season_id, team_id).
- Player(player_id PK, display_name, email, active).
- RosterEntry(roster_entry_id PK, season_team_id FK SeasonTeam, player_id FK Player, joined_on DATE, left_on DATE NULL); A player cannot have overlapping active membership in different teams of the same season; unique membership for the same participation period. left_on >= joined_on when supplied.
- ManagerTeamAssignment is defined in Feature 1. Result statistics reference Player and Game, not just display names.

## Gherkin AC

### US-3.1: Maintain teams and season rosters

#### Scenario: Add a player to a season roster
- **Given** a team entered in a season and an existing player
- **When** the owner adds the player to that season team
- **Then** one roster entry links the player to the correct season

#### Scenario: Reject duplicate roster membership
- **Given** a player already on a season team roster
- **When** the owner attempts to add the same membership again
- **Then** the duplicate is rejected without creating another entry

#### Scenario: Retain statistics after roster departure
- **Given** a player with recorded game statistics
- **When** the owner ends the player's roster membership
- **Then** the historical statistics remain linked to that player and game

#### Scenario: Reject a second team in the same season
- **Given** a player already active on one team in a season
- **When** a manager adds that player to another team in the same season
- **Then** the membership is rejected and the existing roster is unchanged

### US-3.2: Record player contact information

#### Scenario: Reject a malformed contact address
- **Given** a player contact form
- **When** an authorized maintainer saves an invalid email address
- **Then** the address error is shown and the invalid update is not saved

#### Scenario: Keep player email private
- **Given** a player with a stored email address
- **When** a visitor opens the public roster
- **Then** the player email address is not displayed

## Screen Requirements

- Owner team list and season roster: filter by league and season; show team name, manager, and player membership.
- Player form: name, email, active status, and season-team association; provide validation and duplicate feedback.
- Roster history shows participation dates. Private email fields and administrative controls are absent from public roster screens.
