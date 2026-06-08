# Contribution #302: Adapter: xAI Grok Model Provider

**Contribution Number:** [1 / 2 / 3]  
**Student:** Kyle Shi 
**Issue:** [Link](https://github.com/orthogonalhq/nous-core/issues/302)
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because I want to understand and work on AI model integration within a broader agent host. I have experience working with local AI models, but I haven't connected an application to external, hosted AI models yet. By contributing to this project, I hope to learn how to bridge these external connections while keeping the architecture as abstract and provider-agnostic as possible. Additionally, since this open-source project aligns perfectly with my current tech stack (TypeScript/JavaScript), it allows me to focus fully on learning these integration patterns without the added friction of learning a new language for my first contribution.

---

## Understanding the Issue

### Problem Description

An adaptor is needed to parse user data into nous-core data shape and ultimtely into xAi api shape needed to evaluate the prompt. The end product is to build an adaptor that handles invoke, stream, and getConfig.

An adapter is needed to parse user input into the nous-core unified data shape and ultimately translate it into the xAI API schema required to evaluate prompts. The final deliverable is a dedicated model provider adapter that implements the core IModelProvider interface, handling invoke, stream, and getConfig. 

### Expected Behavior

User Input -> nous-code data shape -> Zod Schema Validation -> xAi Payload mapping -> secured fetch-> stream data as chunks and parsing -> returning final response

### Current Behavior

User Input -> nous-code data shape

### Affected Components

self/subcortex/providers/src/grok-provider.ts

A new grok-provider.ts file with the functions of invoke, stream, and getConfig

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
