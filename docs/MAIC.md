# MAIC Tier & Delta Credit Release — Update 20 Nov 2026

## Status
🟢 Deployed & verified on production server.

## Subscription Tiers
| Tier | Price | Required LL | Delta Credit |
|---|---|---|---|
| Free | Forever | Yes | No |
| Pro | $5/mo or $50/yr | Yes | Yes |
| Advance | $10/mo or $100/yr | Optional | Only if LL contributed |

## Delta Credit Accumulation & Release
- Delta credit accumulates per member during membership.
- When token release quota (200 paid Pro/Advance yearly members) is met, accumulated credits are released progressively.
- Formula: `daily_rate = accumulated_credits / accumulation_days`.
- Example: 10,000,000 credits over 20 days → 500,000/day.
- Example: 20,000 credits over 10 days → 2,000/day.
- Free tier: delta credit zeroed; LL accepted but no delta credit.
- Downgrade to Free: future LL produce no delta credit; already accumulated credits not clawed back.

## Files Changed
- backend/src/services/membershipTiers.js
- backend/src/services/deltaCreditRelease.js
- backend/src/services/deltaAssessment.js
- backend/src/services/membershipPlan.js
- backend/src/db/migrations/091_delta_credit_accumulation.sql
- backend/src/db/migrations/093_delta_credit_dynamic_rate.sql
- backend/src/routes/me.js, meUsage.js, admin/releases.js
- backend/src/jobs/runner.js
- admin.html, member.html, assets/js/admin.js, assets/js/member.js, assets/css/member.css
- MAIC.md section 52

## Tests
- Full suite: 1128 tests passed, 0 failed.
