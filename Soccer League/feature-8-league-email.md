# Feature: League Email Preparation and Delivery

**Feature ID:** 8
**Branch pattern:** `feature/8-league-email`
**Status:** Ready
**Created:** 2026-10-21
**Input:** Let the owner send referees their officiating assignments and use collected player emails for relevant league communications.

## User Stories

### US-8.1: Send referee assignment notices
**As a** league owner
**I want to** review and send each referee their game assignments
**So that** officials know when where and in what role they work
**Priority:** P1
**Independent test:** Preview one referee's current assignments and verify that the notice contains only their selected assignment details.
**Acceptance scenarios:** See US-8.1 under Gherkin AC.

### US-8.2: Prepare player communications
**As a** league owner
**I want to** select players by season or team and review recipient addresses
**So that** relevant players can receive league information
**Priority:** P1
**Independent test:** Select a team roster, preview recipients, and identify missing addresses before sending.
**Acceptance scenarios:** See US-8.2 under Gherkin AC.

## Functional Requirements

- **FR-001:** System MUST prepare referee notices containing each selected referee's current game date, time/time zone, location, participating teams, and referee role/type.
- **FR-002:** System MUST allow the owner to preview recipients and message contents and explicitly choose Send; generating schedules, editing contacts, or replacing an official MUST NOT send messages automatically.
- **FR-003:** System MUST obtain referee recipients from saved referee contact details and player recipients from selected season/team rosters; it MUST deduplicate repeated addresses within one send.
- **FR-004:** System MUST identify missing or invalid email addresses before sending, retain affected recipients as unsent, and not report them as delivered.
- **FR-005:** System MUST record send attempts and distinguish pending, accepted by mail provider, and failed outcomes. Provider acceptance MUST NOT be described as confirmed inbox delivery.
- **FR-006:** System MUST support review and resend for failed recipients without automatically resending to recipients already accepted by the provider in the same operation.
- **FR-007:** System MUST avoid disclosing one recipient's private contact address or unrelated assignments to other recipients.
- **FR-008:** System MUST permit only Professor North to send league email. Referee messages contain current assignments; player messages are limited to schedule changes, cancellations, and tournament notices for selected season or team rosters. The sender MUST be a league email account configured by the owner. The email delivery service MUST be configured before Send is enabled. No automatic reminder or background mailing is included.

## Key Entities

- **EmailBatch:** a reviewed communication operation and its message template.
- **EmailRecipient:** one intended recipient and send outcome.
- **AssignmentNoticeItem:** the assignment details included in a referee notice.

## Initial Data Model

Logical fields and relationships; storage technology and implementation are not selected by this specification.

- EmailBatch(batch_id PK, sender_user_id FK User, purpose, subject, body, created_at DATETIME, sent_at DATETIME NULL).
- EmailRecipient(recipient_id PK, batch_id FK EmailBatch, recipient_kind REFEREE|PLAYER, referee_id FK Referee NULL, player_id FK Player NULL, email_snapshot, state PENDING|ACCEPTED|FAILED, error_message NULL); exactly one person link. Deduplicate by normalized address per batch.
- AssignmentNoticeItem(recipient_id FK EmailRecipient, assignment_id FK RefereeAssignment, details_snapshot); captures the information reviewed for the notice.
- Recipient data is private. Current assignments are revalidated before a pending message is sent so a withdrawn assignment is not represented as current.

## Gherkin AC

### US-8.1: Send referee assignment notices

#### Scenario: Preview an assignment notice without sending
- **Given** a referee with an email address and current assignments
- **When** the owner previews the notice
- **Then** the correct role dates times locations and teams appear and no email is sent

#### Scenario: Send only after explicit action
- **Given** a reviewed batch with valid recipients
- **When** the owner selects Send
- **Then** send attempts are recorded for those recipients only

#### Scenario: Do not send a withdrawn assignment
- **Given** a notice drafted before an official was replaced
- **When** the owner attempts to send the old draft
- **Then** the outdated content requires refresh before it can be sent as a current assignment

### US-8.2: Prepare player communications

#### Scenario: Report missing addresses
- **Given** a selected roster containing a player without email
- **When** the owner previews recipients
- **Then** that player is identified as unsent and is not counted as successfully contacted

#### Scenario: Retry a partial failure safely
- **Given** one accepted recipient and one failed recipient in a batch
- **When** the owner retries the failed recipient
- **Then** only the failed recipient is retried and the accepted recipient is not sent a duplicate

#### Scenario: Require email delivery setup
- **Given** the league sender or delivery service is not configured
- **When** Professor North opens the email composer
- **Then** Send is disabled and the missing configuration is identified

## Screen Requirements

- Owner email composer: communication purpose, league/season/team or referee selection, recipient preview, subject/body, and explicit Send button.
- Referee preview groups assignments by recipient and shows dates, time zones, locations, teams, and roles.
- Delivery summary distinguishes accepted, pending, failed, and missing-address recipients; provide targeted retry and no automatic send on page load.
