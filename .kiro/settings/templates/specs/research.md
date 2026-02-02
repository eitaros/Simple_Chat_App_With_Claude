# Research & Discovery Log: {{FEATURE_NAME}}

## Summary
Brief overview of the discovery scope and key findings.

## Research Topics

### Topic 1: [Investigation Area]
**Question**: What we needed to find out
**Sources**:
- [URL or reference 1]
- [URL or reference 2]

**Findings**:
- Key finding 1
- Key finding 2

**Implications for Design**:
- How this impacts technical decisions

---

### Topic 2: [Investigation Area]
**Question**: What we needed to find out
**Sources**:
- [URL or reference 1]

**Findings**:
- Key finding 1

**Implications for Design**:
- How this impacts technical decisions

## Technology Decisions

### Decision 1: [Technology Choice]
**Options Considered**:
1. Option A - Pros/Cons
2. Option B - Pros/Cons

**Selected**: Option [X]

**Rationale**: Why this option was chosen based on requirements and constraints

**Verification**:
- Version compatibility confirmed
- Known issues reviewed
- Migration path documented (if applicable)

---

### Decision 2: [Technology Choice]
**Options Considered**:
1. Option A - Pros/Cons

**Selected**: Option [X]

**Rationale**: Why this option was chosen

## Architecture Patterns Evaluated

### Pattern 1: [Pattern Name]
**Description**: Brief description of the pattern
**Pros**: Benefits for this project
**Cons**: Drawbacks or limitations
**Fit Assessment**: How well it matches requirements

### Pattern 2: [Pattern Name]
**Description**: Brief description of the pattern
**Pros**: Benefits for this project
**Cons**: Drawbacks or limitations
**Fit Assessment**: How well it matches requirements

**Selected Pattern**: [Pattern Name]
**Rationale**: Why this pattern was chosen

## External Dependencies

### Dependency 1: [API/Library Name]
**Purpose**: What it will be used for
**Version**: Specific version to use
**Documentation**: [URL]
**Key Constraints**:
- Rate limits, quotas, or usage restrictions
- Breaking changes in recent versions
- Compatibility requirements

**Integration Points**: Where it connects to our system

---

### Dependency 2: [API/Library Name]
**Purpose**: What it will be used for
**Version**: Specific version to use
**Key Constraints**:
- Important limitations

## Risks Identified

### Risk 1: [Risk Name]
**Description**: Detailed description of the risk
**Likelihood**: High/Medium/Low
**Impact**: High/Medium/Low
**Mitigation Strategy**: How design addresses this
**Source**: Where this risk was identified

---

### Risk 2: [Risk Name]
**Description**: Detailed description of the risk
**Likelihood**: High/Medium/Low
**Impact**: High/Medium/Low
**Mitigation Strategy**: How design addresses this

## Design Boundaries & Parallelization

### Component Independence Analysis
**Independently Developable Components**:
- Component A: Can be developed in parallel (dependencies: none)
- Component B: Can be developed in parallel (dependencies: Component A interfaces only)

**Sequential Dependencies**:
- Component C must wait for Component A completion
- Component D requires Component B and C integration

**Critical Path**: [A → C → Integration]

### Integration Points
**External Boundaries**:
- User Interface: [Details]
- External APIs: [Details]

**Internal Boundaries**:
- Layer 1 ↔ Layer 2: [Interface contract]
- Service A ↔ Service B: [Interface contract]

---

*Last Updated: {{TIMESTAMP}}*

## Template Usage Notes (Remove before final output)

This research log captures:
1. **Discovery process**: What was investigated and why
2. **Technology decisions**: Options evaluated and rationale for choices
3. **External dependencies**: Verified APIs, libraries, versions, constraints
4. **Risks**: Identified issues and mitigation strategies
5. **Design boundaries**: Component independence for parallel development

Use this log to:
- Document research process transparently
- Justify technical decisions with evidence
- Reference in design.md for key decisions
- Track risks and mitigation strategies
- Plan parallel implementation tasks

**Remove this section before final output.**
