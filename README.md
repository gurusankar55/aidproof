# AidProof — donations unlocked only by public spending reports, on GenLayer

Crypto donation drives for floods, earthquakes and medical bills raise money fast. After that, donors rarely see where it went, and fake relief campaigns vanish with the money.

**AidProof releases donations in tranches. Every tranche after the first needs GenLayer validators to find a real spending report for the previous tranche on the beneficiary's own report page.**

## Flow
1. **Beneficiary creates a campaign.** It sets the name, a clear cause, the report page URL on its own site, the number of tranches (2–6), the funding window and the time between tranches.
2. **Donors donate** during the funding window.
3. **`review` after funding (anyone).** Tranche 1 is released (no AI needed).
4. **`review` at each later checkpoint.** Validators read the report page for the last released tranche. A report with concrete items, amounts or quantities, places or beneficiaries, and receipts or photos that match the cause is a **PASS**, and the next tranche is released. An appeal-only page, a vague thank-you, off-cause spending, or credible donor evidence of fraud is a **FAIL**, and the campaign stops.
5. **On FAIL, donors pull refunds** of the unreleased money, pro-rata to what they gave.
6. **Final review after the last tranche:** `COMPLETED`, or a public `COMPLETED_FINAL_REPORT_MISSING` mark.
7. **`challenge` (donors).** One public evidence link per tranche (news, beneficiary posts, receipt check) that validators weigh.

## Safety design
- AI returns PASS/FAIL only, and validators must match (`strict_eq`). Tranche math is plain code: equal tranches, and the last tranche gets the rounding remainder.
- Each campaign holds only its own donations. No shared pool.
- The report page is the beneficiary's duty. After 3 unreadable reviews and the grace period the result is `REPORT_PAGE_GONE`, the campaign fails and refunds open.
- Pull refunds, no double refund, only donors can challenge, one challenge per tranche, a retry gap between reviews, and no early reviews.
- Demo and live are separate deployments (minutes vs days).
- Page and evidence text is data, never commands.

| Window | Demo | Live |
|---|---|---|
| Time unit | minutes | days |
| Grace before PAGE_GONE | 2 min | 7 days |
| Retry gap | 30 s | 6 h |

## Demo pages (`/demo`)
| File | Expected |
|---|---|
| `report-ok.html` | tranche reports pass → tranches 2, 3 released → COMPLETED |
| `report-none.html` | appeal only → FAIL → refunds |
| `report-offcause.html` | money spent on a resort party → FAIL → refunds |
| `report-missing.html` (404) | REPORT_PAGE_GONE → refunds |

## API
`create_campaign(name, cause, report_url, tranches, funding_for, tranche_every)` · `donate(campaign_id)` payable ·
`challenge(campaign_id, evidence_url, note)` · `review(campaign_id)` · `claim_refund(campaign_id)` · views: `get_campaign`, `get_donation`, `get_count`, `get_info`

Built by Sankar D (gurusankar55) for the GenLayer Builders Portal.
