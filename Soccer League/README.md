# Soccer League Feature Specifications

These specifications cover the class notes and use the OC SpecKit header, story IDs, priorities, independent tests, FR numbering, conceptual entities, and story-grouped Given/When/Then acceptance criteria. Each feature contains only the seven sections requested in Canvas, with the header at the top.

The supplied PDF is a POS example. Its POS domain, Java Swing stack, extra sections, implementation commands, and repository policies are not soccer requirements. No product code is included.

The assignment branch is `Soccer-League`. Feature header branch patterns are preserved as SpecKit metadata for possible later implementation. No feature implementation branches, commits, pushes, pull requests, merges, or submissions are performed by these files.

## Feature catalog

- [Feature 1 — User Access and Permissions](feature-1-user-access.md)
- [Feature 2 — Sports Leagues Seasons and Locations](feature-2-league-maintenance.md)
- [Feature 3 — Teams Players and Rosters](feature-3-teams-and-players.md)
- [Feature 4 — Automatic Season Scheduling](feature-4-season-scheduling.md)
- [Feature 5 — Game Scores and Player Statistics](feature-5-scores-and-statistics.md)
- [Feature 6 — Referee Availability and Assignments](feature-6-referee-assignments.md)
- [Feature 7 — Public League Website](feature-7-public-league-views.md)
- [Feature 8 — League Email Preparation and Delivery](feature-8-league-email.md)
- [Feature 9 — Top Four Postseason Tournament](feature-9-postseason-tournament.md)

## Requirement coverage

- Ten teams and eighteen games per team, automatic scheduling, two weekly playing nights and byes: Feature 4. Ninety games is the arithmetic consequence of the confirmed team/game counts; double round robin is the selected pairing rule.
- Games, scores, manager-chosen statistics, and individual player statistics: Feature 5.
- Family/friend website access, league/season/team/game/schedule display: Feature 7.
- Professor North alone changes schedules: Features 1 and 4.
- Referee directory, referee type, availability, assignments, replacement: Feature 6.
- Referee assignment emails and collection of player emails: Features 3 and 8. Player messages cover schedule changes, cancellations, and tournament notices.
- Top-four end-of-season tournament: Feature 9.
- Sports, leagues, seasons, games, teams, players, locations, referees, and users: Features 1–6. Men's/women's leagues and the named sports are represented in Feature 2.
- Relationships sketched in the notes, including game/player statistics and referee-assignment roles: the relevant Initial Data Model sections.

## Selected business rules

On user request, the previous clarification placeholders were replaced with these decisions. They are chosen design rules, not additional quoted class notes.

- Owner-created username/password accounts; manager and referee roles may overlap.
- Owner-managed league setup; managers maintain their own rosters. Active players need names/emails and may join only one team per season.
- Double round robin: ninety games, eighteen per team, nine home and nine away. Two owner-selected playing nights; at most one game per team per night. Dates, windows, duration, and time zone are owner inputs.
- Home-team manager records the initial score after the game; Professor North handles corrections with a reason and retained history. Managers maintain their own team's nonnegative numeric statistics; matching categories sum across the season.
- Soccer requires one center referee and two assistants with thirty minutes between a referee's games. Referees maintain future availability; the owner handles assignment and replacement.
- Player display names, membership, results, and statistics are public. Private contact details and referee names are excluded.
- Owner-triggered email covers referee assignments and relevant player notices, with sender/service setup required and no automatic reminders.
- Standings use 3/1/0 points, goal difference, goals scored, then an owner-recorded witnessed draw. Forfeits are owner-entered 3-0 results.
- Four-team single elimination: 1 vs 4, 2 vs 3, then a final; no third-place match. Penalty shootouts decide tied playoff games. Qualification is frozen and protected once semifinal results exist.

All nine feature specifications are Ready as specifications; implementation and automated tests have not been performed. The seven requested sections remain intact.
