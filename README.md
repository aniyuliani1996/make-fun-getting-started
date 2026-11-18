# Getting Started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/aniyuliani1996/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/aniyuliani1996/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you (and your AI coding agent) build real software for real USDC rewards. Someone posts a task with a deadline and a prize pot; you build an entry, submit it, and if it places, you claim the prize to your own Solana wallet.

This guide walks you from zero to your first submitted entry.

> **Start here — sign up with my link:** [https://make.fun/r/hkdhmkfa](https://make.fun/r/hkdhmkfa)
>
> My referral code is `hkdhmkfa`. It costs you nothing; it just tells make.fun who pointed you here.

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build your entry](#step-4--build-your-entry)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Read the verdict, fix, resubmit](#step-6--read-the-verdict-fix-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Bring your own friends: referrals](#bring-your-own-friends-referrals)
- [Honest answers to common questions](#honest-answers-to-common-questions)

## What make.fun is

Every bounty on make.fun follows the same loop:

1. **Open** — a task, a deadline, and a prize in USDC are published.
2. **Build** — you (usually with an AI coding agent) build the entry.
3. **Submit** — you send a public GitHub repository at one commit, plus the transcripts of the agent sessions that built it.
4. **Check** — automated checks confirm the transcripts are genuine and match the code, and that the rules were followed.
5. **Score** — entries are graded the same way, in a sandbox.
6. **Pay** — winners claim USDC on Solana.

Three facts that matter before you spend an evening on it:

- **Most bounties are free to enter.** A bounty with an entry fee says so on its page. This guide assumes the free ones.
- **The prize money is locked up front.** New bounties open with the full reward in an on-chain escrow on Solana, and winners claim from that escrow. The escrow program is public code.
- **Your agent's session logs are part of the entry.** They prove your agent did the work. That is how the site stops copied code from winning.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, and dozens of others all work. If you have never used one, pick the one whose setup you can finish in ten minutes — the agent matters less than finishing.
- **A GitHub account.** Entries live in public repositories. Link it to your make.fun profile (see Step 1).
- **Node.js 22** on your machine, to run the kit's command line. Nothing else to install for most bounties.
- **A Solana wallet** to receive USDC. Your agent can generate a keypair for you; it doubles as your signing wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/hkdhmkfa](https://make.fun/r/hkdhmkfa) and click **Sign in** (top right).
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in is free and signs no transaction.
3. Open your profile (your avatar, top right) and fill it in: username, picture, a one-line bio.
4. Link the other pieces from your profile: your GitHub account (your entries come from its repositories) and your Solana wallet (your rewards go there). If you signed in with GitHub, that link is already done.

Your agent will need to act as you once. From a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code matches, approve. If the command times out before you approve, just run it again. The first login also creates a Solana keypair on your machine; later commands sign your entries with it.

## Step 2 — Pick a bounty

The front page groups bounties into Trending, New, and Ending soon, across categories like Code, Games, Trading, Forecasting, and Growth. Each card shows the prize; each bounty page shows:

- **The task**, in one line and then in full
- **Rules** — what an entry must contain, how it is scored, how ties break
- **Rewards** — what each paid place gets
- **Phases** — when entries close and when results land

Read the rules before you build anything. Some bounties need extra software or an account elsewhere, and the page says so up front. As a first bounty, prefer one whose rules you understand completely and whose deadline is at least a day away.

A quick checklist for your first pick:

- You can explain the task back in one sentence.
- The kit has a starter entry, or the rules name a single clear deliverable.
- Nothing in the rules needs an account or a download you cannot get.
- The deadline leaves you time to get rejected once, fix it, and resubmit.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. From the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. Read it — it is the only document that decides whether your entry counts. Where a bounty can be graded on your own machine, the rules give the exact local command.

## Step 4 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it, and open a fresh session of your agent inside it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license** such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and make it your own. It is a working entry that meets the rules — not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so pasting code in from elsewhere counts against you. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Ship the README the bounty asks for**, where one is required, with the badges and links the rules name.

One habit that saves entries: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, commands and checks included.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and tells you which flag to add.

Run the site's own checks on your machine before you enter — a rejected entry still counts toward the day's limit:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 1. That wallet is where the reward will be paid.

## Step 6 — Read the verdict, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and their stage: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit. Your latest entry is the one that counts.

Submit early when you can: on bounties where an exact tie goes to the earlier entry, the receipt time is the tie-breaker, and a fix round after a rejection still leaves you ahead of everyone who only entered at the deadline.

## Step 7 — Claim your reward

When results are posted and your entry has placed, you get a notification. Claim from your profile, or from the kit with the claim id `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where the law requires one.

## Bring your own friends: referrals

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works the same way. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after they sign up — paid on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped you, signing up through [my link](https://make.fun/r/hkdhmkfa) (code `hkdhmkfa`) is a good way to say thanks — and it is exactly what this guide was written to earn.

## Honest answers to common questions

**Does it cost anything?**
Most bounties are free to enter. No payment is needed to enter or to win them. The model calls your agent makes while building are on your own account.

**Is make.fun legit?**
It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. Note: make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code and you steer: you pick the bounty, run the local checks where there are any, read what the verdicts say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Aider, and dozens more are found by the kit on its own.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses, and home paths before upload.

**Can I enter with a friend?**
One account per person. A team enters under one account and splits any prize itself.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/hkdhmkfa · code `hkdhmkfa`</sub>
