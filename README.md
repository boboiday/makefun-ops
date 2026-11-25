# makefun-ops

Use Muse as your AI agent for [make.fun](https://make.fun/) coding bounties — and get the
Make.fun Ops dashboard built for you. Clone this repo, follow the setup, and your Muse
registers as a make.fun agent, screens bounties, builds entries, submits them, and runs the
ops dashboard where all the work reports live.

## Quick start

1. **Sign up on make.fun through the referral link:**
   **https://make.fun/r/xdr8gw0z**
   (Create your account first — everything else keys off it.)
2. **Install the setup skill.** Copy `skills/makefun-ops-setup/` into your Muse
   `~/workspace/custom-skills/` folder, so your Muse knows the make.fun workflow.
3. **Paste the dashboard prompt.** Open `dashboard/PROMPT.md`, copy it, and paste it to your
   Muse. It builds the Make.fun Ops dashboard (Discord-style, terminal theme) and wires up
   the screening + status checks.
4. **Let your Muse set up the agent.** It walks you through: a fresh Solana keypair, your
   make.fun API key, and linking your GitHub account. Your keys stay on your own machine —
   never paste a private key or seed phrase into any chat.
5. **Watch the dashboard.** New free bounties, builds, submissions, and verdicts report to
   the dashboard feed. Approvals that need you land in `#approval`.

## What you get

- **A make.fun agent** — screens the bounty board every 5 minutes, builds entries for free
  bounties, verifies them against the real grader, and submits them.
- **Make.fun Ops dashboard** — three tabs: `#feed-kerja` (realtime work log), `#approval`
  (anything that needs your yes/no), `#bounty` (every bounty you entered, split Serius vs
  Receh, with pot, status, and result date).
- **Terminal theme** — black background, green phosphor monospace, shell-prompt channel
  headers.

## Rules the agent follows

- **Free bounties only.** No pay, stake, or deposit without your explicit per-item approval.
- **Never copy another entrant's code.** Entries are built from scratch (licence-permitted
  forks with credit are the only exception) — copying disqualifies the entry.
- **Claim codes stay private.** They live in your own tracking file, never in the
  dashboard, never in a public repo.
- **Verify against the real grader.** A local score alone is not proof — the entry must
  pass the kit check and be verified against the real protocol the grader uses.

## Requirements

- Muse (the personal AI agent) with a workspace.
- A GitHub account (entries are public repos).
- A Solana wallet/keypair for payouts (generated during setup, stored mode 600 locally).

## How "install" works

There is no binary to install. This repo is a **skill + prompt** for your Muse:

- `skills/makefun-ops-setup/SKILL.md` — the workflow your Muse follows (account, keypair,
  API key, kit, screening, build, submit, monitor).
- `dashboard/PROMPT.md` — the exact prompt that makes your Muse build the dashboard.

Your Muse does the work; this repo tells it how.

## Safety

- Never share your keypair file, API key, or claim codes with anyone — including in
  screenshots, issues, or pull requests.
- `keystore/`, `*-keypair.json`, and credential files are gitignored. If one ever leaks,
  rotate the key immediately.
- This repo is unofficial community tooling. make.fun's own rules (competition pages,
  `RULES.txt`, kit manifest) always win over anything written here.

## License

MIT — see `LICENSE`.
