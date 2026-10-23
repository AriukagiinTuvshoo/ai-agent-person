# AI Agent Language and Provider Audit

Date: 2026-10-23

## Verified findings

- The checked-in `agent/agent.ts` selects `anthropic/claude-sonnet-5`.
- The checked-in `agent/instructions.ts` defines the system instructions and previously only gave French and English as explicit language examples.
- Therefore, the repository configuration does **not** currently show a Google Gemini model selection. The running local app may differ from this branch, or the model may be overridden by runtime/provider configuration; this requires deployment/runtime verification.
- A successful provider request only proves that a request completed. It does not prove that Mongolian output is linguistically correct.
- The project uses Nuxt 4 and Eve, with a separate agent runtime. Provider changes must follow Eve's supported model/provider configuration rather than assuming a generic SDK interface.

## Changes in this branch

- Added explicit Mongolian Cyrillic language-quality instructions to the agent's base system prompt.
- Clarified language matching, preservation of code/identifiers, and avoidance of repeated filler.
- Did not change the model ID because the intended provider credentials, exact supported Gemini model ID, runtime override behavior, and live API response have not yet been verified.
- Did not claim runtime tests or a production deployment were successful; this repository-only change has not been executed in a local runtime.

## Required next verification

1. Inspect deployment environment and Eve model/provider settings without exposing secrets.
2. Confirm whether the production runtime uses `anthropic/claude-sonnet-5`, Gemini, or an override.
3. Add a reproducible Mongolian evaluation set covering ordinary chat, translation, technical explanation, long answers, repeated output, and streaming.
4. Compare raw provider output with the final UI text to distinguish model-quality issues from duplicate/incorrect stream rendering.
5. Confirm supported model IDs and provider options from Eve's current documentation before adding Gemini, OpenAI, or other providers.
6. Run typecheck, lint, build, and integration tests in an environment with the project's required Node version and dependencies.
7. Treat 3D redesign and multi-provider routing as separate implementation phases after the baseline chat behavior is measured.

## Acceptance criteria

- The displayed provider/model matches the provider/model actually called.
- Mongolian responses use natural, coherent Cyrillic by default when the user writes in Mongolian.
- Streaming output does not duplicate interim and final text.
- Provider errors, rate limits, and timeouts are visible as errors rather than being presented as valid answers.
- Tests and deployment status are reported with evidence; unrun checks are explicitly marked as unrun.
