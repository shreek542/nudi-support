# Nudi Support

**Your store, in every conversation.**

Update: typed language switching and an owner-operated human-support inbox are now included. See [human handover guide](docs/HUMAN_SUPPORT.md), [Intercom feature comparison](docs/INTERCOM_COMPARISON.md) and [proposed pricing experiments](docs/PRICING_PLAN.md).

A self-hosted customer-support workspace for **Kannada, Tamil, Telugu, Malayalam, English and Hindi**. Bring your own AI key, add business knowledge, customise the chat widget and embed it on your website.

Built as an open-source portfolio project by **Shree Krishna R**, with AI development assistance. MIT licensed. The fictional Orange Thread clothing store is included for safe exploration.

## Start locally

Install **Node.js 24 or newer**. No npm packages or Python environment are required.

```bash
git clone https://github.com/shreek542/nudi-support.git
cd nudi-support
npm start
```

Open **http://localhost:3000**. The terminal prints your randomly generated admin password. Enter it on the login screen. Keep Terminal open while using the application.

Your database and generated encryption secret are saved in `.nudi/` (excluded from Git). Back up this directory securely. Losing or replacing the encryption secret makes saved API keys unreadable. Never upload it to GitHub.

Alternatively, with Docker Compose:

```bash
docker compose up --build -d
docker compose logs nudi
```

The named volume preserves your data. The Compose file binds the app to localhost; put it behind an HTTPS reverse proxy for public use. See [deployment guide](docs/DEPLOYMENT.md).

## What is implemented

| Area | Features |
| --- | --- |
| Appearance | Store name, assistant name, PNG/JPG/WebP logo, colour, typography, button text/shape/position, greeting and suggested questions |
| Preview | Clothing-store preview, mobile-width preview and interactive test chat |
| Languages | Six language choices, script detection, manual override and language changes within a chat |
| Knowledge | Create/edit/delete sources; text/Markdown/CSV import; optional public-page source with manual refresh |
| Connections | Gemini and OpenAI keys, encrypted server storage, configurable model ID, small live connection test |
| Chat | Source excerpts, recent conversation context, missing-information fallback and optional human-contact link |
| Inbox | Persistent customer threads, waiting/human/AI/closed states, operator replies, private notes and a separate unanswered-question list |
| Installation | Embeddable script, inline iframe and guidance for Hostinger, WordPress, Wix and GoDaddy |
| Usage | Atomic daily business-answer request limit and per-minute chat limits; UTC-day counter |
| Export | Settings and documents as JSON, without API keys or unanswered questions |
| Administration | Password login, signed HttpOnly cookie, same-origin mutations and protected endpoints |

## Demo vs live AI

**Demo mode is not an AI model.** It returns prewritten translations for four unchanged fictional sample topics: shipping, returns, sizes and catalogue. Editing a sample policy disables its canned answer, preventing an old translation from overriding new facts. Greetings, thanks, acknowledgements, goodbyes and identity questions are handled separately in all six languages without an AI key or policy citation. Unsupported business questions use the missing-information response.

To use your own knowledge:

1. Open **Connections**, select Gemini or OpenAI and save your API key.
2. Enter a model ID available to your account. Defaults are examples, not a guarantee of provider availability or pricing.
3. Click **Test connection**. This makes a small real API request and may incur a charge.
4. Choose the matching live answer mode and click **Save changes**.
5. Replace demo knowledge and test it with real business questions before deployment.

Live chat sends selected source excerpts, the question and up to six recent messages to the selected provider. The provider bills your account and processes that information under its own terms. No AI subscription, API credit or key is bundled.

All six language interfaces and sample responses are included. **Live language accuracy and mixed/romanised-language understanding depend on your model and need native-speaker evaluation.** Automatic script detection does not automatically identify every romanised language. Select a language manually when appropriate.

## How retrieval works

This release uses **lightweight lexical/topic ranking**, not vector embeddings. For libraries up to eight documents, all sources are considered within a bounded context; larger libraries use topic/title and word matches. At most eight sources and 36,000 characters are selected, with up to 6,000 characters per source. Split long policies into focused documents and evaluate retrieval before expanding the library.

The model returns JSON with answer text, source IDs and an unanswered flag. Unknown source IDs are rejected. If no valid cited source remains, the application shows a fallback instead of an unsupported answer. **A valid citation is not proof that the answer is correct**; this is not a formal entailment or policy-compliance verifier. Important answers require review.

## Website integration

Host the backend on a compatible server over HTTPS. Your website can stay on its existing platform.

1. Open **Install** and add your website origin, for example `https://myshop.com`.
2. Save settings.
3. Copy the loader into your site's supported custom-code location.
4. Where scripts are restricted, use the iframe alternative.
5. Test on the published website, including mobile.

The widget code contains a public assistant URL, **never your API key**. The widget loads the latest saved appearance when the page loads. Existing open widgets update on reload.

Website-builder scripts/iframes can have plan, sandbox and content-security-policy restrictions. Compatibility must be tested on the target site. A section-level embed is not necessarily a floating widget across every page. A native WordPress plugin is not included.

An owner-private Sites preview is for your review; it cannot serve anonymous visitors on another website. The standalone self-hosted version provides that path. GitHub hosts source code, not the running backend.

## Source refresh

A knowledge document can reference a final HTTPS URL on an allowed origin. Click its refresh button to import readable HTML/text. Redirects are not followed; pages over 300 KB are rejected. The parser removes basic scripts/styles/navigation and imports up to 20,000 characters. JavaScript-rendered pages, PDFs and authenticated pages require a text upload.

Refresh replaces the document text immediately. Review what was imported. **Automatic scheduled sync, crawl discovery, changed-policy approval and live inventory are not implemented.** An AI key alone cannot enable them.

## Development and tests

```bash
npm test
npm run build
npm run validate
```

The application uses browser-native HTML/CSS/JavaScript and a standard Fetch API backend. SQLite runs locally through Node's built-in module; the private hosted preview uses Cloudflare D1. There are no third-party npm runtime dependencies.

`npm run dev` intentionally runs an **unauthenticated disposable demo sandbox with API key entry and live mode disabled**. Use `npm start`, not the development command, for actual operation. Never expose the sandbox as a production server.

| Path | Role |
| --- | --- |
| `public/` | Dashboard, chat interface, stylesheet, widget loader and fictional product image |
| `worker/index.js` | Auth, protected settings/knowledge APIs, encrypted keys, provider calls, limits and exports |
| `worker/core.js` | Language metadata, sample translations, validation and source retrieval |
| `server.mjs` | Standalone Node HTTP server and SQLite adapter |
| `drizzle/` | Schema-only SQL migration |
| `scripts/` | Worker build and artifact validation |
| `tests/` | Language, security, settings, chat and storage checks |
| `docs/` | Setup, installation and evaluation guides |

## Scope and roadmap

**Single business and authenticated workspace operator per deployment.** The architecture separates public widget settings from private administration, but this is not a multi-tenant SaaS or a production security certification.

Future work: semantic multilingual retrieval, human-reviewed multilingual evaluations, scheduled sync, a WordPress plugin, streaming replies, identity-verified order tracking, live product connectors and multi-business billing. None is represented as working in v1.

## Credits

Created for Shree Krishna R's open-source portfolio. Code and design developed with AI assistance. The clothing-store image was AI-generated for this fictional demo; no real stock, prices or brand affiliation is implied.

MIT licence: [LICENSE](LICENSE). Contribution guidance: [CONTRIBUTING.md](CONTRIBUTING.md). Security and data handling: [SECURITY.md](SECURITY.md).

## Conversation data and handover limits

Customer transcripts now persist for support. They are limited to 150 messages per thread; only the latest 200 conversations are listed. Records older than 30 days are purged when the operator lists conversations. Inactive installations need a separate retention job if deletion must occur on a precise schedule. Public customers access only their own thread using a random bearer token in browser-tab session storage. Internal notes remain private. The queue uses five-second polling, not WebSockets. AI is paused after human handover until the operator explicitly returns the thread to AI. Unavailable providers also route to the queue. No independent agent accounts, notifications, email integration or large-team concurrency guarantees are included.
