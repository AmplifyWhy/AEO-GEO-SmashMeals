# AEO Artifacts — SmashMeals

Agent Experience Optimization (AEO) makes smashmeals.com discoverable and usable by AI assistants — ChatGPT, Claude, Perplexity, Google AI Overview, and others. These files implement the discovery layer.

---

## What goes where

All files live in your site's **public root** (the folder that maps to `https://www.smashmeals.com/`). In most frameworks this is `public/`, `static/`, or the repo root.

```
your-repo/
├── public/
│   ├── llms.txt                        → https://www.smashmeals.com/llms.txt
│   ├── llms-full.txt                   → https://www.smashmeals.com/llms-full.txt
│   ├── llms-ctx.txt                    → https://www.smashmeals.com/llms-ctx.txt
│   ├── robots.txt                      → https://www.smashmeals.com/robots.txt
│   ├── sitemap.xml                     → https://www.smashmeals.com/sitemap.xml
│   └── agent-layer.jsonld              → https://www.smashmeals.com/agent-layer.jsonld
```

**Do not deploy** `agent-layer-gaps.json` or `README.md` — those are internal only.

---

## File-by-file

### `llms.txt`
Short machine-readable summary of the business for AI crawlers. Follows the [llms.txt spec](https://llmstxt.org). Copy as-is.

### `llms-full.txt`
Full context version — includes menu, policies, hours, FAQ. Used by agents that need complete information in one request.

### `llms-ctx.txt`
Compact context file optimized for token-limited models. All three llms files should be deployed; crawlers choose based on their needs.

### `robots.txt`
**Merge with your existing `robots.txt`, do not replace it.** The new lines add explicit allow rules for AI crawlers (ClaudeBot, PerplexityBot, GPTBot, etc.) and point to the sitemap and llms.txt. Preserve any existing rules. The relevant lines to add:

```
# AI agent crawlers — explicitly allowed
User-agent: ClaudeBot
Allow: /

User-agent: anthropic-ai
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: GPTBot
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: CCBot
Allow: /

# Agent discovery
X-LLMs-Txt: https://www.smashmeals.com/llms.txt
Sitemap: https://www.smashmeals.com/sitemap.xml
```

### `sitemap.xml`
Standard XML sitemap. If you already generate one automatically (Lovable, Astro, Next.js, etc.), skip this file and verify your generated sitemap includes all pages. If you deploy this one, make sure `robots.txt` points to it.

### `agent-layer.jsonld`
Combined JSON-LD structured data (Organization + Menu + Products + FAQ + PotentialActions). Two ways to use it:

**Option A — reference file** (simpler): Deploy to public root and add one line to your HTML `<head>`:
```html
<link rel="alternate" type="application/ld+json" href="/agent-layer.jsonld">
```

**Option B — inline** (recommended for SEO): Copy the contents of this file and inject them as an inline script tag in your HTML `<head>`:
```html
<script type="application/ld+json">
  { ...paste contents here... }
</script>
```
Google and Bing parse inline JSON-LD more reliably than linked files.

---

## Future agent integrations (optional, deploy when ready)

These files are placeholders for capabilities that don't require anything from your site today. Deploying them now reserves the right locations and signals to AI platforms that agent integrations are planned. Nothing will break if an AI tries to use them — it will simply find no active endpoint.

When you're ready to connect a live AI agent or booking/ordering integration, these files will be updated with real endpoint URLs.

```
your-repo/
└── public/
    └── .well-known/
        ├── agent-card.json             → https://www.smashmeals.com/.well-known/agent-card.json
        ├── mcp-tools.json              → https://www.smashmeals.com/.well-known/mcp-tools.json
        └── security.txt               → https://www.smashmeals.com/.well-known/security.txt
```

### `.well-known/agent-card.json`
Declares what AI agents are allowed to do with this business. Currently a placeholder. No changes needed to deploy.

### `.well-known/mcp-tools.json`
Lists tools an AI assistant could invoke (e.g., check menu, get hours). Currently a placeholder. No changes needed to deploy.

### `.well-known/security.txt`
Security contact information per [RFC 9116](https://www.rfc-editor.org/rfc/rfc9116). This one is complete and accurate — deploy as-is.

---

## Verification

After deploying, confirm these URLs return the files:

- https://www.smashmeals.com/llms.txt
- https://www.smashmeals.com/robots.txt
- https://www.smashmeals.com/sitemap.xml
- https://www.smashmeals.com/.well-known/agent-card.json

Test structured data with [Google's Rich Results Test](https://search.google.com/test/rich-results) — paste your homepage URL and look for Organization and Menu entries.

---

## Questions

Contact audrey@amplifywhy.ai
