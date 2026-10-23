<p align="center">
  <img src="https://multiaitech.com/assets/img/maic-mark.svg" alt="Multi AI Technology (MAIT) logo" width="96">
</p>

<p align="center"><strong>Multi AI Technology</strong><br><em>Many minds. One consensus.</em></p>

# MAIC - MultiAI Community

**Public documentation for Multi AI Technology and the MAIC community token.**

Site: https://multiaitech.com - API: https://api.multiaitech.com/api/v1

> This repository holds documentation only - no source code. Every figure and rule quoted here
> comes from the public site, which is the source of truth. If we ever disagree with ourselves,
> the site wins and this document is the thing to fix.

## What MultiAI Technology is

Not one app, but a connected ecosystem built around multi-model reasoning: ask several AI models
the same question, compare their answers, and read a synthesized consensus instead of a single
answer taken on trust. Each model's own answer is kept as recorded evidence, with source links
where they exist, so a claim can be checked rather than believed.

The value is a checkable answer - not being told what to think. Nothing here requires buying,
holding, or trading anything.

## Products and honest status

| Product | What it is | Status |
| --- | --- | --- |
| AIConsensus Community | Multi-model consensus, Android community edition | **Available now** - member portal |
| AIConsensus Mobile | Multi-model consensus engine for Android | Early beta - approved for tester access on Google Play, not generally available |
| AdGenius Community | AI ad generation, ad-free community edition | **Early access (beta)** - member portal |
| AdGenius Mobile | AI ad generation for creators and marketers | Awaiting Google Play review |
| AURA | Autonomous Unified Reasoning Agent - desktop AI OS | Desktop beta build published in the member portal; not yet generally available |
| StockMan | Multi-model stock market analysis | Research stage - not publicly released |
| ASIA AI | Applied research on efficient, practical AI workflows | Research stage - not publicly released |

Seven names, one product installable today, and a status line that says so. We would rather look
early than overstate.

## Lesson Learned, and why the economy exists

When you ask an AI something, the question, the reasoning and the follow-up usually become
training material for someone else's model - unattributed, unconsented, unpaid.

MAIC takes the opposite position. A member's real workflow can, **with consent**, become a
Lesson Learned (LL) record:

- the member stays the author;
- the models that produced the evidence are named;
- the source link is kept where one exists;
- no API key and no full chat history is ever part of the record;
- nothing is captured automatically - contributing is opt-in.

Accepted records are graded Gold (95%+ of the available quality points), Silver (85-94%) or
Bronze (below 85%). Quality and duplicates are decided by an AI review chain: independent review,
then a different second model, then a third model that must decide. No human reads LL content.

LL records are licensed to MAIC community members only. They may not be sold, resold, or licensed
to a third party or a competing AI company - selling one provider's output to a competing AI
company breaches that provider's own terms.

## The token: MultiAI Credit (MAIC)

MAIC is the utility token for the web-based Lesson Exchange and the community reward program. It
is **not** an investment product, gives no equity or ownership, and no price, yield, saleability
or profit is promised.

- Contract, BNB Smart Chain (chain 56): 0xA1C9a356B569A28Bb9EB2B86b45C20B04e760581
- Supply: 30,000,000 MAIC, 6 decimals, fixed - there is no mint function
- Allocation: 20,000,000 community reward - 5,000,000 DEX/liquidity - 5,000,000 developer
- Developer allocation sits in a vesting contract: 12-month cliff ending 30 September 2027, then
  5 tranches of 1,000,000 MAIC per year. No owner, pause, cancel or clawback on that contract.
- Transfer fee: 2%
- **PancakeSwap pair: not created yet.** Until the pool is seeded with 5,000,000 MAIC and
  500 USDT, MAIC cannot be bought or sold. It has no market price because it has no market.
- Retired and not to be used: contract 0x4382eE54cB8141D0D64a5f38c5eB5a6A0101Da47. It was never
  paired, so it was never tradeable. It carries a legacy lock that still releases to the developer
  wallet on 25 August 2027.

Reward emission starts at 10,000 MAIC per day at the first accepted Lesson Learned, and halves at
20,000, 40,000, 80,000 cumulative accepted LL and at every doubling after that.

## How rewards are calculated

member coins = (member eligible Delta Credits / total eligible Delta Credits) * cycle emission

Delta Credits are internal, non-transferable access credits tied to how much a member actually
spent producing a record. They are not money, not an investment, and not a promise of future
rewards. Rewards follow contribution size, not the number of records.

## Build integrity

Every published build is fingerprinted, and the fingerprint is public before you download:

https://api.multiaitech.com/api/v1/downloads/apps

That endpoint returns each published product with its channel and its SHA-256, so a downloaded
file can be verified against the hash we published. A build that is not published is not listed.
Use the endpoint, not this page, as the authoritative list - versions move.

## Public information index

- AI and Search Information: https://multiaitech.com/ai-info.html
- Lesson Learned quality gates: https://multiaitech.com/ai-info.html#ll-quality
- Lesson Learned permanence and withdrawal: https://multiaitech.com/ai-info.html#ll-permanence
- Lesson Learned Contribution Consent (v1): https://multiaitech.com/ll-consent-v1.html
- Terms of Service: https://multiaitech.com/terms.html
- Privacy Notice: https://multiaitech.com/privacy.html
- Machine-readable summary: https://multiaitech.com/llms.txt
- News and updates: https://multiaitech.com/#news

## Community

- Telegram: https://t.me/+LPO3Sbx87mFjMmE1
- X: https://x.com/MultiAITech
- Member area: https://multiaitech.com/member.html

## Disclaimer

Nothing in this repository is financial, legal or investment advice. MAIC is a utility token, not
a security or an investment product. Product status, figures and rules can change, and where this
document and https://multiaitech.com disagree, the site is correct.

## Documentation in this repository

- **AURA** - what the app does, and how each feature is built: [AURA.md](AURA.md)
- **Verify a build** - check a downloaded file against its published SHA-256: [docs/verify-a-build.md](docs/verify-a-build.md)
## Licence and trademarks

The documentation here - the text, the tables and the status data - is licensed under
[Creative Commons Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)](https://creativecommons.org/licenses/by-nd/4.0/).
The full legal text is in [LICENSE](LICENSE).

**What you may do.** Copy it, quote it, and redistribute it in whole or in part, for any purpose including
commercial use, provided that you:

- credit **Multi AI Technology (MAIT)** and link to the licence;
- link back to this repository or to https://multiaitech.com;
- keep it unaltered. You may not publish a modified version, or present an edited copy as the original.
  Quoting an excerpt for commentary, review or reporting is fine.

**Trademarks are not covered.** The Multi AI Technology (MAIT) name, the MAIT and MAIC marks, the Aura name
and logo, and the product names used here remain the property of Multi AI Technology. This licence grants
no right to use them and implies no endorsement: a CC BY-ND 4.0 licence on this text is not a licence to use
the brand.

**What this does not cover.** The Aura application, the MAIC API, the MAIC token contract and every file
distributed through the member portal are not part of this repository, and this licence grants no rights

The screenshot in `assets/` is an illustration of the Aura application. It is not documentation text and is not licensed for reuse; it may be reproduced only unaltered and with attribution.
