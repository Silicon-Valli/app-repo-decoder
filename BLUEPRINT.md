# BLUEPRINT â Repo Decoder

## What this is

Repo Decoder takes any public GitHub URL and returns a plain-English breakdown in seconds: what the project does, who it's for, key stats (stars, forks, language, last updated), a cleaned README excerpt, and a one-sentence pitch you can copy and share. No signup, no API key, no install. Works entirely in the browser.

Built for non-technical founders, PMs, investors, and indie builders who land on GitHub repos from Slack messages, newsletters, and trending lists â and can't quickly figure out what they're looking at.

## Why it was built

GitHub is built for developers. But non-developers encounter repos constantly and have no good way to evaluate them quickly. Reading a full README takes 10 minutes and often still doesn't answer the basic question: *what does this actually do?*

The vibe coding wave made GitHub more visible to non-technical builders. Repo Decoder is the translator layer that was missing.

The demand pattern: founders ask "what is this library?" PMs paste repo links in Slack with zero context. Investors see repos in pitch decks. Nobody had a URL-first tool that just answers the question plainly.

## Stack

- **Single HTML file** â no build step, no npm, no backend. Deploys by drag-and-drop.
- **Tailwind CSS via CDN** â utility styling without a pipeline
- **Inter font via Google Fonts** â clean, readable
- **GitHub Public API** â two endpoints called directly from the browser, no auth required

## Architecture decisions

**Why a single HTML file?** The product value is zero friction. A single file is honest about that â it deploys anywhere and has no surface area for things to break. Vercel, Netlify, GitHub Pages, or literally a USB drive.

**No auth for v1.** GitHub's public API gives 60 requests/hour per IP. That's enough for an MVP. Rate limit errors surface inline with a clear message. OAuth for 5,000 req/hour is a v2 item.

**Promise.allSettled for parallel API calls.** Both the repo metadata and README fetch happen in parallel. If the README fails (private repo, no README), the app still renders with just the description â it degrades gracefully rather than blocking.

**README cleaning heuristic.** The strategy: strip code blocks, badge lines, HTML tags, Markdown headings, and lines under 30 characters. Take the first 2â3 clean paragraphs, cap at 500 chars. Aggressive but effective â it surfaces the narrative text rather than the scaffolding.

**Audience inference via regex.** Patterns run against `description + topics` to classify target users (e.g. `/\bai\b|\bllm\b/` â "AI builders and product teams"). Up to 3 labels returned. Falls back to the repo's primary language if nothing matches. Simple, fast, good enough for v1.

**Dark theme with indigo accent.** The design targets SaaS-level polish â this is eventually a trending discovery platform, not a weekend hack. The v1 should look like the foundation of that.

## How to replicate

1. Create a single `index.html` file
2. Add Tailwind via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
3. Add Inter via Google Fonts in the `<head>`
4. Parse GitHub URLs with regex: `github\.com\/([^\/\s?#]+)\/([^\/\s?#]+)` â strip `.git` suffix
5. Call two GitHub API endpoints in parallel using `Promise.allSettled`:
   - `GET https://api.github.com/repos/{owner}/{repo}` for metadata
   - `GET https://api.github.com/repos/{owner}/{repo}/readme` for the README (base64-encoded)
6. Decode the README with `atob()`, then clean it (strip code blocks between ``` markers, badge lines containing `![`, lines under 30 chars, Markdown headings)
7. Run regex patterns against description + topics for audience inference
8. Generate a pitch string from the repo data
9. Handle error states: 404, 403 (rate limit), network failure â all inline, never crash the page
10. Deploy to Vercel by dragging the file in, or `npx vercel` from the folder

No environment variables needed. No backend. No build step.

## What I'd do differently

**Smarter README parsing.** The current approach strips aggressively and takes the first clean text. A better version would find the first `##` section with a meaningful name ("What is this?", "Overview", "About") and extract that specifically.

**Trending repos tab.** Pre-populating with live trending repos would make the tool useful even without a URL to paste. The GitHub trending API via `gh-trending.deno.dev/repositories` is unofficial but stable.

**Multiple pitch formats.** One pitch sentence is useful. Three formats would be more useful: one-liner for Twitter, paragraph for LinkedIn, bullets for a Slack message. Same data, three outputs.

**Email gate on Copy pitch.** A localStorage counter (3 free uses per session, then email capture) is the cleanest early monetization for a free tool. Build the list before building a paid tier.

**GitHub OAuth.** Adds 10 minutes of auth flow but unlocks 5,000 req/hour â meaningful if the tool sees real traffic.

## V2 vision

A listing place for trending GitHub repos with the same plain-English breakdown layer applied automatically. Weekly digest of the top 10 repos. SaaS-like design that functions as a discovery platform for non-technical builders â not a developer tool.
