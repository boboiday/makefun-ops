# Make.fun Ops Dashboard — build prompt

Copy this whole prompt and paste it to your Muse (after finishing the agent setup in
`skills/makefun-ops-setup/SKILL.md`). Your Muse builds the dashboard in your own Muse.

---

Build me a **Make.fun Ops dashboard** as a Space in my Muse. It is the operations centre
for my make.fun coding-bounty work as an AI agent.

## Look and feel — terminal theme

- Black background, green phosphor monospace text throughout.
- Channel headers styled like shell prompts (`~/feed-kerja`, `~/approval`, `~/bounty`) with
  a blinking block cursor.
- Subtle CRT scanline effect. Green accent buttons with black text. Status badges in the
  terminal palette.
- Discord-style layout: a left channel list, one channel open at a time.

## Tabs

1. **`#feed-kerja`** — realtime work log. Every step my agent takes reports here as a feed
   post: bounty found, build started, kit check passed/failed, submitted (with submission
   ID), verdict arrived, result date. Newest first. The agent writes here; I read.
2. **`#approval`** — anything that needs my decision, one item per row, each with an
   **Approve** button and a **Decline** button, plus the exact question and the
   consequences of each choice. Approving an item lets the agent proceed; declining stops
   it. Also send me a short notification ping (WhatsApp) when something new needs approval,
   so I check this tab — but keep work reports out of WhatsApp.
3. **`#bounty`** — the list of EVERY bounty I have entered, all of it, not just the latest
   20. Columns: slug, entry title, prize pot, status (submitted / scored / checked +
   outcome), result date. Two categories: **Serius** (bigger pots, real builds) and
   **Receh** (small one-liner / micro bounties). Show counts: pending results, decided,
   dead. A refresh button re-checks the board.

## Data — mine only, starting empty

- Seed the dashboard EMPTY. It fills only from MY OWN bounty work: my submissions, my
  verdicts, my results.
- Source of truth is my own private tracking file (submission IDs, claim codes, statuses).
  The dashboard reads statuses from it; it never invents rows.
- **Claim codes NEVER enter the dashboard.** They stay private in my tracking file. The
  dashboard shows submission IDs and outcomes only.

## Automation wired into it

- A screening loop polls `https://make.fun/api/competitions?limit=200` every 5 minutes for
  new FREE bounties and posts new ones to `#feed-kerja` (and to `#approval` if my rules say
  that bounty needs my go-ahead).
- A board refresh every 30 minutes re-checks open bounties; a daily check re-checks every
  pending submission's status until it reads `scored` (then it posts the result to the feed
  and stops polling that entry).
- When an entry is submitted, a verdict arrives, or a result lands, write the row and the
  feed post immediately — don't wait for the next refresh.

## Rules

- Free bounties only, unless I approve a fee for that specific item. Never pay, stake, or
  deposit without my explicit per-item approval.
- Never copy another entrant's code. Entries are built from scratch.
- If a build or submit step is blocked, say plainly what is blocked and what the real next
  step is — never report a result you didn't verify.
- When the dashboard is built, show me its card and tell me the counts (rows seeded, feed
  posts, approvals pending). Seeded means empty until my first bounty runs through it.
