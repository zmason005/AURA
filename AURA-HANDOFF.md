# AURA Handoff

Paste this at the start of a new conversation. Date: October 1, 2026.

## Who and what

I'm building AURA, the flagship AI product of Stand Alone Braille Solutions. It is an accessible, text-first AI orchestrator for blind and low-vision professionals who use screen readers (VoiceOver, NVDA) and refreshable Braille displays. It was formerly called BRAILLEAI. The repo is zmason005/AURA (to be private). A live skeleton is at zmason005.github.io/AURA, but it is not the final hosting.

AURA is the parent platform. ANLLMS (my animal nutrition chat assistant, currently Flask-based with its own UI) and future products will be AURA plus a customization pack. The goal is the best AI orchestrator for any job AI can do.

## How I want to work

- Explain the reasoning before any implementation.
- Lock decisions one at a time. Ask me a single yes/no on the step you judge most logical, and give next steps one clear action at a time.
- I'm front-end experienced but backend-novice. Use plain language and analogies for backend topics.
- Prefer elegant, table-driven solutions over complex logic.
- Do not generate full code files without asking first. Keep responses token-efficient.
- I work from iOS, so I relay files manually between GitHub and Claude. Claude cannot read the private repo.

## Locked decisions (full detail in BUILD_PLAN.md)

- **D1:** One engine, many profiles. The accessible UI exists once. Products are configurations, never forks.
- **D2:** The UI is drawn from a settings table (AURA defaults, then product profile, then user overrides). Chat requests can only flip approved switches.
- **D3:** The DOM is linear. Users choose visuals freely within an accessibility floor (WCAG limits checked before a change applies). Input box and reset command can never be hidden.
- **D4:** Every response is stored as typed bricks (inline and block tiers). Every brick has a text face. No spatial layout, no autoplay.
- **D5:** New bricks come through a registry table with a device-capability column (for example haptics). Unknown bricks fall back to the text face.
- **D6:** Connector registry (type, login, permissions, confirm-first, output mapping). MCP is the default plug for tool jobs. Other jobs (robot control, RAG, agents, files, sensor streams) use their best-fit protocol. AURA's own credentials stay server-side.
- **D7:** Packs bundle bricks, connectors, and defaults. ANLLMS is AURA plus the farm pack. Domain math runs in deterministic tools (the Flask calculators can be a connector). The AI only presents results.
- **D8:** Hosted on Cloudflare (Pages plus Functions). A second host only when a connector needs a full server.
- **D9:** AURA's AI budget is $0. Users bring their own AI connection (OAuth where supported, otherwise their own API key, kept on their device only in v1). Chat goes through my existing LiteLLM proxy, which also powers ANLLMS. AURA's function uses an AURA-specific proxy key with no company model keys behind it and forwards the user's own key per request.
- **D10:** Proprietary license, all rights reserved.

## Files produced

README.md, LICENSE, BUILD_PLAN.md (saved as AURA-BUILD-PLAN.md here), and this handoff. These need to be committed to the repo by me.

## Build phases

0. Housekeeping (README, summary alignment, heading scheme)
1. Real backend (LiteLLM readiness, proxy key, Cloudflare Pages, secrets, chat function, accessible key-setup walkthrough)
2. Bricks v1
3. Settings table v1 with accessibility guard and reset
4. Connectors v1 (registry, one read-only MCP connector, confirm gate)
5. Real-device accessibility testing (continuous from Phase 2)
6. ANLLMS pack v1

## Open items

- Confirm my LiteLLM proxy is version 1.82 or later and turn on client provider-key forwarding. Anthropic, Google AI Studio, and Azure OpenAI forwarding are documented; OpenAI and Mistral are unconfirmed.
- Confirm AURA's proxy key works without a budget cap blocking it (use rate limits).
- Settle the heading scheme: the architecture summary says h3/h6, the live page uses h5/h6. Fix the "ordered lists (ul)" wording.
- Decide where user settings are stored and when to add user accounts (also needed for a future server-side key vault).
- Pick the first MCP connector.
- Confirm the exact legal entity name for the copyright line, and that the company holds the rights to the code.

## Where we stopped

Phase 0 files are drafted. Next single action: check the LiteLLM proxy's version and whether client provider-key forwarding is available and enabled. Do not create a $0-budget key.
