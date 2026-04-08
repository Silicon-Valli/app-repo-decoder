# ARCHITECTURE â Repo Decoder

## Stack

Single HTML file. No build step. No npm. No backend. Deploys to Vercel by drag-and-drop or via CLI.

- **HTML/CSS/JS** â everything in one file
- **Tailwind CSS** via CDN â utility styling without a build pipeline
- **Inter** via Google Fonts â clean, readable typography
- **GitHub Public API** â two endpoints, called directly from the browser

## Why Single HTML File

The product value is zero-friction. No signup, no install, no API key. A single HTML file is honest about that â it deploys anywhere (Vercel, Netlify, GitHub Pages, literally a USB drive) and has no surface area for things to break.

## API Calls

Two parallel calls via `Promise.allSettled`:
1. `GET https://api.github.com/repos/{owner}/{repo}` â stats, description, topics
2. `GET https://api.github.com/repos/{owner}/{repo}/readme` â base64-encoded README

Rate limit: 60 requests/hour per IP. Shown inline if hit.

## Pages / Screens

One page. States:
- **Empty** â hero + input + example repo chips
- **Loading** â spinner, input locked
- **Results** â 4 cards below the input
- **Error** â inline message, input re-enabled

## Output Cards

1. **Header** â repo name, description, stars/forks/watchers, language, last updated, topics
2. **What it does** â cleaned README excerpt (â¤500 chars)
3. **Who it's for** â up to 3 inferred audience labels from description + topics
4. **The pitch** â one auto-generated sentence, copyable

## Key Decisions

- No auth for v1. Rate limiting is a UX note, not a blocker.
- README cleaning: strip code blocks, badges, heading markers, short lines. First 2â3 clean paragraphs.
- Audience inference via regex on description + topics. Falls back to primary language if nothing matches.
- Design: SaaS-level polish. This will eventually be a trending discovery platform â the v1 should look like the foundation of that, not a weekend hack.

## V2 Notes (logged, not built)

- Trending tab: `gh-trending.deno.dev/repositories`
- Email gate on Copy pitch (localStorage counter, 3 free uses)
- GitHub OAuth for 5,000 req/hour rate limit
- Multiple pitch formats: tweet / LinkedIn / Slack
