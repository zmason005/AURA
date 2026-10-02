# AURA Build Plan

Stand Alone Braille Solutions. Draft v0.1, October 1, 2026. Plan only; no code.

## 1. What AURA is

AURA is an accessible, text-first AI orchestrator for blind and low-vision professionals who use screen readers (VoiceOver, NVDA) and refreshable Braille displays. It is the parent platform. ANLLMS and future products are AURA plus a customization pack, not separate apps.

Goal: the best AI orchestrator for any job AI can do, with every result delivered in a clean, predictable, Braille-safe form.

## 2. Locked decisions

These are locked for now and may be revisited in future versions.

**D1. One engine, many profiles.** The accessible UI exists once, in AURA. Each product is a profile (configuration), never a fork.

**D2. Settings table, three layers.** The UI is drawn from a settings table: AURA defaults, then product profile, then user overrides. Chat requests to change the UI can only flip approved switches. AURA never generates arbitrary page code. Requests with no matching switch are declined and logged as feature requests.

**D3. Linear DOM, free visuals, accessibility floor.** The DOM is linear. Users choose how the app looks, but visual order, reading order, and focus order must match. Every switch has allowed limits (for example minimum text contrast, minimum text size, always-visible focus, minimum tap size), checked before a change applies. The limits are a floor, not a ceiling. A reset-layout command and the input box can never be hidden.

**D4. Typed bricks.** Every response is stored as data in typed bricks and drawn only through them. Two tiers: inline pieces (emphasis, link, code, citation marker, language tag, raw braille text) and blocks (text, lists, choices, data, media, input, system, structure). Every brick must carry a text face, the plain-text version a Braille display can always show. No spatial layout. No autoplay.

**D5. Brick registry.** New bricks are added through a registry table: fields, text face rule, renderer, accessibility test, and a device-capability column (for example haptics). If a device lacks the capability, or a client does not know a brick, it shows the text face.

**D6. Connector registry.** All tools plug in through a registry table: type, login method, permissions, confirm-first flag, and output-to-bricks mapping. MCP is the default plug for tool-style jobs. Other job types (robot control, RAG, agents, files, sensor streams) use their best-fit protocol behind the same registry. All results pass through the normalizer into bricks. AURA's own credentials stay server-side, never in the browser or a public repo. A user's personal provider key may be kept on their own device (see D9).

**D7. Packs.** Domain features ship as packs: bricks, connectors, and defaults. ANLLMS is AURA plus the farm pack. Domain math (for example diet optimization) runs in deterministic tools; the AI only presents the results, with units, sources, and assumptions shown. Sensor streams are watched by the backend and surface only as threshold alerts, not continuous updates.

**D8. Hosting.** AURA is hosted on Cloudflare: Pages for the front end and Pages Functions for the chat proxy. A second host is added only when a connector needs a full server (for example the Flask calculators, which stay where they are today and plug in as a connector).

**D9. Bring your own AI connection.** AURA's own AI budget is $0.00. AI model providers (Anthropic, Gemini, OpenAI, Mistral, and others) are user-configured connectors in the registry, listed in a provider table (endpoint, how it authenticates, available models). Users connect through an OAuth account connection where a provider supports it, otherwise by adding their own API key. In v1 a user's personal key is kept on their own device only; AURA's function passes it through and never stores or logs it. A server-side encrypted key vault is deferred until user accounts exist. Because chat subscriptions are generally not API access, and MCP connects tools rather than models, the guided setup for adding a key must itself be fully accessible.

Chat requests go through the existing LiteLLM proxy, which also powers ANLLMS. AURA's Cloudflare function authenticates to LiteLLM with an AURA-specific proxy key that has no company model keys behind it, so it cannot spend company money, and it forwards the user's own provider key with each request. This relies on LiteLLM's client-key forwarding (version 1.82 or later, setting enabled). The browser never sees the LiteLLM address or AURA's proxy key.

**D10. License.** AURA is proprietary: Copyright (c) 2026 Stand Alone Braille Solutions, all rights reserved (see LICENSE). Copies that people already received under the earlier MIT license stay under MIT. Open-sourcing later remains possible.

## 3. Architecture layers

1. UI shell: the three-region layout (conversation, index, input dock) with settings-driven styling.
2. Orchestrator: decides which tool to call and in what order.
3. Connectors: MCP servers and gateways, APIs, agents, storage, domain tools.
4. Normalizer: converts any tool output into bricks. This is the core product value.

## 4. Build phases

Each phase ends with something to test by hand with a screen reader or Braille display before the next begins.

**Phase 0. Housekeeping.** Rewrite the README. Update the architecture summary to match these decisions. Settle the heading scheme (the summary says h3/h6, the live page uses h5/h6). Fix the "ordered lists (ul)" wording.
Done when: README and summary match this plan.

**Phase 1. Real backend.** In order: (1) confirm the LiteLLM proxy version and enable client-key forwarding; (2) create AURA's own proxy key, with no company model keys behind it; (3) connect Cloudflare Pages to the repo; (4) store the LiteLLM address and AURA's proxy key in Cloudflare's secret settings; (5) add the chat function, which forwards the user's own provider key without storing or logging it; (6) build an accessible, guided walkthrough for connecting a provider.
Done when: a user connects their own provider through the walkthrough, a typed prompt gets a real reply through LiteLLM, and no user key appears in the repo or on AURA's server.

**Phase 2. Bricks v1.** Paragraph, heading, list, choices, table, status, each with a text face. Responses stored as data. Buffer a full response before inserting it, to avoid reflow on Braille displays.
Done when: the same stored response can be drawn as a list or as checkboxes.

**Phase 3. Settings table v1.** Three switches (hide index, checkboxes for choices, text size), the accessibility guard, a reset command, and calm confirmations with stable focus.
Done when: a chat request flips a switch, an out-of-limit request is declined with an explanation, and reset always works.

**Phase 4. Connectors v1.** The registry, one read-only MCP connector, and the confirm-before-acting gate.
Done when: AURA answers using a real tool and presents the result as bricks.

**Phase 5. Real-world accessibility testing.** VoiceOver, NVDA, and a Braille display. This also runs continuously from Phase 2 on.
Done when: all Phase 1 to 4 behavior passes on real devices.

**Phase 6. ANLLMS pack v1.** The current ANLLMS chat assistant as a profile, one ration table brick, and the existing Flask calculator as the first domain connector.
Done when: ANLLMS runs as a profile of AURA with no separate UI.

## 5. Out of scope for v1

Haptics, video, robot control, sensor streams, CLI orchestration, and the full list of bricks beyond Phase 2.

## 6. Open questions

- LiteLLM readiness: confirm the proxy is version 1.82 or later and turn on provider-key forwarding. Anthropic, Google AI Studio, and Azure OpenAI forwarding is documented; OpenAI and Mistral are unconfirmed. Also confirm AURA's proxy key works without a budget cap blocking it (use rate limits instead).
- Which MCP connector comes first.
- Where user overrides are stored, and whether users need accounts.
- How the architecture notes get relayed to Claude while the repo is private (suggested: keep ARCHITECTURE.md and this plan in the repo and paste them into sessions).

## 7. Risks

- Scope creep from the "largest orchestrator" ambition. Mitigation: prove each foundation in a thin slice first.
- Automated checks cannot judge things like alt-text quality. Mitigation: real-device testing and human review.
- Write actions through connectors. Mitigation: confirm-first on anything that writes, sends, or deletes.
- Free-tier hosting limits. Mitigation: verify current limits before launch and budget for the paid tier.
