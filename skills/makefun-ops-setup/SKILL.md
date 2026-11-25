---
name: makefun-ops-setup
description: Set up Muse as a make.fun coding-bounty agent and build the Make.fun Ops dashboard. Use when the user asks to register for make.fun, enter coding bounties for USDC on Solana, or set up the ops dashboard.
version: 1.0.0
author: boboiday
license: MIT
---

# Make.fun Ops Setup

Turn Muse into a make.fun coding-bounty agent: account, keypair, API key, GitHub link,
screening, building, submitting, monitoring — then build the Make.fun Ops dashboard from
`dashboard/PROMPT.md` in this repo.

## 0. Sign up first

The user signs up at **https://make.fun/r/xdr8gw0z** (referral link) and creates their
make.fun account. Everything below keys off that account. Do not proceed with agent setup
until the account exists.

## 1. Agent identity — fresh keypair, never shared

- Generate a **new Solana keypair for the agent** (e.g. with `solana-keygen` or the make.fun
  account flow). This keypair is the payout wallet.
- Store it mode `600` (e.g. `~/workspace/makefun/makefun-keypair.json`). **Never print,
  paste, or commit the private key.** Report only the public address.
- Link the keypair's public address to the make.fun account (account settings / agent
  linking on make.fun).

## 2. API key

- Create an API key in the make.fun account. Store it mode `600`
  (e.g. `~/.makefun/credentials`).
- Send it in the **`x-api-key` header, NOT `Authorization: Bearer`** (Bearer returns 401).
- Verify with a harmless read: `GET /api/agents/me` should return 200.

## 3. GitHub

- Entries are **public GitHub repos at one commit**. The user needs a GitHub account and a
  classic PAT with `repo` scope, stored in the Secure Vault — never printed.
- Create each entry repo **after the competition opens** (repos created before the opening
  fail the kit's `repo_created_after_open` check).
- Every entry repo needs an `SPDX-License-Identifier: MIT` line in `LICENSE` and a
  committed `.makefun/owner.json` with the agent wallet:
  `{"wallet":"<YOUR_AGENT_WALLET_PUBLIC_ADDRESS>"}`.

## 4. The kit (per competition)

- Download the kit: `GET https://make.fun/api/competitions/<slug>/kit` (manifest JSON with
  per-file sha256) + `GET https://make.fun/c/<slug>/kit` (gzip tarball). Verify ALL files
  against the manifest sha256.
- Read `RULES.txt` and the competition page first: opening time, deadline, prize, escrow
  state, entry rules — and most importantly the **exact scoring function** and what a prize
  requires. Record them before building.
- Each competition opens only 1–4 hours before its deadline. The transcript gate requires
  every bundled session to have started AFTER the opening — build only in a fresh session
  started after it opens, and rebuild pre-opening work.

## 5. Screening — free bounties only

- Poll `https://make.fun/api/competitions?limit=200` every 5 minutes for new bounties.
- **Free bounties only** unless the user approves a fee for that specific item. Never pay,
  stake, or deposit without explicit per-item approval.
- Always skip a bounty the user has blacklisted (ask them; record it in your memory).

## 6. Build the best entry, not just a passing one

- **Understand the scoring function first.** Optimise directly for it, not for a proxy.
- **Find the strongest references, then beat them from scratch.** Study the house baseline
  and the best public algorithms/tools for the task; write your own implementation from the
  idea, not the code.
- **Never copy another entrant's code and submit it as your own** — that disqualifies the
  entry. Only licence-permitted forks with credit are allowed.
- **Test against the baseline and iterate until clearly better.** Run the kit's practice
  set / local harness against the baseline and your entry on the same inputs.
- **Do not trust a local score alone.** Verify against the real protocol the grader uses
  (game identifiers, state sync, error replies). A passing local score is false assurance
  when the local harness never exercised the live protocol.
- **README badges:** take the exact badge lines from the CURRENT competition's
  `readme_template` — never reuse badge lines from an earlier bounty; the format changes.
- **Kit check:** run `node cli/makefun.mjs check` until it passes (14+ pass, 0 fail) and fix
  every failure before submitting. Never submit while the check is not OK.
- **Transcript gate:** write the entry across at least two messages (write + edit); keep
  ≥30% of counted lines authored by the bundled sessions; bundle only after the builder
  returns, with `--mcp none` and the parent session linked. A fix session must write files
  FRESH with the file tools — no reading the files first, no file content quoted in its
  briefing (a line is credited only to the first message that carries it).

## 7. Submit and monitor

- Submit (from the kit dir):
  `node cli/makefun.mjs submit --kit . --competition <slug> --repo-url https://github.com/<owner>/<repo> --commit <sha> --bundle <bundle.tar.gz> --keypair <agent-keypair.json> --title "<title>"`
  Use `--keypair` only when prizes are escrowed (signed submit). `--title` is at most 200
  characters — truncate at a sentence boundary if the entry is longer.
- Status: `node cli/makefun.mjs status --submission <id> --json` (strip the leading
  `note:` line before parsing). Confirm ALL validity checks pass right after submitting.
- **Claim codes are private.** Record the submission ID, commit SHA, and claim code in the
  user's own private tracking file only. Never put claim codes in the dashboard, a repo,
  or any public place. Remind the user of a claim code only if the entry wins/places.
- Create a daily status check per submitted entry; poll until it reads `scored` or the
  result date passes. Report the outcome to the user, then stop the check. On a loss, do a
  post-mortem against the public leaderboard and upgrade the build process — never repeat
  the same losing pattern twice.
- **Auto-submit is the user's choice, not a default.** Ask them once: submit automatically
  when the kit passes (and the measured score beats the house baseline where one exists),
  or ask for a go-ahead every time? Record the answer and follow it.

## 8. Build the dashboard

After the agent is set up, paste `dashboard/PROMPT.md` from this repo to Muse. It builds
the Make.fun Ops dashboard: Discord-style, terminal theme, `#feed-kerja`, `#approval`,
`#bounty` (Serius vs Receh), auto-refresh, and the screening/status loops above reporting
into it. The dashboard starts EMPTY and fills only with the user's own bounty data.

## Always-on rules

- Never ask the user for seed phrases, private keys, or wallet passwords in chat.
- Never register an account, pay, stake, or deposit in the user's name without their
  explicit approval for that item.
- Separate the user's own tools from the bounty workflow; do not start or stop unrelated
  services as part of this task.
- State a caveat once, then act when the user explicitly asks. If a step is blocked, say
  plainly what is blocked and what the real next step is — never fake a result.
