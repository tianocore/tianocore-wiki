# RFC: [Your RFC Title]

## Metadata

- **RFC Number**: TBD
- **Title**: [Your RFC Title]
- **Status**: Draft

## Change Log

- [YYYY-MM-DD]: Initial RFC created
- [YYYY-MM-DD]: Updated based on community feedback (describe changes)

## Motivation

Explain why this change is needed. What problem does it solve? What use cases does it enable?

Be specific and provide examples of the pain points that motivate this RFC.

## Technology Background

Provide relevant background information that reviewers need to understand your proposal:

- Related technologies or concepts
- Existing approaches in other projects
- Relevant standards or specifications
- Historical context for this problem domain

## Goals

List the specific goals this RFC aims to achieve. What defines success for this proposal?

1. Goal 1
2. Goal 2
3. Goal 3

## Requirements

List specific requirements that the solution must satisfy:

1. Requirement 1
2. Requirement 2
3. Requirement 3

## UEFI/PI Specification Impact

Describe how this change relates to UEFI or PI specifications:

- Does it impact specification behavior?
- Does it require specification changes?
- Does it extend specifications in a compatible way?
- Is it orthogonal to specifications?

If a Code First issue related to this RFC exists, link to it here.

## Backward Compatibility

Analyze the impact on existing code and deployments:

- Breaking changes: List any incompatibilities
- Migration path: How can users/platforms upgrade?
- Deprecation: What will be deprecated and when?
- Compatibility layer: Is such a layer needed?

## Platform/Package Impact

List which packages relevant to this RFC and platform integration impact.

## Unresolved Questions

List open questions that need community input:

- Question 1?
- Question 2?
- Question 3?

Note: These questions will be resolved during the RFC review process.

## Prior Art/Related Work

Describe similar work in other projects or previous attempts:

- How have other projects solved this problem?
- What can we learn from their approaches?
- Have there been previous attempts in TianoCore?
- Reference relevant papers, RFCs, or discussions

## Alternatives

Describe alternative approaches you considered and why they were not chosen:

### Alternative 1: [Description]

- **Pros**: List advantages
- **Cons**: List disadvantages
- **Why not chosen**: Explain the decision

### Alternative 2: [Description]

- **Pros**: List advantages
- **Cons**: List disadvantages
- **Why not chosen**: Explain the decision

## Implementation Design

Provide detailed technical design of your proposal. This is the core of your RFC.

### Architecture Overview

High-level architecture description. Consider including (using mermaid or similar diagrams):

- Component diagrams
- Data flow diagrams
- Sequence diagrams

### Detailed Design

Detailed technical specification including interfaces, algorithms, key data structures, etc.

### Code Examples

Provide code examples showing how the code will be used. For example, key touch points for a platform to configure
and use the code.

```c
// Example usage
```

## Testing Strategy

Describe how the implementation will be tested such as unit tests, integration tests, validation tests, performance,
and security tests.

## Migration/Adoption Plan

Describe the rollout strategy:

1. **Phase 1**: Initial implementation (what's included?)
2. **Phase 2**: Adoption by platforms (migration steps?)
3. **Phase 3**: Deprecation of old approach (if applicable)
4. **Phase 4**: Long-term maintenance

Include:

- Timeline estimates
- Dependencies on other work
- Risk mitigation strategies

## Guide-Level Explanation

Explain the feature to different audiences:

### For Package Developers

How will package developers use this feature? Provide details like:

- Practical examples
- Common patterns
- Best practices

### For Platform Developers

How does this impact platform integration? Provide details like:

- Integration steps
- Configuration options
- Platform-specific considerations

### For End Users (if applicable)

How does this affect firmware behavior from a user perspective?

- Visible changes
- New capabilities
- Required user actions
