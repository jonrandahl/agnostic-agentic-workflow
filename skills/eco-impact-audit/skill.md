---
name: eco-impact-audit
description: >
  Audit a website or URL for ecological and environmental impact by querying
  multiple eco-audit tools and collating the results. Use whenever the user
  asks to measure, test, or audit the carbon footprint, eco-design score,
  environmental impact, or sustainability of a website or URL. Triggers on
  phrases like "eco audit", "carbon audit", "sustainability check", "check
  the green credentials of", "run an eco test on", "how green is", or any
  request to measure the environmental performance of a site.
---

# Eco Impact Audit

Audit a public URL against a suite of ecological performance tools and
present a collated summary of the results.

---

## Overview

Most tools on the eco-audit landscape are UI-only. A small number expose
public APIs or URL-parameterised endpoints. The strategy is:

1. Call the tools that have programmable APIs.
2. Generate direct deep-links for the UI-only tools so the user can
   run them in one click.
3. Collate and present the API results alongside those links.

---

## Tools with programmable APIs

### 1. Yellow Lab Tools

REST API. Asynchronous: POST to start a run, poll until complete.

```
POST https://yellowlab.tools/api/runs
Content-Type: application/json
{ "url": "<target>", "waitForResponse": false }

→ { "runId": "abc123" }

GET https://yellowlab.tools/api/results/<runId>
→ 404 while pending; full JSON when complete
```

Key fields to surface:
- `scoreProfiles.globalScore` – overall score out of 100
- `scoreProfiles.scores` – per-category scores (network, rendering, etc.)
- Rate limit: 12 runs per 24 h from a single IP.

### 2. EcoIndex (via RapidAPI)

Requires a RapidAPI key (`ECOINDEX_API_KEY`). Runs are asynchronous;
check status until `status === "DONE"`.

```
POST https://ecoindex.p.rapidapi.com/v1/tasks/ecoindexes/
Headers:
  X-RapidAPI-Key: <key>
  X-RapidAPI-Host: ecoindex.p.rapidapi.com
Body: { "url": "<target>", "width": 1280, "height": 800 }

→ { "id": "<taskId>" }

GET https://ecoindex.p.rapidapi.com/v1/tasks/ecoindexes/<taskId>
→ poll until result.status === "DONE"
```

Key fields:
- `result.score` – 0–100 (higher is better)
- `result.grade` – A–G
- `result.ges` – greenhouse gas emissions (gCO₂e per page view)
- `result.water` – water consumption (cl per page view)
- `result.nb_requests`, `result.page_size_total`, `result.dom_size`

### 3. Website Carbon (`/data` endpoint)

The `/site` endpoint was removed in July 2025. The `/data` endpoint still
works but requires you to supply `bytes` and `green` (hosting status)
rather than a URL. Use this as a supplementary calculation once page size
is known from another tool.

```
GET https://api.websitecarbon.com/data?bytes=<n>&green=<0|1>
→ { "gco2e": <grams>, "rating": "<A-F>", "cleanerThan": <0-1> }
```

To get `bytes`: parse `page_size_total` from the EcoIndex result, or
`requests.totalTransferSize` from the Yellow Lab Tools result.

---

## UI-only tools (open in browser)

Generate a direct deep-link where the tool supports URL parameters, or
a link to the tool's homepage with instructions otherwise.

| Tool | Deep-link pattern |
|---|---|
| EcoGrader | `https://ecograder.com/?url=<encoded>` |
| EcoDesign | `https://ecodesign.dev/?url=<encoded>` |
| EcoIndex (web UI) | `https://www.ecoindex.fr/en/?url=<encoded>` |
| Yellow Lab Tools (web UI) | `https://yellowlab.tools/?url=<encoded>` |
| Kastor.green | `https://kastor.green/` – paste URL manually |
| GreenFrame | `https://greenframe.io/` – requires account |
| GreenSpector | `https://greenspector.com/` – requires account |
| MilleCheck | `https://millecheck.ai/` – paste URL manually |
| WEBKARBON | `https://www.webkarbon.bzh/` – paste URL manually |
| Greenoco | `https://greenoco.io/` – paste URL manually |
| Tuil | `https://tuil.vertuance.fr/` – paste URL manually |
| PAGIEL | `https://pagiel.com/` – paste URL manually |
| Scaphandre | CLI tool – see https://github.com/hubblo-org/scaphandre |
| Boavizta | `https://boavizta.org/en/tools` – multiple tools |
| ArneoGroup | Contact for access |
| appYranking | Contact for access |

---

## Workflow

```
1. Accept the target URL from the user.
2. Validate it starts with http:// or https://.
3. Kick off all available API calls in parallel:
   a. Yellow Lab Tools run
   b. EcoIndex task (if API key present)
4. Poll until all async results are ready (max ~60 s; back off 3 s
   between polls).
5. Once Yellow Lab Tools returns, extract page bytes and call
   Website Carbon /data.
6. Collate results into a summary table:
   - Tool | Score | Grade/Rating | CO₂ | Notes
7. Below the table, render a "Run these manually" section with all
   UI-only links pre-populated where possible.
8. Highlight the most actionable findings (DOM size, request count,
   transfer weight, hosting status).
```

---

## Presenting results

Lead with a single-row summary per tool. Example structure:

```
## Eco audit — example.com

| Tool              | Score      | CO₂ per view | Grade |
|---|---|---|---|
| EcoIndex          | 72 / 100   | 0.9 gCO₂e    | B     |
| Yellow Lab Tools  | 65 / 100   | –            | –     |
| Website Carbon    | –          | 1.2 gCO₂e    | C     |

### What this means
- Page weight: 340 KB across 28 requests
- DOM size: 1,842 nodes
- Green hosting: No

### Run these manually for additional data
- [EcoGrader](https://ecograder.com/?url=https%3A%2F%2Fexample.com)
- [EcoDesign](https://ecodesign.dev/?url=https%3A%2F%2Fexample.com)
- …
```

---

## Building this as an interactive artifact

When the user wants an in-browser tool rather than a one-off audit,
build a React artifact that:

- Accepts a URL via an `<input>` element
- Calls Yellow Lab Tools and optionally EcoIndex via `fetch()`
- Polls for async results and shows a progress indicator
- Renders the collated results table
- Generates all UI-only deep-links automatically

Note: the EcoIndex RapidAPI endpoint will require a key; surface a
visible input for the user to provide it rather than hard-coding.

---

## Key limitations

- Yellow Lab Tools enforces 12 runs / 24 h per IP from the public API.
- EcoIndex API requires a RapidAPI account.
- Website Carbon no longer accepts a URL directly (as of July 2025);
  byte-count must come from another tool.
- All other tools require manual use via their web UIs.
- Server-rendered or SPA pages may produce different results depending on
  whether the tool uses a headless browser or a static fetch.
