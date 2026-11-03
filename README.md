# AgentKey Docs

Documentation for [AgentKey](https://agentkey.app) — one key that connects your
AI agent to web search, scraping, social media, e-commerce, crypto/on-chain,
finance, business, jobs, travel, weather, academic and developer data. Built with
[Mintlify](https://mintlify.com).

## Develop locally

Install the Mintlify CLI and run the dev server from the repo root (where
`docs.json` lives):

```bash
npm i -g mint
mint dev
```

The site is served at `http://localhost:3000`.

## Validate

```bash
mint validate        # strict build check
mint broken-links    # internal links
mint openapi-check api-reference/openapi/v2.yaml
```

## Structure

```
docs.json                       # navigation, theme, and site config
index.mdx                       # introduction
quickstart.mdx                  # 0 → first tool call
authentication.mdx              # master key usage (MCP and REST)
connect/                        # installation (one-line / npx / prompt / desktop), supported agents
concepts/                       # unified key, two ways to call, discover→describe→execute, billing, failover
capabilities/                   # the categories: search, scrape, social, ecommerce, crypto, finance,
                                #   business, jobs, travel, weather, academic, developer
api-reference/
  introduction.mdx              # the two surfaces
  scenario/*.mdx                # Scenario Methods guides: overview, quickstart, requests, responses,
                                #   routing-and-pricing, errors
  scenario/methods/*.mdx        # GENERATED — one stub per public method, rendered from the OpenAPI file
  openapi/v2.yaml               # GENERATED — copy of the server's OpenAPI snapshot
  find-tools.mdx, describe-tool.mdx, execute-tool.mdx, account.mdx, errors.mdx   # MCP meta tools
scripts/
  sync-openapi.py               # pulls the OpenAPI snapshot from the server repo and regenerates the above
  scenario-excluded.txt         # methods that exist in the snapshot but have no routable provider yet
faq.mdx, support.mdx            # resources
logo/, favicon.svg              # placeholder brand assets — replace with official ones
```

## Syncing the Scenario Methods reference

The method pages under `api-reference/scenario/methods/` and the `Methods` /
`Catalog & Describe` groups in `docs.json` are generated from the server's
OpenAPI snapshot (`docs/openapi/v2.yaml` in the AgentKey-Server repo). Whenever
that snapshot changes:

```bash
python3 scripts/sync-openapi.py --server-repo /path/to/AgentKey-Server
```

The script copies the spec, writes one MDX stub per method (frontmatter
`openapi:` points at the operation; Mintlify renders the page), removes stubs
for methods that disappeared, and rewrites the generated navigation groups. Hand
edits to generated stubs are overwritten.

Methods listed in `scripts/scenario-excluded.txt` are skipped: they are in the
snapshot but every provider binding is still a draft, so calling them returns
`502`. Refresh that list from the server repo with
`go run ./tools/gen-scenario methods` (exclude methods whose bindings are all
`(draft)`). The script also prints a "verify" list of methods whose providers all
have at least one draft binding — check those against the server before
publishing.

Requires Python 3 and PyYAML.

## Notes

- Install methods are real: the default one-line installer
  (`https://agentkey.app/install.sh` / `install.ps1`), the manual two-step
  (`npx skills add chainbase-labs/agentkey && npx -y @agentkey/cli --auth-login`,
  per the skill repo README) and the ClawHub skill `@chainbase/agentkey`.
  Source of truth for install and setup: https://github.com/chainbase-labs/Agentkey.
- Prices and plan credits are never written into the docs; pages link to
  https://agentkey.app/pricing.html instead.
- The Scenario Methods (`/v2/ops`) reference documents the server as of the
  snapshot date in `scripts/scenario-excluded.txt`. Publish it once `/v2` is
  live on `api.agentkey.app`.
- Brand assets in `logo/` and `favicon.svg` are placeholders.

## Deploy

Connect the repo in the [Mintlify dashboard](https://dashboard.mintlify.com);
changes pushed to the default branch deploy automatically.
