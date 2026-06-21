# Contribution #302: Adapter: xAI Grok Model Provider

**Contribution Number:** [1 / 2 / 3]  
**Student:** Kyle Shi 
**Issue:** [Link](https://github.com/orthogonalhq/nous-core/issues/302)
**Status:** Phase 3 Complete

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
The process of setting up the dev dependencies was very straightforward. The only thing required was using pnpm to install them, though that meant I had to install the pnpm package itself first. Unexpectedly, there were no errors when installing the dev dependencies, however, setting up the repository presented several challenges. For one, we had a dedicated branch that the maintainer created for us, but I couldn't find it initially. When I checked my current Git configuration, I noticed it was pointing to my local fork rather than the remote view of the main repository. After adding the original repository as an upstream remote and fetching it, I was able to switch directly to the correct branch and start my work.

### Steps to Reproduce

There is no bug to reproduce the goal of this issue is to develop a xAI adapter so data in nous-core shape can be sent to xAI and retrieved.

### Reproduction Evidence

There is no bug to reproduce so this is N/A

---

## Solution Approach

### Analysis

Although there is no root bug, the response shape that xAI follows mirrors OpenAI's structure. This allows me to utilize the existing template and architecture to create my version of the xAI adapter

### Proposed Solution

Create the invoke, stream, and getConfig functions by first defining the contracts so the automated script knows how to interact with xAI, then send and collect tokens via lazy streams.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
We are creating an adapter for xAI so when a user types in a prompt it can be sent to their respective AI models, and retrieve the response back without breaking.

**Match:** 
The OpenAI and Anthropic folders contain functions and implementations similar to what I will be developing.

**Plan:** 
1. Fill in the contracts for definition.ts, adapter.ts, provider.ts, index.ts so the automated scripts knows the structure of the xAI schema as well as meta data needed to connect to the actual xAI api.
2. Regenerate catalogs so the main script can access the adapter with a clean connecetion.
4. Check for any type, lint, or any other errors with the given checks.
5. Create test

**Implement:** [[Link]](https://github.com/KyleS88/nous-core/tree/feat/contributor-friendly-inference-provider-surface)

**Review:** 
1. I would check to make sure that my code is decoupled from the main engine and cortex ensuring that the scope of what my files have access are not accessing anything other then the edges(leafs). Also I will have a side by side reference of the anthropic template code to ensure that I am not missing anything important for the function.
2. For testing I will make sure to pnpm test to ensure that 5 specific things work:model definitions, API key/credentials handling, provider setup, model picker, and the actual behavior of the invoke and stream functions.
3. For commit I will make sure it follows the ```feat(subcortex): add xai model provider leaf``` structure.

**Evaluate:**
The following automated test will check if my new adapter follows the proper convetions as described by the CONTRIBUTION.md
Testing functionality and type
```
pnpm --filter @nous/subcortex-providers run typecheck
pnpm --filter @nous/subcortex-providers exec vitest run src/__tests__/provider-codegen.test.ts src/__tests__/public-exports.test.ts src/__tests__/provider-definitions src/__tests__/adapter-resolver.test.ts src/__tests__/provider-pipeline-integration.test.ts --config vitest.config.ts
```
Testing documentation
```
pnpm --filter docs lint
pnpm --filter docs build
```
---

## Testing Strategy

### Unit Tests

- [ ] ProviderDefinitionSchema validates the xAI definition — covered by provider-definitions.test.ts
- [ ] vendorKey, adapterKey, protocol, auth metadata, endpoint, and default model are correct — covered by provider-definitions.test.ts
- [ ] wellKnownProviderId is derived from vendorKey, not hand-authored — covered by provider-definition-types.test.ts
- [ ] Adapter resolves correctly for vendor: 'xai' — covered by adapter-resolver.test.ts
- [ ] parseResponse returns fallback without throwing on malformed output — covered by provider-pipeline-integration.test.ts

### Integration Tests

- [ ]  xAI provider registered and constructed correctly with env-var credentials — covered by provider-pipeline-integration.test.ts
- [ ]  Missing XAI_API_KEY skips provider construction correctly — covered by provider-pipeline-integration.test.ts
- [ ]  Generated catalogs include xAI leaf in deterministic order — covered by provider-codegen.test.ts

### Manual Testing

There is no exact manual test, the maintainer has given us the following script to test our code 
```
pnpm --filter @nous/subcortex-providers run check:generated
pnpm --filter @nous/subcortex-providers run typecheck
pnpm --filter @nous/subcortex-providers exec vitest run src/__tests__/provider-codegen.test.ts src/__tests__/public-exports.test.ts src/__tests__/provider-definitions src/__tests__/adapter-resolver.test.ts src/__tests__/provider-pipeline-integration.test.ts --config vitest.config.ts
```

---

## Implementation Notes

### Week 1 Progress

I added xAi leaf, but the testing fails due to a strict testing and lax use of raw arrays to check for what protocols are needed. I commented this issue and I am still waiting for a response from the maintainer.

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
