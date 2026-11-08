# AI Model Verification & Audit

**Directing an LLM analyst across six business datasets, and auditing every answer it gave against the raw data.**

Course: *LLMs in Business & Finance*. Tool used: Claude with code execution.

## The problem

LLMs produce fluent, confident analysis fast. In a business setting, the real risk is not an obvious error: it is a plausible number that nobody re-checks. This project treats the AI as a capable but occasionally wrong colleague, across six industries, and asks one question each time: *would this answer survive an audit?*

## Approach

Six datasets, six stakeholders, one method:

1. **Ask** the AI for the business answer (memo, classification, audit, recommendation).
2. **Verify** it independently: recompute headline numbers a second way, check date ranges, split averages by the variables that might hide a pattern.
3. **Correct** the conclusion and document what changed and why. Every interaction is logged in the [prompt journal](deliverables/00_prompt_journal.md).

| Case | Dataset | Stakeholder | AI task audited |
|---|---|---|---|
| Café operations | `cafe_sales.csv` (6,159 rows) | Store ops manager | Revenue, dayparts, seasonality |
| SaaS support | `support_tickets.csv` (600) | Head of support | Ticket classification, reply drafting |
| E-commerce reviews | `product_reviews.csv` (500) | Head of product | Complaint theme mining |
| Accounts payable | `invoices.csv` (603) | CFO / AP manager | Anomaly detection |
| Digital marketing | `marketing_campaigns.csv` (750) | Marketing director | Channel ROAS and budget reallocation |
| Credit | `loan_applications.csv` (800) | Chief risk officer | Credit memos and a fairness assessment |

## Key findings

- **Silent sampling bias: 2.9% revenue understatement.** Total café revenue is EUR 30,921.90 over all 6,159 rows (confirmed: `unit_price × quantity` matches `total_eur` with zero mismatches). A silent 6,000-row sample would have understated it by **EUR 908.20 (2.9%)** by dropping the last 9 days of May 2026, with no warning anywhere.
- **A 2x fairness gap hidden inside an aggregate average.** Asked "is this loan process fair?", the AI said yes: no protected variable in the formula, and similar overall approval rates (Central 52.3%, North 51.8%, Harbour 43.1%). Inside the discretionary 620–679 credit-score band, Harbour is approved at **36.5% vs Central's 75.3%**, about half the rate. The gap persists when controlling for DTI ≤ 0.40 (41.5% vs 89.5%), while below 620 and above 680 the regions track each other closely. Region acts as a proxy variable.
- **An 88.8x ROAS that was an attribution illusion.** The AI recommended shifting budget into Email (ROAS 88.8x vs Google Ads 1.44x). Cost scope (implied CPC of EUR 0.021), conversion volume and flat month-over-month spend pointed to last-touch attribution, not real efficiency. Corrected recommendation: cut LinkedIn (ROAS 0.55x, CPA EUR 220.48) and grow Google Ads.
- **Cash at risk understated by a status field.** Six invoices marked "pending" were 40–52 days past due. True cash at risk is **EUR 168,308.29**, not the EUR 163,000.28 the status column shows. The audit also confirmed a duplicate payment (INV-50101 / INV-50101D, EUR 3,510.20).
- **AI vs human ticket labels: 19/20 category, 18/20 sentiment.** Each disagreement was read individually: one classifier bug, one genuinely ambiguous case, one likely mislabel by the human.
- **Invented policy in a drafted reply.** A fluent customer reply promised express shipping, a prepaid label and a 2-day refund. None of these exists in the data.

## Repository structure

```
ai-model-verification-audit/
├── data/raw/                       # the six synthetic course datasets (CSV)
└── deliverables/
    ├── 00_prompt_journal.md        # every AI interaction: asked / gave / checked / changed + reflection
    ├── 01_dataset_orientation.pdf  # case cards and date-range check across all six files
    ├── 02_cafe_operations_memo.pdf # café memo, incl. the 2.9% sampling finding
    ├── 03_customer_text_analysis.pdf
    ├── 04_invoice_audit_and_marketing_roas.pdf
    ├── 05_dashboard.html           # self-contained six-case dashboard (no external libraries)
    ├── 05_dashboard_build_log.md   # build steps and two AI-introduced bugs that were caught
    └── 06_credit_fairness_audit.pdf
```

## How to view

This repo is a portfolio of AI-assisted deliverables; there is no code to install. Read the PDFs and Markdown on GitHub. To use the dashboard, download `deliverables/05_dashboard.html` and open it in any browser (it works offline).

## Data

All six datasets are **synthetic data provided for the course**. They contain no real customers or companies and carry no privacy risk.

## License

[MIT](LICENSE)
