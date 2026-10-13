# Feature Specification: [FEATURE NAME]

**Feature Directory**: `specs/[NNN-short-name]`
**Created**: [YYYY-MM-DD]
**Status**: Draft
**Input**: "[original feature description]"

<!-- WHAT and WHY only. No languages, frameworks, databases, APIs, or code structure. -->

## User Scenarios

<!-- Ordered by priority. Each story alone must be a viable, demonstrable slice. -->

### User Story 1 - [Title] (Priority: P1)

[The user journey in plain language]

**Why this priority**: [value delivered]

**Independent Test**: [how to show this story works on its own]

**Acceptance Scenarios**:

1. **Given** [state], **When** [action], **Then** [outcome]

---

### User Story 2 - [Title] (Priority: P2)

[...]

### Edge Cases

- What happens when [boundary condition]?
- How does the system handle [failure of a dependency / concurrent edit / bad input]?

## Requirements

### Functional Requirements

- **FR-001**: System MUST [testable capability]
- **FR-002**: Users MUST be able to [interaction]
- **FR-003**: System MUST [behaviour] [NEEDS CLARIFICATION: question] <!-- max 3 markers -->

### Access and Data Protection

- **FR-0XX**: Only [role] MAY [action]; others are refused.
- [Sensitive data involved and how it must be protected, or remove this section]

### Key Entities

- **[Entity]**: [what it represents, key attributes, relationships]

## Success Criteria

<!-- Measurable and technology-agnostic. -->

- **SC-001**: [user-facing outcome with a number, e.g., "Search returns results in under 1 second for 95% of queries"]
- **SC-002**: [scale, e.g., "Supports 10,000 concurrent users without degradation"]
- **SC-003**: [reliability or failure behaviour, e.g., "A payment provider outage never loses a confirmed order"]

## Assumptions

- [Reasonable default chosen where the description was silent]
- [Out of scope for this feature]
- [Dependency on an existing system]
