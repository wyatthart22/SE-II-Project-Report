# Feature: Automatic Season Scheduling

**Feature ID:** 4
**Branch pattern:** `feature/4-season-scheduling`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Replace manual scheduling for ten soccer teams playing eighteen regular-season games each, with two weekly playing nights and occasional byes.
**Depends on:** [Feature 1](feature-1-user-access.md), [Feature 2](feature-2-league-maintenance.md), [Feature 3](feature-3-teams-and-players.md)

## User Stories

### US-4.1: Generate a season schedule
**As a** league owner
**I want to** generate a complete schedule automatically
**So that** I do not have to arrange every game manually
**Priority:** P1
**Independent test:** Generate a schedule for ten teams and verify ninety games with eighteen appearances per team.
**Acceptance scenarios:** See US-4.1 under Gherkin AC.

### US-4.2: Review publish and change a schedule
**As a** league owner
**I want to** review a draft and control published schedule changes
**So that** families and participants see a reliable schedule
**Priority:** P1
**Independent test:** Publish a valid draft, change one game without a conflict, and verify that public views show the new time.
**Acceptance scenarios:** See US-4.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST generate a draft regular-season soccer schedule for exactly ten participating teams with eighteen games per team, producing ninety distinct games because every game has two teams.
- **FR-002:** System MUST separate draft schedules from published schedules. Generation MUST NOT silently overwrite a published schedule or completed game.
- **FR-003:** System MUST use the season's two designated weekly playing nights, usable locations, playing windows, and season dates; a team with no game on a playing night MUST be identifiable as having a bye.
- **FR-004:** System MUST prevent a team playing itself, overlapping games for either team, and overlapping use of the same indivisible playing location. It MUST reject missing teams, venues, or insufficient capacity with an explanation rather than publish a partial schedule as complete.
- **FR-005:** System MUST count only regular-season games toward the eighteen-game target; postseason games are separate.
- **FR-006:** System MUST allow only Professor North to generate, edit, or publish a schedule, including changes after publication.
- **FR-007:** System MUST validate all changed games before saving and MUST preserve existing results; an invalid change leaves the prior schedule unchanged.
- **FR-008:** System MUST use exactly two weekly playing nights chosen by Professor North and MUST schedule at most one game per team per playing night. Professor North MUST supply season dates, game duration, playing windows, and the league time zone. Nights with no assigned game for a participating team are byes. The generator MUST reject insufficient capacity rather than relax these limits.
- **FR-009:** System MUST use a double round robin: every team plays each of the other nine teams twice, once as the home team and once as the away team. Each team therefore has eighteen regular-season games, nine home and nine away, and the season has ninety games.

## Key Entities

- **Schedule:** a versioned draft or published arrangement of season games.
- **Game:** two distinct season teams playing at a location and time.
- **PlayingNight:** a permitted weekly league playing period.
- **Bye:** no game for a participating team on a scheduled playing night; derived from that night's games.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Schedule(schedule_id PK, season_id FK Season, version INTEGER, status DRAFT|PUBLISHED, created_by FK User). One authoritative published schedule per season.
- Game(game_id PK, season_id FK Season, stage REGULAR|PLAYOFF, home_season_team_id FK SeasonTeam, away_season_team_id FK SeasonTeam, status SCHEDULED|COMPLETED|CANCELLED); team IDs differ and both belong to the season.
- ScheduleGame(schedule_id FK Schedule, game_id FK Game, location_id FK Location, starts_at DATETIME, ends_at DATETIME); unique (schedule_id, game_id), ends_at > starts_at. Conflicts are interval comparisons, not only equal start times.
- SeasonPlayingWindow(window_id PK, season_id FK Season, weekday, starts_at_local, ends_at_local). Weekly frequency details remain subject to FR-008.
- No stored bye result or score is created. Completed Game identities remain stable across schedule revisions.

## Gherkin AC

### US-4.1: Generate a season schedule

#### Scenario: Generate a complete feasible schedule
- **Given** ten season teams and enough valid playing windows and locations using double round-robin pairing
- **When** Professor North generates a draft
- **Then** the draft has ninety regular-season games and each team appears eighteen times

#### Scenario: Explain an infeasible request
- **Given** ten teams but insufficient playing capacity
- **When** Professor North requests a complete schedule
- **Then** generation reports the unmet constraints and does not publish an incomplete schedule

#### Scenario: Identify a bye night
- **Given** a valid playing night on which one participating team has no game
- **When** the owner reviews that team's schedule
- **Then** the night is shown as a bye and contributes zero games to its eighteen-game count

#### Scenario: Verify balanced double round robin
- **Given** a generated ten-team season
- **When** the owner checks every pair and each team total
- **Then** each pair meets twice with reversed home teams and every team has nine home and nine away games

### US-4.2: Review publish and change a schedule

#### Scenario: Reject an overlapping change
- **Given** a game whose proposed new interval overlaps another game for one team or location
- **When** Professor North attempts to save the change
- **Then** the conflict is explained and the prior schedule remains unchanged

#### Scenario: Publish only after validation
- **Given** a draft satisfying the double round-robin, weekly, and capacity rules
- **When** Professor North publishes it
- **Then** the published version becomes visible through the public schedule

#### Scenario: Reject a manager schedule change
- **Given** an authenticated team manager
- **When** the manager attempts to change a game time
- **Then** the request is denied and the game time remains unchanged

## Screen Requirements

- Owner schedule generator: season, participating teams, two playing nights, time windows, locations, Generate draft, and a constraint summary.
- Draft review: calendar/list views, per-team game counts, total game count, byes, conflicts, and unresolved inputs; Publish is unavailable while validation fails.
- Owner game editor: teams, date/time, location, stage, and status; conflicts appear before save. Public users receive only the published version.
