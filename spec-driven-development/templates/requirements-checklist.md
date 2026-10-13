# Specification Quality Checklist: [FEATURE NAME]

**Purpose**: Validate the spec is complete and unambiguous before planning
**Created**: [YYYY-MM-DD]
**Feature**: [spec.md](../spec.md)

<!-- These test the requirements, not the implementation. -->

## Content Quality

- [ ] No implementation details (languages, frameworks, APIs, storage)
- [ ] Focused on user value and business needs
- [ ] All mandatory sections completed

## Requirement Completeness

- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Every requirement is testable and unambiguous
- [ ] Success criteria are measurable and technology-agnostic
- [ ] Every user story has acceptance scenarios and an independent test
- [ ] Edge cases and failure behaviour are identified
- [ ] Scope is bounded; assumptions and dependencies are listed

## Scale and Security

- [ ] Expected scale or growth is stated (users, data volume, request rate)
- [ ] Every action states who may perform it
- [ ] Sensitive data and its protection are identified, or explicitly none

## Notes

- [Failing items and the spec text that fails them]
