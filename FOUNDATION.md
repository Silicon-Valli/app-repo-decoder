# FOUNDATION â Repo Decoder

## The Problem

GitHub is built for developers. But non-technical founders, PMs, investors, and indie builders land on repos constantly â from trending lists, Slack links, newsletter shoutouts â and they can't tell what they're looking at. Reading a full README takes 10 minutes and often still doesn't answer the basic question: *what does this actually do?* That friction compounds. People skip the evaluation entirely or ask a developer, which is a waste of everyone's time.

## What Exists

- **GitHub's own UI** â raw README, no synthesis, no audience cues. Designed for contributors, not evaluators.
- **readme.so / readme.ai** â tools for *writing* READMEs, not reading them.
- **ChatGPT/Claude** â you can paste a README and ask, but there's no URL-first workflow. It's manual and slow.
- **StackShare / LibHunt** â tech stack discovery, not repo explanation for non-technical readers.

Nobody has built the "translator" layer that sits on top of GitHub for non-developers.

## Our Angle

Paste a GitHub URL, get plain English back in 5 seconds. No login, no API key, no install. Four outputs: repo stats, what it does (README excerpt), who it's for (inferred audience), and a copyable pitch sentence. Works entirely in the browser via GitHub's public API.

## In Scope for v1

- URL input â parse owner/repo
- Two GitHub API calls (repo metadata + README)
- README parsing to extract meaningful plain-text excerpt
- Audience inference from description + topics
- Four output cards: stats, what it does, who it's for, copyable pitch
- 4â5 example repos preloaded
- Graceful error states (404, rate limit, network failure)
- Mobile responsive

## Out of Scope

- Authentication / GitHub OAuth
- Trending repos tab
- Email capture gate
- Paid tier / Repo Radar digest
- Multiple pitch format variants (tweet/LinkedIn/Slack)
- Comparing multiple repos side by side
- Saving/history of decoded repos

## Success Signal

Someone pastes a repo URL, reads the output, and immediately copies the pitch to share with their team or in a Slack message. Secondary: people share the tool itself by linking to it.

## Monetization Direction

Free launch with email capture gate on the "Copy pitch" button after 3 uses per session (localStorage counter). Build list of non-technical founders who regularly evaluate repos. Medium-term: $9/month Repo Radar digest â weekly plain-English breakdown of the 10 most-starred repos. B2B later: repo audit for procurement/security teams at $49/month per team.
