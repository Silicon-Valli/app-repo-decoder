# Repo Decoder

Paste any public GitHub URL and get a plain-English breakdown: what it does, who it's for, key stats, and a one-line pitch you can copy and share.

No signup. No API key. No install. Works entirely in the browser.

## What it does

- Parses any GitHub URL to extract owner/repo
- Fetches repo metadata (stars, forks, language, topics) via GitHub's public API
- Downloads and cleans the README to extract a plain-English excerpt
- Infers the target audience from the description and topics
- Generates a copyable one-sentence pitch

## How to run locally

Just open `index.html` in a browser. That's it. No server needed.

```bash
open index.html
# or
python3 -m http.server 8000
```

## How to deploy

Drag the `index.html` file (or this folder) to Vercel, Netlify, or any static host. Or:

```bash
npx vercel
```

## Environment variables

None. This app uses only GitHub's public API â no API key or authentication required.

## Notes

- GitHub's public API allows 60 requests/hour per IP. Rate limit errors are shown inline.
- The README parser strips code blocks, badges, and short lines to surface meaningful content.
- Audience inference uses regex patterns on description + topics. Falls back to primary language.

## V2 ideas

- Trending repos tab (weekly top repos with plain-English breakdowns)
- GitHub OAuth for 5,000 req/hour limit
- Email capture gate on "Copy pitch" (3 free uses per session)
- Multiple pitch formats: tweet / LinkedIn / Slack
