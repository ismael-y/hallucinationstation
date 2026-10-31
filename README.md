# Hallucination Station

A multiplayer bluffing trivia game built with AI assistance in ChatGPT Work mode.

**Play:** https://hallucinationstation.xyz

## How to play

2–8 players join from their own devices with a room code. No account is required.
Across five rounds, players write believable fake answers and pick the truth from
anonymized choices. Find the truth for 2 points; fool another player for 1 point.
Each player has one optional Call the Bluff token per game: identify a different
answer as fake for +1, or lose 1 if it is the truth. Tied leaders share the win.

## Features

- Server-managed rooms, timers, voting, and scoring
- 150 randomized questions and persistent question history per host browser
- Synchronized 3, 2, 1, “Trust no one” opening
- Original synthesized music with individual device controls and increasing urgency
- Server-side profanity filtering for nicknames and answers
- Responsive interface for phones and computers

## Technology

React, TypeScript, Vinext/Vite, Tailwind CSS, Cloudflare Workers, Cloudflare D1,
and Drizzle migrations. This is a full-stack application; it needs a Worker
runtime and database. GitHub Pages alone cannot run its multiplayer server.

## Local setup

Install Node.js 22.13 or later and the pnpm version specified in `package.json`.
From the extracted project folder:

```sh
pnpm install --frozen-lockfile
pnpm build
```

Initialize the local database once by applying the two migrations in order:

```sh
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_organic_roland_deschain.sql
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0001_confused_silhouette.sql
pnpm start
```

Open the local URL printed by Wrangler. For development with hot reloading, use
`pnpm dev` after database setup. Local database state stays in `.wrangler/`.
These instructions describe the existing build workflow; dependency availability
and a complete installation on a fresh computer have not been re-tested for this export.

## Source layout

- `app/page.tsx` — homepage, lobby, voting, reveals, and scoreboard
- `app/api/game/route.ts` — room API and validation
- `lib/game.ts` — phase transitions, scoring, question selection, and player views
- `lib/moderation.ts` — nickname and answer filtering
- `components/game/music.tsx` — music synthesis and device controls
- `db/` and `drizzle/` — database schema and migrations
- `public/` — artwork and static assets
- `build/` — required build integration source (not generated output)

## Hosting and GitHub

The existing deployment is hosted through ChatGPT Sites. `.openai/hosting.json`
identifies that Site and its logical database binding; it contains no secret.
Uploading these files to GitHub does not automatically update the live game.
Do not commit `.env` files, credentials, `node_modules`, `dist`, or local databases.
The included `.gitignore` excludes these local files.

To upload using GitHub's website, extract this ZIP first and upload its contents,
preserving the folders. Do not upload only the ZIP. For larger folder uploads,
GitHub Desktop is convenient; include the `.openai` folder and `.gitignore`.

## Credits

Questions include adaptations from Open Trivia DB: https://opentdb.com/,
licensed under CC BY-SA 4.0: https://creativecommons.org/licenses/by-sa/4.0/.
Preserve source links and attribution when reusing question content. Third-party
component and build integration licenses remain alongside their respective files.

Exported from source commit: `30e41c54585d588dec34e44704abad0f066a3154`.
