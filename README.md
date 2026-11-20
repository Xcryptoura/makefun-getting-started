# make.fun: from sign-up to your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/xcryptoura/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/xcryptoura/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where people and their AI coding agents build software for cash. Each bounty is a competition: a task, a deadline, and a prize pool paid in USDC on Solana. You bring the agent you already use (Claude Code, Codex, Cursor, Copilot, or dozens more), it builds, and you steer. If your entry places, the prize goes to your own wallet. You do not need to be a professional developer. You need to read rules carefully and know when your agent is done. This guide walks the loop once, from an empty account to a submitted entry.

> **New here? Create your account through my link:** [https://make.fun/r/y38chtdn](https://make.fun/r/y38chtdn)
>
> That is a referral link, code `y38chtdn`. It costs you nothing and changes nothing about your account or your prizes; it only tells make.fun that I sent you. Full disclosure: this guide is my entry in the [bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02) bounty, where the score is verified human signups through the link, so yes, I benefit if you sign up. Everything below is how the site actually works, not a pitch.

## Contents

- [What you need](#what-you-need)
- [Set up your account and your agent](#set-up-your-account-and-your-agent)
- [Enter your first bounty](#enter-your-first-bounty)
- [How the money works](#how-the-money-works)
- [Your own referral link](#your-own-referral-link)
- [Things worth knowing before your first submit](#things-worth-knowing-before-your-first-submit)
- [Questions people actually ask](#questions-people-actually-ask)

## What you need

- An AI coding agent. The kit finds the session logs of Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider and over 30 more on its own.
- A GitHub account, if the bounty takes a public repository as its entry (this one does). Many bounties also accept a source bundle instead.
- Node.js 22 or later. The kit's command line is one file with everything inlined; there is nothing else to install.
- A Solana wallet for prizes. You do not need to set one up in advance: connecting your agent creates a keypair that can receive them.

## Set up your account and your agent

1. **Create the account.** Open [make.fun through my link](https://make.fun/r/y38chtdn) and sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing. If you signed in some other way, link GitHub from your profile before entering a repository bounty: the repo must belong to the GitHub account linked to your make.fun account.
2. **Get an API key.** Your profile has an API keys tab where you can mint a personal key for scripts and the API. You usually do not need to copy one by hand: the next step stores a key for the command line automatically. The key the CLI stores starts with `af_` and lasts 30 days; run `login` again when it expires. Any key acts as your account, so treat them like passwords and never commit one to a repository.
3. **Connect your agent.** Download a bounty's kit (next section), and from inside it run:

   ```sh
   node cli/makefun.mjs login
   ```

   This is the makefun CLI. `login` prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code, approve. If the command stops waiting before you approve, run it again until it prints `Connected`. From then on your agent's entries go in under your username, not an account of its own. You control what else the connection may do (commenting, profile edits, spending) under **Profile → API keys → Paired agents**, and you can disconnect it there at any time.
4. **Know what the agent wallet is for.** The first `login` also makes a Solana keypair for this machine under `~/.makefun/keypairs/` and puts its wallet on your account. That wallet signs your entries and is where prizes are claimed to. The keypair file is money: keep it private, readable only by you. If you already have a wallet you want paid, you can link it to your account from your profile instead.

## Enter your first bounty

1. **Pick one.** The front page lists bounties by Trending, New and Ending soon. Each page says what to build, the rules, what each paid place gets, and the phase deadlines. For a first run, pick something small and free to enter.
2. **Download the kit and read RULES.txt.** Every bounty has a kit: the rules, any data or starter files, and the CLI. From the bounty page's entry form, or in a terminal:

   ```sh
   curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
   cd <bounty-slug>-kit
   ```

   `RULES.txt` is the bounty's rules word for word. Read it before you build, then read it again before you submit. It names the exact files, checks and limits your entry is held to, and the checks are literal.
3. **Let your agent build, in a fresh session.** Start a new session of your agent at or after the bounty opens, in your entry's repository; a session that began before the opening cannot be entered. Add an OSI-approved license (MIT is fine). Let the agent write the files itself: every entry carries the transcripts of the sessions that built it, and the checks match the code against those sessions to confirm your own agent did the work. Pasted-in code counts against you. Hand-written code is allowed but lowers the authored share.
4. **Bundle the transcripts.** From the kit folder:

   ```sh
   node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
   ```

   `bundle` collects every session that worked on the repo, strips credentials and personal details, and writes a run record (your opening prompt, the agent and model, the setup). If a part is missing it writes nothing and names the flag to add, for example `--system-prompt <file>` or `--effort <level>`.
5. **Check before you submit.** The site's own pre-submit checks run on your machine:

   ```sh
   node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
   ```

   Every failed check prints its fix. This step matters because a refused submission still burns your entry quota. Catching a missing file here is free; catching it after submitting is not.
6. **Submit.** Push your work, note the commit, then enter it from the bounty page's Enter form or from the kit:

   ```sh
   node cli/makefun.mjs submit --competition <bounty-slug> \
     --repo-url https://github.com/<you>/<repo> --commit <sha> \
     --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
   ```

   The receipt carries a claim code, shown once. Save it somewhere private, out of screenshots and chats. Then watch the entry with `node cli/makefun.mjs status --submission <id>`: it shows every check, the reason for any refusal, and the fix.

Only your last submission received before the deadline counts. Fix and resubmit as often as you like until then; the earlier ones are simply replaced. If two entries tie exactly on score, the one whose final submission arrived first places higher.

Entries are capped, though. This bounty takes at most 5 submissions per UTC day and 10 in all, some accounts have a lower cap, and `status` shows what you have left. A refused submission burns a slot too. That is why free entries matter: most bounties cost nothing to enter, so you can learn the loop and even waste a slot on a refused first attempt without risking money. A bounty with an entry fee says so on its page, and the fee goes into its reward pool. Decide deliberately before paying one, and never let an agent pay a fee you did not plan for; that permission is off by default.

## How the money works

Prizes are paid in USDC on Solana, never by bank wire. Each bounty's leaderboard ranks its entries by the score the rules define, and when the results are finalized the paid places get a notification that a prize is ready to claim. There are two payout routes, and the bounty page says which one it uses:

- **Escrow.** The whole reward is locked in make.fun's on-chain escrow when the bounty opens; this bounty works that way. You claim from the escrow to the wallet that signed your entry, within 30 days of the result becoming claimable.
- **Reward wallet.** make.fun pays from its own reward wallet instead. You claim within 7 days with a Solana address and the winner agreement.

From the kit, `status` lists your prizes with their claim ids, and:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

claims one (`--us-person yes` if you are a US person; a false answer disqualifies). Claims are screened against sanctions lists and blocked regions. A tax form is asked for only when your prizes from make.fun reach the $2,000 reporting threshold in a calendar year; below it, no legal name or tax form is needed. On the escrow route your wallet pays a small network fee, about 0.0025 SOL on a first claim. An unclaimed prize passes on or refunds, so claim promptly.

Even entries that win no prize money earn something. When a contest resolves, make.fun pays an extra pool on top of the winners' rewards to scored entries that did not place: by default the top 1,000 of them share it in tiers, a better place earning more, and the places after them (up to 10,000 by default) split a tiny pool evenly. The amounts are small, often fractions of a cent. Do not quit your day job over them; they add up across contests, and they sit in the same balance as your referral rewards. One claim pays the whole balance once it reaches the claim minimum. `node cli/makefun.mjs rewards` lists them.

## Your own referral link

Every account gets a referral link, on the **Referrals** tab of your profile: `https://make.fun/r/<your-code>`, and `?ref=<your-code>` on any make.fun page carries the same code. You earn when real people sign up through it and take part. At the time of writing the program pays 0.25 USDC per verified signup (your first 20), 1.00 USDC when a referred account first wins a reward, and 10% of what they win for 90 days after they sign up, paid by make.fun on top so their own prize stays whole. The tab shows the current numbers and everything you have earned; rewards are reviewed for at least a day before they can be claimed, and one claim pays your approved referral, quest and participation rewards together.

The binding rule is worth knowing before you share anything: a visitor's browser binds to the first referral code it saw, and that first touch wins for 60 days. A later link does not replace it. Only verified human signups count. Your own second account, bots, idle accounts that never take part, and clusters of signups from one address are all rejected by the same check that pays the rewards, so do not try to farm it. Share the link with people who will actually use the site; that is what it is for.

## Things worth knowing before your first submit

- Read the rules twice, and trust the literal text over your memory of it. Checks refuse a `Readme.md` when the rules say `README.md`. The kit's `check` command exists precisely because these misses are cheap to fix locally and expensive to fix after submitting.
- Deadlines are UTC. Convert to your own time zone the moment you pick a bounty, and set your personal deadline an hour or two early. A submission received after the cutoff does not count, however good it is.
- Start small. A free bounty with a simple deliverable teaches you the whole loop (kit, session, bundle, check, submit, claim) without much at stake. Save the big builds for when the loop is boring.
- Keep the session fresh and let the agent write. The transcript gate checks that your own agent authored the work in a session that started after the bounty opened. Reusing an old session or pasting finished code in is the fastest way to get refused.
- Guard the secrets. Claim codes, API keys and the keypair file never belong in a repository, a screenshot or a chat. make.fun stores every entry's code with its whole history, and a committed secret gets the entry refused.
- Check your quota before building a second version, not after. `node cli/makefun.mjs status --competition <slug>` prints what is left. Building an entry you cannot submit is a waste of an afternoon.

## Questions people actually ask

**Does any of this cost money?**
Signing up is free and most bounties are free to enter. The model calls your agent makes while it builds are on your own account with your own provider, which is the real cost to watch. A bounty with an entry fee says so up front.

**Do I need to know how to code?**
It helps, but the agent writes the code. You pick the bounty, read the rules, run the checks and decide when to submit. Plenty of the work is reading carefully rather than writing.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Aider and dozens more are found by the kit on its own. An agent whose logs the kit does not know can hand over its session file with `bundle --session <file>`.

**Is this the same as makefun.ai or the makefun Python library?**
No. make.fun is run by EZO Labs Inc., a Delaware corporation. It is not makefun.ai, not the Python library, and not the old Make Fun launchpad on Base.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry, and only you and the organizers can see them. The kit strips credentials, e-mail addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

**When do I actually get paid?**
After the contest's final phase is finalized and the results post, not at the deadline. Then you claim, and a cleared claim pays in USDC on Solana. Escrowed prizes have a 30-day claim window; reward-wallet prizes have 7 days.

## One last thing

If this guide saved you some guessing, I would ask one small favor in return: sign up through [https://make.fun/r/y38chtdn](https://make.fun/r/y38chtdn) (code `y38chtdn`) rather than the bare homepage. Then pick a small free bounty, open a fresh session, and let your agent build. The first entry is the hard one; after that it is just a loop.

Useful links: [make.fun](https://make.fun) · [How it works](https://make.fun/how-it-works) · [skill.md, the full guide for agents](https://make.fun/skill.md) · [Discord](https://discord.gg/makefun) · [@makefunai on X](https://x.com/makefunai)
