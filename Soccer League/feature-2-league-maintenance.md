# Feature: Sports Leagues Seasons and Locations

**Feature ID:** 2
**Branch pattern:** `feature/2-league-maintenance`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Maintain the sports, leagues, seasons, and locations needed to organize the soccer league and the other sports identified in class.
**Depends on:** [Feature 1](feature-1-user-access.md)

## User Stories

### US-2.1: Maintain league structure
**As a** league owner
**I want to** maintain sports, leagues, and seasons
**So that** teams and games belong to the correct competition
**Priority:** P1
**Independent test:** Create a soccer league and season, edit its details, and retrieve them together.
**Acceptance scenarios:** See US-2.1 under Gherkin AC.

### US-2.2: Maintain playing locations
**As a** league owner
**I want to** maintain locations and their available playing periods
**So that** games can be placed at usable venues
**Priority:** P1
**Independent test:** Create a location with a playing window and verify that it can be selected for a game.
**Acceptance scenarios:** See US-2.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST support maintenance of sports, leagues, seasons, and locations, with the owner responsible for changes in this initial specification.
- **FR-002:** System MUST represent the named sports: soccer, flag football, basketball, and softball, without assuming that soccer scheduling rules apply to every sport.
- **FR-003:** System MUST associate a league with a sport and a season with a league; men's and women's competitions MUST be distinguishable where applicable.
- **FR-004:** System MUST record a required name for each sport, league, season, and location, and required season start and end dates; the end date MUST NOT precede the start date.
- **FR-005:** System MUST retain references to records already used by games or results. Deactivation MUST NOT erase historical games, rosters, or statistics.
- **FR-006:** System MUST make locations and their usable date/time windows available to schedule validation; conflicting game use is handled in Feature 4.
- **FR-007:** System MUST allow the owner to define league names and men's or women's divisions, season dates, two weekly playing nights, game duration, and location availability or blackout periods before generating a schedule. Each location record represents one playing field with capacity for one game at a time. Only the owner may maintain this league setup.

## Key Entities

- **Sport:** a type of competition.
- **League:** a competition for a sport and applicable division.
- **Season:** a dated occurrence of a league.
- **Location:** a named playing venue with usable periods.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Sport(sport_id PK, name, active).
- League(league_id PK, sport_id FK Sport, name, division_label, active).
- Season(season_id PK, league_id FK League, name, start_date DATE, end_date DATE, time_zone, game_duration_minutes INTEGER, status). Constraint: end_date >= start_date.
- Location(location_id PK, name, description, active).
- LocationAvailability(availability_id PK, location_id FK Location, starts_at DATETIME, ends_at DATETIME); ends_at > starts_at. Each location is one field; the season supplies the league time zone.

## Gherkin AC

### US-2.1: Maintain league structure

#### Scenario: Create a valid season
- **Given** an existing soccer league
- **When** the owner enters a season name and valid date range
- **Then** the season is saved under that league

#### Scenario: Reject an invalid date range
- **Given** a season with an end date earlier than its start date
- **When** the owner attempts to save
- **Then** the date error is displayed and the season is not saved

#### Scenario: Preserve referenced history
- **Given** a league referenced by completed games
- **When** the owner deactivates the league
- **Then** historical games remain available and the league is not offered for new active seasons

#### Scenario: Reject incomplete schedule setup
- **Given** a season without a game duration or league time zone
- **When** Professor North requests schedule generation
- **Then** generation is blocked and the missing setup fields are identified

### US-2.2: Maintain playing locations

#### Scenario: Make a location available to scheduling
- **Given** a location with a usable playing window
- **When** the owner reviews available locations for a game in that window
- **Then** the location is available subject to conflict validation

## Screen Requirements

- Owner maintenance lists: Sports, Leagues, Seasons, and Locations, each with create and edit forms and active/inactive filters.
- Season form: league, name, start/end dates, and status; show inline required-field and date errors.
- Location form: name, description, and availability windows. Every list has loading, empty, and failure states.
