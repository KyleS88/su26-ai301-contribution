# Contribution #302: Adapter: xAI Grok Model Provider

**Contribution Number:** [1 / 2 / 3]  
**Student:** Kyle Shi 
**Issue:** [Link](https://github.com/orthogonalhq/nous-core/issues/302)
**Status:** Phase 4 Complete
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

### Week 2 Progress
Built on top of week 1's progress given week 1 was still pending maintainer response.
Built the four required leaf files (definition.ts, adapter.ts, provider.ts, index.ts). Key issues fixed along the way:

Wrong type (ProviderDefinition → ProviderDefinitionLeaf)
Hand-authored UUID removed — IDs are auto-derived from vendorKey
Added fail-closed credential check after reading maintainer feedback on PR #398, preventing accidental OPENAI_API_KEY fallback

Upstream added 9 new providers simultaneously from other contributors, resolved merge conflicts by re-running the generator for catalog files and manually merging test files. Updated hardcoded vendor lists across 5 existing test files to include xAI.
Updated provider for leaf that calls on xAI fallback. Added manual xAI insertions into the designated test cases to ensure that all maintainer test includes xAI leaf node. 

### Week 3 Final
Updated xAI model to latest Grok 4.3
Implemented manual end-to-end test for registry registration, definition hydration, schema validation, auth contract, adapter parsing, and factory security boundaries
Submitted PR #1

### Code Changes

- **Files modified:** 
- [ ]  self/subcortex/providers/src/providers/xai/definition.ts
- [ ]  self/subcortex/providers/src/providers/xai/adapter.ts
- [ ]  self/subcortex/providers/src/providers/xai/provider.ts
- [ ]  self/subcortex/providers/src/providers/xai/index.ts
- [ ]  src/provider-adapters.ts
- [ ]  provider-definitions.ts
- [ ]  provider-factories.ts
- [ ]  src/__tests__/provider-codegen.test.ts
- [ ]  src/__tests__/provider-definition-types.test.ts
- [ ]  src/__tests__/provider-definitions/provider-definitions.test.ts
- [ ]  src/__tests__/adapter-resolver.test.ts
- [ ]  src/__tests__/provider-pipeline-integration.test.ts
- [ ]  src/__tests__/providers/xai.test.ts

- **Key commits:** 
feat(sub-cortex):Added xai leaf nodes
- **Approach decisions:**
Added a fall back on xai api because we currently don't have any connected apis so fall back will ensure that an ai model will be routed.

---

## Pull Request

**PR Link:** [[Link to PR]](https://github.com/orthogonalhq/nous-core/pull/424)

**PR Description:** 
## Summary
Add xAI Grok as a provider leaf under the current provider-leaf contract (issue #302). Reuses the shared OpenAI-compatible Chat Completions protocol, thus no custom implementation.ts needed. Includes fail-closed credential handling so the factory never falls back to OPENAI_API_KEY when XAI_API_KEY is absent.

## Linked Issue
Closes #302 

## Changes
- Add `self/subcortex/providers/src/providers/xai/` leaf: `definition.ts`, `adapter.ts`, `provider.ts`, `index.ts`
- Definition uses `ProviderDefinitionLeaf` (metadata only); built-in id derives from `vendorKey`, no hand-authored `wellKnownProviderId``
- Reuse shared `chat-completions` protocol
- Factory resolves `XAI_API_KEY` explicitly and fails closed rather than passing `undefined` downstream
- Regenerate provider catalogs (`provider-definitions`, `provider-adapters`, `provider-factories`)
- Update registry-wide test expectations to include the `xai` vendor
- Add `__tests__/providers/xai.test.ts` covering registry registration, definition hydration, schema validation, auth contract, adapter parsing, and factory security boundaries
- Set default xAI Grok model to grok-4.3

## Verification
- [x] Tests pass (`pnpm test`)
- [x] Lint passes (`pnpm lint`)
- [x] Typecheck passes (`pnpm typecheck`)
- [x] Build passes (`pnpm build`)
- [x] Manually ran the change end-to-end and confirmed it behaves as described

### Manual behavior check

Ran the full provider test suite confirming xAI is correctly discovered, registered, and integrated end-to-end through the real `ProviderRegistry`:

```
pnpm --filter @nous/subcortex-providers exec vitest run src/__tests__/provider-codegen.test.ts src/__tests__/public-exports.test.ts src/__tests__/provider-definitions src/__tests__/adapter-resolver.test.ts src/__tests__/provider-pipeline-integration.test.ts src/__tests__/providers/xai.test.ts --config vitest.config.ts
```
<img width="1402" height="299" alt="image" src="https://github.com/user-attachments/assets/91cd6664-5677-4bca-ba06-efbe84d788ae" />
<img width="1183" height="37" alt="image" src="https://github.com/user-attachments/assets/c8faaede-3423-473b-98cb-8d1e39f08b63" />
<img width="1798" height="256" alt="image" src="https://github.com/user-attachments/assets/d0aaaa9b-2a39-4fb9-990d-efc384fb3db6" />

`Note: Live end-to-end invoke test not included — no xAI API key available, consistent with other provider leaf reviews where the maintainer also noted they could not run live tests without the provider's API key.`

## Checklist
- [x] Branch follows flow: targets feat/contributor-friendly-inference-provider-surface per maintainer note on issue #302)
- [x] Commits follow [Conventional Commits](https://www.conventionalcommits.org/)
- [x] Docs updated if behavior changed (or N/A)


**Maintainer Feedback:**
- July 8, 2026: 
The implementation looks good overall, but there is a bug causing xAI API requests to use incorrect URLs with a duplicated /v1. Fix the endpoint configuration so chat completions and model listing use the correct URLs. Also retarget the PR from main to feat/contributor-friendly-inference-provider-surface. There are a few trailing whitespace issues that should be cleaned up as well.
- July 14, 2026:
  I addressed this by doing the following
[x] Updated definition.ts to use https://api.x.ai (removed /v1) to eliminate doubled paths
[x] Updated test expectations in provider-definitions.test.ts to match
[x] Fixed trailing whitespace
[x] Retargeted PR to feat/contributor-friendly-inference-provider-surface
- July 15, 2026
Succesfully merged to `feat/contributor-friendly-inference-provider-surface`.

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

- Able to read large code base and understand
- Write test both unit and intergration
- Understand how we can use automation scripts to update central catalogs
- Archetectural strategies use to make a code base more robust and modular
- Learn how to fix merged conflicts
- Analyze and understand other people's code

### Challenges Overcome

The hardest part was understanding what I needed to change manually. Although there weren't many details regarding manaul updates to test files and specific code I was able to utilize other peoples code to match off, and thus guide my approach when developing this new adapter.

### What I'd Do Differently Next Time

My current process started with me just diving into the code base after a brief review. Although implementing wasn't challenging by any means I had a hard time understand what else besides the adapter I needed to change, what test I needed, and how to use that test to what I need to change. Rather next time in addition to reviewing I would also use other people's PR to try to understand their approach that got their code merged, as well as asking more questions to the maintainers.

---

## Resources Used

- (https://docs.nue.orthg.nl/docs/development/provider-adapters/quickstart
- https://docs.nue.orthg.nl/docs/development/provider-adapters/provider-leaf-anatomy
- https://docs.nue.orthg.nl/docs/development/provider-adapters/schemas-abi-reference)
- https://github.com/orthogonalhq/nous-core/issues/308
- https://github.com/orthogonalhq/nous-core/issues/305
