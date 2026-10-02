# AURA

AURA is an accessible, text-first AI orchestrator built by Stand Alone Braille Solutions for blind and low-vision professionals who use screen readers (VoiceOver, NVDA) and refreshable Braille displays.

Modern AI tools and enterprise software are visually dense and constantly changing, which floods screen readers with noise and disrupts the reading line on a Braille display. AURA sits between those tools and the user. It takes whatever a tool returns and presents it as clean, predictable, standardized text structures.

AURA is the parent platform. Other products, such as ANLLMS, are AURA plus a customization pack, not separate apps.

## Status

Early development. The front-end skeleton exists. The backend, response format, settings system, and connectors are planned. See [BUILD_PLAN.md](BUILD_PLAN.md) for the roadmap and locked decisions.

## Core ideas

- **One engine, many profiles.** The accessible interface exists once. Each product is a configuration of it.
- **Typed bricks.** Every response is stored as data in typed content blocks, and each block carries a plain-text version that a Braille display can always show.
- **User-customizable, accessibility-safe.** Users can tell AURA how they want the interface to look and behave. Every change is checked against accessibility limits first, and a layout reset is always available.
- **Registry-based connectors.** MCP servers and gateways, APIs, agents, and storage plug in through a connector registry, with confirmation required before any action that writes, sends, or deletes.
- **Linear structure.** The page is a single reading order: conversation, conversation index, input dock.

## Hosting and security

AURA is hosted on Cloudflare. AURA's own credentials live only in the host's secret settings and are never committed to this repository or exposed to the browser.

AURA does not supply AI. Users connect their own AI provider accounts or API keys, and chat requests are routed through a LiteLLM gateway. A user's personal key is kept on their own device and passed through with each request; AURA does not store or log it.

## Repository layout

- `index.html`: the page
- `css/`: styles
- `js/`: client scripts, including the response parser
- `BUILD_PLAN.md`: build plan and locked decisions

## License

Proprietary. Copyright (c) 2026 Stand Alone Braille Solutions. All rights reserved. See [LICENSE](LICENSE).
