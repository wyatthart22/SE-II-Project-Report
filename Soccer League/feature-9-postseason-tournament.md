# Feature: Top Four Postseason Tournament

**Feature ID:** 9
**Branch pattern:** `feature/9-postseason-tournament`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Organize an end-of-season tournament for the top four teams without changing the eighteen-game regular-season requirement.

## User Stories

### US-9.1: Determine qualifying teams
**As a** league owner
**I want to** identify the top four teams from final regular-season standings
**So that** the correct teams enter the tournament
**Priority:** P1
**Independent test:** Use the points, goal-difference, goals-scored, and recorded-draw ranking rules to order completed-season teams and select exactly the first four.
**Acceptance scenarios:** See US-9.1 under Gherkin AC.

### US-9.2: Publish the tournament
**As a** league owner
**I want to** create and publish postseason games
**So that** players and supporters can follow the final competition
**Priority:** P1
**Independent test:** Create a tournament from four qualifiers and publish its games after validating the chosen single-elimination format and schedule.
**Acceptance scenarios:** See US-9.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST support one postseason tournament for a soccer season involving exactly the top four qualifying season teams.
- **FR-002:** System MUST derive standings inputs from recorded regular-season results only and MUST NOT count playoff results toward regular-season standings or the eighteen-game target.
- **FR-003:** System MUST identify incomplete or unresolved regular-season results before qualification and MUST NOT silently treat unplayed games as losses or zero-to-zero draws.
- **FR-004:** System MUST rank teams by regular-season points, then goal difference, then goals scored. If teams remain tied, Professor North MUST record the outcome of a witnessed random draw and its date to establish their final order; the system MUST NOT invent a result.
- **FR-005:** System MUST allow only Professor North to create, schedule, publish, or revise tournament games. All team, venue, and referee availability conflicts remain subject to Features 4 and 6.
- **FR-006:** System MUST make published tournament games available on the public website and reuse the score/statistics model for participating teams and players.
- **FR-007:** System MUST award three points for a win, one for a draw, and zero for a loss. Only Professor North may record a forfeit. Qualification MUST remain blocked until all regular-season games have final results and any remaining tied ranks have recorded draw outcomes. Professor North MUST freeze the top four and their seeds before publishing the tournament. A regular-season correction before a semifinal result is saved MUST invalidate qualification and require the owner to revalidate the bracket before play continues; after a semifinal result is saved, the correction updates season standings but MUST preserve the frozen tournament entries and record that qualification was frozen.
- **FR-008:** System MUST use a four-team single-elimination tournament: seed 1 plays seed 4 and seed 2 plays seed 3 in the semifinals; the winners play the final. There is no third-place game. A tied playoff game MUST be decided by a penalty shootout, with its winner recorded separately from the game score. Only the winner advances, and the final winner is the champion.

## Key Entities

- **Standing:** a season team's regular-season totals and rank under the specified ranking rules.
- **Tournament:** a season's postseason competition for four qualifiers.
- **TournamentEntry:** a qualifying season team and approved rank/seed.
- **TournamentGame:** a playoff-stage game and its tournament relationship.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Standings are calculated from regular-season GameResult records using the specified ranking rules; they are not a second editable score store.
- Tournament(tournament_id PK, season_id FK Season UNIQUE, format SINGLE_ELIMINATION, status DRAFT|PUBLISHED, created_by FK User, qualification_frozen_at DATETIME NULL).
- TournamentEntry(tournament_id FK Tournament, season_team_id FK SeasonTeam, qualifying_rank INTEGER, seed INTEGER); unique team per tournament and exactly four entries at finalization.
- TournamentGame(tournament_id FK Tournament, game_id FK Game UNIQUE, round_label, sequence INTEGER). Linked games have stage PLAYOFF; semifinal winners populate the final. GameResult adds playoff_winner_season_team_id FK SeasonTeam and decided_by NORMAL|PENALTIES; the winner must be one of the two participating teams. Shootout scores remain separate from home_score and away_score.

- TieResolution(resolution_id PK, season_id FK Season, tied_team_ids, ordered_team_ids, draw_date DATE, recorded_by FK User, witness_name); order contains each tied team exactly once.
- GameResult adds is_forfeit BOOLEAN with owner-only entry and retains score correction history from Feature 5.

## Gherkin AC

### US-9.1: Determine qualifying teams

#### Scenario: Select four unambiguous qualifiers
- **Given** all required regular-season results and points, goal-difference, goals-scored, and recorded-draw rules yielding a unique order
- **When** Professor North finalizes qualification
- **Then** exactly the top four season teams become tournament entries

#### Scenario: Do not guess a tied qualification
- **Given** teams still tied after points, goal difference, and goals scored with no owner-recorded draw outcome
- **When** Professor North requests final qualification
- **Then** the unresolved tie is displayed and qualification is not finalized

#### Scenario: Identify missing regular-season results
- **Given** a required regular-season game with no recorded result
- **When** Professor North reviews qualification readiness
- **Then** the missing game is identified rather than counted as a completed draw

### US-9.2: Publish the tournament

#### Scenario: Publish valid postseason games
- **Given** four frozen qualifiers seeded 1 through 4 and conflict-free semifinal games pairing 1 with 4 and 2 with 3
- **When** Professor North publishes the tournament schedule
- **Then** the playoff games are publicly visible and remain separate from regular-season game counts

#### Scenario: Keep playoff results out of season totals
- **Given** a playoff game with a recorded result
- **When** regular-season standings are recalculated
- **Then** the playoff result does not change regular-season standings or eighteen-game counts

#### Scenario: Advance a shootout winner
- **Given** a tied semifinal between seeds 1 and 4
- **When** the owner records seed 4 as the penalty-shootout winner
- **Then** seed 4 advances to the final and the tied game score remains separate from the shootout outcome

## Screen Requirements

- Owner qualification view: season, completed/missing results, standings, ranking-rule status, ties affecting selection, and four qualifier slots.
- Tournament builder: single-elimination format, qualifiers/seeds, games, times, locations, validation, and Publish; block qualification until all results and any required draw outcomes are present.
- Public tournament page: qualified teams, published games, and saved results. Show two semifinals feeding one final and identify the champion after the final result.
