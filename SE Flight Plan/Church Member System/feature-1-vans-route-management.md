# Feature: Van Route Management
 
**Feature ID:** 3
**Branch pattern:** `feature/3-van-route-management`
**Status:** Draft
**Created:** 2026-09-11
**Input:** Organize van routes and stops as the church adds more routes to pick up kids for church.
**Depends on:** [Feature 1 — Member & Guest Attendance](feature-1-member-guest-attendance.md)
 
---
 
## User Stories
 
### US-3.1: Create a van route
**As a** church staff member
**I want to** create a named van route
**So that** I can organize pickups as the church adds more routes
 
**Priority:** P1
**Independent test:** Create a route with a name; it appears in the route list
**Acceptance scenarios:** see ### US-3.1 under Acceptance Criteria
 
### US-3.2: Add stops to a route
**As a** church staff member
**I want to** add pickup stops, each with an address and order, to a route
**So that** the driver knows where and in what order to pick up kids
 
**Priority:** P1
**Independent test:** Add a stop with an address to a route; it appears in the route's stop list in order
**Acceptance scenarios:** see ### US-3.2 under Acceptance Criteria
 
### US-3.3: Assign a person to a route
**As a** church staff member
**I want to** assign a member or guest to a van route
**So that** I know who rides which van
 
**Priority:** P1
**Independent test:** Assign a person to a route; they appear on that route's rider list
**Acceptance scenarios:** see ### US-3.3 under Acceptance Criteria
 
### US-3.4: View a route's rider list and stops
**As a** church staff member
**I want to** see all stops and all riders assigned to a route
**So that** I can confirm the route is organized correctly before pickup day
 
**Priority:** P1
**Independent test:** Open a route and see both its stops and its riders
**Acceptance scenarios:** see ### US-3.4 under Acceptance Criteria
 
### US-3.5: Edit or delete a route
**As a** church staff member
**I want to** rename a route, reorder its stops, or remove a route
**So that** routes stay accurate as the van program grows
 
**Priority:** P2
**Independent test:** Edit a route's name and confirm it updates
**Acceptance scenarios:** see ### US-3.5 under Acceptance Criteria
 
---
 
## Requirements
 
### Functional Requirements
 
- **FR-001**: System MUST allow staff to create a van route with a name.
- **FR-002**: System MUST allow staff to add one or more stops to a route, each with an address and a pickup order.
- **FR-003**: System MUST allow staff to assign any existing person (member or guest) from Feature 1 to a route.
- **FR-004**: A person MUST be assignable to at most one route at a time.
- **FR-005**: System MUST list a route's stops in pickup order.
- **FR-006**: System MUST list all riders assigned to a route.
- **FR-007**: System MUST allow staff to reorder, edit, or remove a route's stops.
- **FR-008**: System MUST allow staff to delete a route; deleting a route MUST also remove its stops and rider assignments.
- **FR-009**: Route name and stop address MUST NOT be blank when saving.
---
 
## Initial Data Model
 
### Key Entities
 
- **VanRoute**: a named pickup route.
- **RouteStop**: an address and pickup order belonging to one route.
- **RouteAssignment**: links one Person (Feature 1) to one VanRoute as a rider.
### Data Model Requirements
 
#### `van_routes` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `name` | STRING(100) | Required |
| `driverName` | STRING(100) | Optional |
| `createdAt` | DATE | Auto-set |
| `updatedAt` | DATE | Auto-set |
 
#### `route_stops` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `routeId` | INTEGER FK | Required; references `van_routes.id` |
| `address` | STRING(255) | Required |
| `stopOrder` | INTEGER | Required; determines pickup sequence |
| `pickupTime` | STRING(20) | Optional |
 
#### `route_assignments` table
| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `routeId` | INTEGER FK | Required; references `van_routes.id` |
| `personId` | INTEGER FK | Required, unique; references `people.id` |
 
Unique constraint on `personId` enforces one active route per person.
 
---
 
## Acceptance Criteria
 
### US-3.1 — Create a van route
 
#### Scenario: Staff creates a new route
*   **Given** staff is on the add-route form
*   **When** staff enters name `North Route`
*   **And** staff saves
*   **Then** a new van route `North Route` is created
*   **And** `North Route` appears in the route list
### US-3.2 — Add stops to a route
 
#### Scenario: Staff adds stops in order
*   **Given** route `North Route` exists with no stops
*   **When** staff adds stop `123 Main St` as stop `1`
*   **And** staff adds stop `456 Oak Ave` as stop `2`
*   **Then** `North Route`'s stop list shows `123 Main St` before `456 Oak Ave`
#### Scenario: Staff attempts to add a stop with no address
*   **Given** route `North Route` exists
*   **When** staff attempts to add a stop with a blank address
*   **Then** the save is blocked
*   **And** staff sees a message that address is required
### US-3.3 — Assign a person to a route
 
#### Scenario: Staff assigns a rider to a route
*   **Given** route `North Route` exists
*   **And** person `Sam Lee` exists in the directory and is unassigned
*   **When** staff assigns `Sam Lee` to `North Route`
*   **Then** `Sam Lee` appears on `North Route`'s rider list
#### Scenario: Staff attempts to assign a person already on another route
*   **Given** `Sam Lee` is already assigned to `South Route`
*   **When** staff attempts to assign `Sam Lee` to `North Route`
*   **Then** the assignment is blocked
*   **And** staff sees a message that `Sam Lee` is already on another route
### US-3.4 — View a route's rider list and stops
 
#### Scenario: Route detail shows stops and riders together
*   **Given** route `North Route` has two stops and one assigned rider
*   **When** staff opens `North Route`
*   **Then** staff sees both stops in pickup order
*   **And** staff sees the assigned rider
### US-3.5 — Edit or delete a route
 
#### Scenario: Staff renames a route
*   **Given** route `North Route` exists
*   **When** staff renames it to `North Side Route`
*   **And** staff saves
*   **Then** the route's stored name is `North Side Route`
#### Scenario: Staff deletes a route
*   **Given** route `North Route` has stops and riders
*   **When** staff deletes `North Route`
*   **Then** the route no longer appears in the route list
*   **And** its stops and rider assignments are removed
