# Feature: Game Scores and Player Statistics

**Feature ID:** 5
**Branch pattern:** `feature/5-scores-and-statistics`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Let team managers enter game scores and manager-selected statistics while retaining a consistent result for each game and statistics for each player.
**Depends on:** [Feature 1](feature-1-user-access.md), [Feature 2](feature-2-league-maintenance.md), [Feature 3](feature-3-teams-and-players.md), [Feature 4](feature-4-season-scheduling.md)

## User Stories

### US-5.1: Record and correct game scores
**As a** team manager
**I want to** enter scores for a game involving my team
**So that** the league can publish accurate results
**Priority:** P1
**Independent test:** Enter both scores for an assigned team's game, save, reload, and confirm one result exists.
**Acceptance scenarios:** See US-5.1 under Gherkin AC.

### US-5.2: Record player statistics
**As a** team manager
**I want to** choose and enter the statistics I track
**So that** each player has a record of performance
**Priority:** P1
**Independent test:** Define a numeric statistic, record it for a rostered player in a game, and retrieve the player's game and season values.
**Acceptance scenarios:** See US-5.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST let the authenticated home-team manager enter the shared score for a game. Visiting-team managers may view it and enter their own team's player statistics, but MUST NOT change the shared score. Only Professor North may correct a saved score.
- **FR-002:** System MUST store one authoritative result per game, with both team scores as nonnegative whole numbers, and MUST distinguish unplayed games from a played zero-to-zero game.
- **FR-003:** System MUST preserve who changed a result and when, and MUST reject stale conflicting edits rather than silently overwrite a newer result.
- **FR-004:** System MUST allow managers to choose the statistics they track by defining named numeric statistic categories. It MUST associate every statistic value with a game, player, team participation, and category.
- **FR-005:** System MUST reject statistics for a player who was not on the selected team's roster for that game. Managers MUST NOT edit another team's player statistics.
- **FR-006:** System MUST validate statistic values using the selected category's declared numeric rules and MUST NOT assume that all sports use the same categories.
- **FR-007:** System MUST show recorded player statistics by game and season without combining categories with different meanings or units. Aggregation MUST follow the category's sum rule.
- **FR-008:** System MUST persist result and statistic changes and reflect saved data in the authorized public views.
- **FR-009:** System MUST publish a valid score when the home-team manager saves it after the game ends; opponent approval is not required. A score disagreement MUST be referred to Professor North, who may correct either score with a required correction reason. Every correction MUST retain the previous values, editor, time, and reason.
- **FR-010:** System MUST let each manager create statistic categories for their assigned teams, using a unique category name within that manager and sport, a unit, and either whole-number or decimal values. Values MUST be nonnegative; decimal values allow at most two decimal places. Season totals MUST sum only values for the same category and unit. Managers may correct their own team's statistics; Professor North may correct any team's statistics. Valid saved statistics are immediately public.

## Key Entities

- **GameResult:** the pair of team scores for one completed game.
- **StatisticDefinition:** a manager-selected measurable category and its value rules.
- **PlayerGameStatistic:** a player's value for a category in a game.
- **ResultRevision:** attribution and prior/new values for result corrections.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- GameResult(game_id PK/FK Game, home_score INTEGER, away_score INTEGER, state RECORDED, version INTEGER, entered_by FK User, updated_at DATETIME); scores >= 0. Absence of a result means no recorded result, not zero.
- StatisticDefinition(statistic_id PK, sport_id FK Sport, owner_manager_id FK User, name, unit, numeric_type, aggregation_rule, active). numeric_type is INTEGER or DECIMAL; values are nonnegative and the aggregation rule is SUM. Categories are unique by owner_manager_id, sport_id, and name.
- PlayerGameStatistic(stat_id PK, game_id FK Game, player_id FK Player, season_team_id FK SeasonTeam, statistic_id FK StatisticDefinition, value DECIMAL(12,2), entered_by FK User, updated_at DATETIME); unique (game_id, player_id, season_team_id, statistic_id).
- ResultRevision(revision_id PK, game_id FK Game, changed_by FK User, changed_at DATETIME, previous_values, new_values, correction_reason). Sensitive information is not included in public history.

## Gherkin AC

### US-5.1: Record and correct game scores

#### Scenario: Save a valid score
- **Given** the authenticated home-team manager and a game whose scheduled end time has passed
- **When** the manager saves valid scores for the first time after the game ends
- **Then** one result is persisted and the saved values are shown after reopening the game

#### Scenario: Reject invalid or unauthorized score entry
- **Given** a manager not assigned to either team in a game
- **When** the manager attempts to save a score
- **Then** the request is denied and the game result is unchanged

#### Scenario: Reject a negative score
- **Given** an authorized score-entry screen
- **When** the manager submits a score below zero
- **Then** a validation error is shown and no invalid score is stored

#### Scenario: Detect competing result edits
- **Given** two owner editing sessions viewing version one of a result and one has saved version two
- **When** the other owner session submits an edit based on version one
- **Then** a conflict is displayed and version two is not overwritten

#### Scenario: Restrict visiting manager score entry
- **Given** the visiting-team manager for a completed game
- **When** the manager attempts to save either shared team score
- **Then** the change is denied but entry of the visiting team's own player statistics remains available

### US-5.2: Record player statistics

#### Scenario: Associate a statistic with a player and game
- **Given** a manager-selected nonnegative numeric category with a unit and SUM aggregation and a rostered player
- **When** the manager saves a valid value for that player's game
- **Then** the saved value is linked to the player game team and category

#### Scenario: Reject statistics for an unrelated player
- **Given** a player not rostered for the manager's team at game time
- **When** the manager attempts to save a statistic for that player
- **Then** the request is rejected and no statistic record is added

## Screen Requirements

- Manager games: show only games for assigned season teams; only the home-team manager sees initial score entry and only the owner sees correction controls; distinguish scheduled, played, and cancelled games.
- Game result form: both team scores, result state, last editor/time, owner correction reason, Save, validation, and stale-edit feedback; do not represent an empty score as zero.
- Statistic setup and entry: category, unit, numeric type, and nonnegative value rules, eligible player selector, game, and value. Player detail shows values grouped by season/game and category.
