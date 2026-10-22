# Feature: Public League Website

**Feature ID:** 7
**Branch pattern:** `feature/7-public-league-views`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Make league schedules, games, teams, players, results, and player statistics accessible to family and friends.

## User Stories

### US-7.1: Browse the league schedule
**As a** family member or friend
**I want to** browse leagues seasons teams and published games
**So that** I can find when and where someone is playing
**Priority:** P1
**Independent test:** Without signing in, select a league/season/team and locate its next published game.
**Acceptance scenarios:** See US-7.1 under Gherkin AC.

### US-7.2: View results and player performance
**As a** public visitor
**I want to** read recorded game results and player statistics
**So that** I can follow teams and players
**Priority:** P1
**Independent test:** Open a completed game and a player page and verify that they show the saved public information.
**Acceptance scenarios:** See US-7.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST provide public read-only website access without sign-in to published leagues, seasons, teams, games, and schedules.
- **FR-002:** System MUST let visitors narrow a schedule by league, season, and team, and MUST display game date, time, time zone, participating teams, location, and current status.
- **FR-003:** System MUST display saved public game results and player statistics with clear game, season, category, and unit context; unplayed games MUST NOT display an invented zero score.
- **FR-004:** System MUST display current published schedule changes, cancellations, and byes without exposing unpublished drafts.
- **FR-005:** System MUST show public team rosters and player performance using display names and team membership only. Private player/referee email addresses and authentication data MUST NOT be returned through public views.
- **FR-006:** System MUST provide clear messages when a league has no published schedule, filters have no games, or a player has no recorded statistics.
- **FR-007:** System MUST publish player display names and team membership, valid saved player statistics, and recorded game results immediately. Referee names, contact email addresses, login data, and private assignment history MUST remain excluded from public views. Only published schedules may appear publicly.

## Key Entities

- **PublicScheduleView:** a read-only projection of the authoritative published schedule.
- **PublicTeamView:** team and public roster information for a season.
- **PublicPlayerView:** public display names and recorded performance.
- **PublicGameView:** published game details and any recorded public result.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- Public views project existing Season, SeasonTeam, Team, Player, Game, ScheduleGame, GameResult, and PlayerGameStatistic records; they do not create a second editable copy.
- Schedule queries require Schedule.status = PUBLISHED and the selected season/team filters.
- Public response fields are explicitly allowlisted; player/referee email and sign-in data are omitted from the response, not merely hidden visually.

## Gherkin AC

### US-7.1: Browse the league schedule

#### Scenario: Find a published team game
- **Given** a published game for a team in a selected season
- **When** a visitor selects that season and team without signing in
- **Then** the game date time zone time opponent and location are displayed

#### Scenario: Keep a draft private
- **Given** a season with only a draft schedule
- **When** a visitor opens its schedule
- **Then** the site reports that no schedule is published and does not show draft games

#### Scenario: Show an empty filter result
- **Given** a valid season/team filter with no matching published games
- **When** a visitor applies the filter
- **Then** a clear no-games message appears

### US-7.2: View results and player performance

#### Scenario: Show recorded performance
- **Given** a completed game with saved public scores and saved player statistics
- **When** a visitor opens the game and player detail
- **Then** the recorded values appear with their game season and category context

#### Scenario: Do not expose contact information
- **Given** a player and referee with stored email addresses
- **When** a visitor reads public league pages
- **Then** their private email addresses and sign-in information are absent

#### Scenario: Publish saved results without a separate approval
- **Given** a home-team manager has saved a valid completed-game score
- **When** a visitor opens the game
- **Then** the saved score appears and referee names and contact emails remain absent

## Screen Requirements

- Public home: league and season selectors, team search/filter, schedule link, and clear read-only presentation.
- Schedule/game pages: date, time and time zone, opponents, location, status, byes, and recorded scores. Links lead to teams and public player detail.
- Player page: display name and statistics grouped by season/game/category. Pages support phone and desktop widths, readable labels, keyboard navigation, and loading/empty/error states.
