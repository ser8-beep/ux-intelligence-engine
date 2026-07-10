# Documentation Maintainer Guide

> Governance reference for maintaining UX Intelligence Engine documentation.

## Purpose

This document defines how documentation should be maintained, updated, and reviewed.

Every technical document in this repository has a defined responsibility. Before modifying documentation, contributors should understand:

- What the document controls
- When it should be edited
- What dependencies exist
- What risks are introduced by changes

Documentation should evolve with the architecture, not after it.

---

# Documentation Principles

## Single Source of Truth

Each concept should have one primary owner document.

Avoid duplicating:

- architecture decisions
- API definitions
- event schemas
- plugin rules
- accessibility requirements

If information belongs elsewhere, reference that document.

---

## Documentation Boundaries

A document should answer:

> "What decisions does this document own?"

It should not answer:

> "Everything related to this topic."

Clear ownership prevents documentation conflicts.

---

## Change Classification

Before editing, classify the change.

### Low Risk

Examples:

- grammar fixes
- examples
- clarification
- formatting

Review:

One maintainer.

---

### Medium Risk

Examples:

- new sections
- updated workflows
- additional capabilities

Review:

Relevant subsystem owner.

---

### High Risk

Examples:

- architecture changes
- event schema changes
- API changes
- runtime behavior changes

Review:

Architecture owner.

May require:

- Design Decision Record
- Migration plan
- Version update

---

# Document Ownership Map

| Document | Owner | Risk |
|---|---|---|
| README.md | Developer Experience | Medium |
| Architecture.md | Core Runtime | Very High |
| Event-System.md | Core Runtime | Very High |
| Plugin-System.md | Plugin Runtime | High |
| Decision-Engine.md | Intelligence Layer | Very High |
| LLM.md | AI Layer | High |
| Motion.md | Renderer | Medium |
| Accessibility.md | Accessibility | Very High |
| Analytics.md | Observability | Medium |
| Mobile.md | Platform Layer | High |
| API.md | Developer Experience | High |
| Roadmap.md | Product Direction | Low |

---

# Required Review Questions

Before merging documentation changes:

## Ownership

- Does this document own this information?
- Should this content live elsewhere?

## Consistency

- Are related documents updated?
- Are terms consistent?

## Architecture

- Does this change alter system boundaries?
- Does this require a design decision record?

## Maintenance

- Will future contributors understand when to edit this?

---

# Documentation Header Template

Every major documentation file should include:

```markdown
> Status:
>
> Owner:
>
> Stability:
>
> Last Updated:
>
> Breaking Change Risk:
```

Example:

```markdown
> Status: Stable
>
> Owner: Core Runtime
>
> Stability: Stable
>
> Last Updated: 2026-07-10
>
> Breaking Change Risk: High
```

---

# Documentation Lifecycle

## Draft

Concept is being explored.

Changes are expected.

---

## Experimental

Implementation exists but may change.

---

## Stable

Used by the runtime.

Changes require review.

---

## Deprecated

Kept for reference but no longer recommended.

---

# When Documentation Becomes Outdated

Update documentation when:

- architecture changes
- public APIs change
- terminology changes
- workflows change
- implementation no longer matches description

Documentation debt should be treated like technical debt.

---

# Related Documents

- Architecture.md
- Event-System.md
- Plugin-System.md
- Decision-Engine.md
- API.md
