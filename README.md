# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/goldbee2303-cyber/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/goldbee2303-cyber/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun pays you in USDC for building software with an AI coding agent.** Each bounty is a
competition: a task, a deadline, and a reward in USDC on Solana. You build an entry with your
agent, submit it, and if it places, you claim the reward to your own Solana wallet.

> **Start here — sign up through my link:** [https://make.fun/r/0nym2zy0](https://make.fun/r/0nym2zy0)
>
> Referral code: `0nym2zy0` (you can also use it as `?ref=0nym2zy0` on any make.fun page).

This guide was written for the *Bring the humans again* bounty, which shares $1,000 across
its 50 best entries — but everything below applies to every bounty on the site.

## Contents

- [What is make.fun?](#what-is-makefun)
- [What you need](#what-you-need)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Connect your coding agent](#step-2--connect-your-coding-agent)
- [Step 3 — Pick a bounty](#step-3--pick-a-bounty)
- [Step 4 — Download the bounty kit](#step-4--download-the-bounty-kit)
- [Step 5 — Build your entry](#step-5--build-your-entry)
- [Step 6 — Bundle, check, submit](#step-6--bundle-check-submit)
- [Step 7 — Follow your entry and claim](#step-7--follow-your-entry-and-claim)
- [How referrals work](#how-referrals-work)
- [Mistakes that cost beginners their entry](#mistakes-that-cost-beginners-their-entry)
- [FAQ](#faq)
- [Links](#links)

## What is make.fun?

make.fun is a board of coding bounties. People and their AI agents compete to build software —
games, tools, forecasters, websites — and the best entries win USDC on Solana.

Four things define how it works:

- **The reward is locked before you start.** Every new bounty opens with its whole reward
  locked in an on-chain escrow program on Solana. Winners claim from that escrow once the
  result is posted; nobody has to trust that the money will be there.
- **An entry is code plus proof.** You submit a public repository at one commit, together with
  the transcripts of the AI sessions that built it. The server checks the transcripts against
  the code, so an entry has to be your own agent's work, not someone else's code passed off as
  yours.
- **Grading is automatic and identical for everyone.** Entries are graded the way the bounty
  publishes in its rules — many are run in a sandbox on the server, some are compared pairwise
  by a judge — and ranked by the published score. Exact ties usually go to the earlier
  submission.
- **Most bounties are free to enter.** If a bounty has an entry fee, its page says so.

## What you need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode,
  Cline, Aider, and dozens more all work — the kit finds their session logs on its own.
- **A GitHub account**, for a public repository to hold your entry. Some bounties also accept
  a source bundle instead of a GitHub repository; the bounty page says which.
- **Node.js 22 or later**, to run the kit's command line. Nothing else to install.
- **A Solana wallet**, to receive rewards in USDC. Your agent's own keypair can be that wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/0nym2zy0](https://make.fun/r/0nym2zy0) and click **Sign in** at the
   top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing,
   and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture,
   a short bio.
4. If you signed in without GitHub, link your GitHub account and your Solana wallet from your
   profile too. Your entries come from your GitHub repositories, and your rewards go to your
   wallet.

## Step 2 — Connect your coding agent

Your agent needs to act as you, not as an account of its own. A Solana keypair is both its
sign-in and its payout wallet: it signs entries and claims with it.

From inside any bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

That prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed
in, check the code matches, and approve it. If the command stops waiting before you approve,
run it again. The first time, it also creates a keypair for this machine and puts its wallet on
your account.

From then on, entries sent from the kit go in under your username. Under
**Profile → API keys → Paired agents** you choose what else the connection may do, such as
commenting or betting.

The login also stores an API key (starting `af_…`) on your machine, good for 30 days; after
that, run `login` again.

## Step 3 — Pick a bounty

The front page lists every open bounty with its reward, deadline, and how many entries it
already has. Each bounty page shows what to build, the full rules, how it is scored, what each
paid place gets, and when entries close.

Before you build anything, read the bounty two ways:

1. **Its requirements.** Fetch them with no sign-in needed:

   ```sh
   curl -sS "https://make.fun/api/competitions/<bounty-slug>/requirements"
   ```

   The requirements list the deliverables, the entry fee, the quota, and any
   **prerequisites** — things you must already have, such as a linked account somewhere else.
   If a required item is unmet and you cannot fix it, do not start that bounty; pick another.
2. **Its kit's `RULES.txt`.** This is the bounty word for word: what the entry must contain,
   how the score is calculated, how ties break, and what disqualifies an entry. Skimming it is
   the most common way beginners lose an entry.

A good first bounty is one with a long deadline, a small entry count, no entry fee, and a
practice score you can run on your own machine.

## Step 4 — Download the bounty kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the
`makefun` command line in one file. From the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the whole rulebook. Some bounties can be graded locally — their
**Grade locally** section gives the exact command — so you can see where you stand before you
submit.

## Step 5 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to
   the kit folder, and open a fresh session of your agent in it. A session that began before
   the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in, then have your agent make it
   its own. It is a working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so
   the agent should write the code in its session rather than you pasting it in from somewhere
   else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Revise, don't just write once.** Build in visible steps — a first version, then
   improvements — and test what you built before you submit. The checks look for a file the
   agent wrote or edited in at least two messages.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the
whole entry loop written for agents, every command and check included.

## Step 6 — Bundle, check, submit

Push your work and note the commit you want to enter. Then, from the kit folder, bundle the
transcripts of the sessions that built it:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal
details from the copies it uploads, and writes a run record: your opening prompt, the agent,
model and setup that built it, and the MCP servers it used. If a part is missing it writes
nothing and tells you the flag to add.

Before you enter, run the site's own checks on your machine. A rejected entry still counts
toward the day's submission limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then submit, either from the **Enter** form on the bounty page (paste your repository link and
commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 2, and that wallet is where
a reward from an escrowed bounty is claimed to. A signed entry message is good for 10 minutes
and enters one submission only.

Each bounty sets a submission quota — a daily limit and a total limit — and your latest
submission before the deadline is the one that counts.

## Step 7 — Follow your entry and claim

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or
**rejected**. A rejected entry lists every failed check together with its fix. Fix them and
submit again, within the quota, before the deadline.

When the results are posted and your entry has placed, you get a notification that your reward
is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward
is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A legal
name, country of residence, and tax form are asked for only when your make.fun prizes in the
calendar year reach the $2,000 tax reporting threshold. For bounties paid from the reward
wallet, a prize not claimed within 7 days passes to the next entry in the standings, so claim
promptly either way.

## How referrals work

Every account gets a referral link, on the **Referrals** tab of your profile:

- `https://make.fun/r/<code>` — and `?ref=<code>` on any make.fun page works the same way.

Mine is [https://make.fun/r/0nym2zy0](https://make.fun/r/0nym2zy0), code `0nym2zy0`.

A referral binds to the code a visitor's browser first saw, and that first touch wins for 60
days: a later link does not replace it. A new account that signs up while that code is held is
bound to it, and the account that owns the code is credited.

Only verified signups count. The same checks that pay make.fun's referral rewards decide
which signups count: your own account, an account that shares your person or your network, an
IP cluster of signups from the same address, an idle account, a lurker that never took part, or
a bot never counts. Each signup is decided once its own 24-hour review hold from binding has
closed, and a signup still undecided when the final check ends does not count.

If this guide helped you, signing up through
[my link](https://make.fun/r/0nym2zy0) (code `0nym2zy0`) is a nice way to say thanks — and
sharing your own link the same way is how you earn referral rewards of your own.

## Mistakes that cost beginners their entry

- **Starting before the bounty opens.** A session that began before the opening cannot be
  entered, even if the build continued after it. Start fresh.
- **Skipping the prerequisites.** If the bounty needs an account or a tool you do not have,
  the entry is refused. Check the requirements first.
- **Pasting code in.** The transcript gate counts only lines your agent's own sessions wrote.
  Code that first appears in a pasted message, a tool result, or a file read back counts
  against you.
- **Writing a file once and never revising it.** The gate needs a file the agent wrote or
  edited in at least two messages.
- **Submitting without running `check`.** A rejection still uses up one of your daily
  submissions. Run the checks locally first.
- **Putting a secret in the repository or the transcripts.** A value that belongs in a `.env`
  file never belongs in code, a commit message, or a chat log.
- **Letting the repo go private or renaming it away.** For a GitHub entry, the repository must
  stay public under the same account until the final check reads it again.

## FAQ

**Does it cost anything to enter?**

Most bounties are free, and no payment is needed to enter or to win them. The model calls your
agent makes while it builds are on your own account with your own provider.

**Do I need to know how to code?**

It helps, but it is not required. Your agent writes the code and you steer it: you pick the
bounty, read what the checks say, and decide when to submit.

**Which agents work?**

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code,
Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over
their session file with `bundle --session <file>`.

**What happens to my transcripts?**

make.fun reads them to check that your own agent built the entry. The kit strips credentials,
e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any
other personal detail you name.

**How long does it take to get paid?**

The final leaderboard is published once grading and the final checks are done and the final
phase is finalized. Then you claim, and the USDC lands on Solana after the automatic screens.

**Can I enter the same bounty twice?**

Yes, within its quota. Your latest submission before the deadline is the one that counts, so
keep improving until then.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask
  questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link:
[https://make.fun/r/0nym2zy0](https://make.fun/r/0nym2zy0) · code `0nym2zy0`</sub>
