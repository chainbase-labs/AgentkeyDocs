# AgentKey Docs

Documentation for [AgentKey](https://agentkey.app) — one key that connects your
AI agent to web search, scraping, social media, crypto/on-chain, finance,
e-commerce, and business data. Built with [Mintlify](https://mintlify.com).

## Develop locally

Install the Mintlify CLI and run the dev server from the repo root (where
`docs.json` lives):

```bash
npm i -g mint
mint dev
```

The site is served at `http://localhost:3000`.

## Validate

Check for broken links and config issues:

```bash
mint broken-links
```

## Structure

```
docs.json                 # navigation, theme, and site config
index.mdx                 # introduction
quickstart.mdx            # 0 → first tool call
authentication.mdx        # master key usage
connect/                  # installation (one-line / prompt / desktop), 22 supported agents
concepts/                 # unified key, discover→describe→execute, billing, failover
capabilities/             # search, scrape, social, crypto, finance, business, ecommerce
api-reference/            # find_tools, describe_tool, execute_tool, account
faq.mdx, support.mdx      # resources
logo/, favicon.svg        # placeholder brand assets — replace with official ones
```

## Notes

- Install methods are real: a one-line installer
  (`https://agentkey.app/install.sh` / `install.ps1`) and the ClawHub skill
  `chainbase/agentkey`. Pages marked with a `<Note>` still contain placeholders
  (exact key-provisioning steps, pricing). Confirm these during install or in the
  [AgentKey console](https://console.agentkey.app) and replace before publishing.
- Brand assets in `logo/` and `favicon.svg` are placeholders.

## Deploy

Connect the repo in the [Mintlify dashboard](https://dashboard.mintlify.com);
changes pushed to the default branch deploy automatically.
