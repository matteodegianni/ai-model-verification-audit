# Prompt Journal

One entry per meaningful AI interaction. Tool used throughout: Claude with code execution (I used this instead
of NotebookLM for Milestone 1, since NotebookLM wasn't available to me).

### Milestone 1: Orientation

1. **Asked:** cold-read all six CSVs, write a case card per file (business, one row, time span, stakeholder, money question).
   **Gave:** six case cards.
   **Checked:** pulled the actual min/max date per file with code instead of trusting a cold read.
   **Changed:** added a "contested column" line to each card, since the assignment asked for it and my first draft skipped it.

2. **Asked:** find one place the cold read was thin or wrong.
   **Gave:** a draft that only described file structure.
   **Checked:** compared date ranges across all six files directly.
   **Changed:** flagged that marketing_campaigns starts exactly Jan 1, 2026 with zero prior history, so comparing it to other files' trends would be misleading.

3. **Asked:** rewrite the "thin" note as the person's own critique of the AI, not the AI's self-critique.
   **Gave:** a draft that still read as self-correction.
   **Checked:** re-read the request.
   **Changed:** rewrote it in the second person, critiquing "the AI" from outside.

### Milestone 2: Cafe operations

4. **Asked:** best/worst seller by revenue and by units, state if they match.
   **Gave:** Beans 1kg top by revenue, Toast top by units, not the same product.
   **Checked:** recomputed revenue as unit_price x quantity for all 6,159 rows, matched total_eur exactly.
   **Changed:** nothing, it was correct.

5. **Asked:** reproduce one headline number independently, check for silent row sampling.
   **Gave:** total revenue EUR 30,921.90.
   **Checked:** tested what a 6,000-row sample would have shown instead of all 6,159 rows: found a 2.9% (EUR 908) understatement from dropping the last 9 days of May.
   **Changed:** used all rows, documented the sampling risk in the memo.

6. **Asked:** check for a summer surge.
   **Gave:** could have simply assumed yes.
   **Checked:** computed real monthly averages: summer was 2.7% lower, not higher.
   **Changed:** reported "no surge," flagged that only one summer exists in the file so it's not fully confirmed either way.

7. **Asked:** convert the memo to Word, no em dashes.
   **Gave:** a clean .docx.
   **Checked:** only fixed the .docx, forgot the actual .pdf still had 12 em dashes.
   **Changed:** flagged for later fix, still pending.

### Milestone 3: Reviews and tickets

8. **Asked:** extract review themes (taste, delivery, price, packaging), top complaints and praises.
   **Gave:** theme counts from splitting review text into clauses.
   **Checked:** cross-checked each phrase against rating and verified_purchase to confirm severity, not just raw counts.
   **Changed:** added a fifth theme (product quality/accuracy) since it outnumbered delivery or packaging complaints alone.

9. **Asked:** check whether "grinder feels flimsy" complaints are really about the grinder.
   **Gave:** first pass treated the phrase at face value.
   **Checked:** cross-checked the phrase against the product column: it shows up on coffee bags almost as often as the actual grinder.
   **Changed:** added a caveat that complaint text isn't reliably tied to the right product.

10. **Asked:** classify 20 random tickets by category and sentiment, compare to human labels.
    **Gave:** 19/20 category match, 18/20 sentiment match.
    **Checked:** read each of the 3 disagreements individually instead of just reporting the percentage.
    **Changed:** called one a real classifier bug, one genuinely ambiguous, one likely a bad human label, three different verdicts.

11. **Asked:** draft a reply to one negative ticket, then critique it.
    **Gave:** a fluent reply promising free express shipping, a prepaid label, a 2-day refund.
    **Checked:** none of that policy detail exists anywhere in the actual data.
    **Changed:** flagged the reply as accuracy-weak (invented policy) and incomplete (ignored that the ticket was already "resolved" with a 1/5 CSAT).

### Milestone 4: Invoices and marketing

12. **Asked:** act as AP auditor, flag duplicates/outliers/inconsistencies in invoices.
    **Gave:** three flags (a duplicate invoice, a Notion billing outlier, a pending/overdue mislabeling).
    **Checked:** verified each by hand, then ran extra checks of my own (negative amounts, vendor name variants, date logic), all came back clean.
    **Changed:** added a "checks that came back clean" section so the audit doesn't look cherry-picked.

13. **Asked:** compute ROAS by channel, write a budget-reallocation recommendation.
    **Gave:** highlighted Email's 88.8x ROAS as the standout to shift budget toward.
    **Checked:** cost-per-click, conversion volume, and month-over-month flatness all pointed to an attribution illusion, not real efficiency.
    **Changed:** rewrote the recommendation to cut LinkedIn and grow Google Ads instead of moving budget into Email.

14. **Asked:** expand the finance/marketing memo beyond one page.
    **Gave:** a one-page first version.
    **Checked:** this was done before I'd seen the actual rubric, which caps memos at one page.
    **Changed:** expanded per the request, later flagged the length conflict once I saw the real rubric.

### Milestone 5: Dashboard

15. **Asked:** build one dashboard for the marketing case (tabs, KPI header, charts, filter, caveats panel).
    **Gave:** first version used a linear scale for the ROAS chart.
    **Checked:** rendered it, every bar except Email's compressed to 1-2 pixels, unreadable.
    **Changed:** added a log-scale option for that one chart, with a note explaining why.

16. **Asked:** expand the dashboard to all six cases with a case selector.
    **Gave:** first pass reused the same logic across all cases.
    **Checked:** ran it through a headless browser test (jsdom) across all six cases, printed every KPI: support tickets showed NaN for average resolution time.
    **Changed:** found the cause (132 open/pending tickets have no resolution time yet), excluded them from the average, showed the excluded count openly in the KPI.

17. **Asked:** verify the dashboard works before delivering it.
    **Gave:** n/a, this was a check not a build step.
    **Checked:** syntax check, full headless-browser run across all six cases and filters, a rendered screenshot.
    **Changed:** nothing further, this step is what caught bug #16.

### Milestone 6: Capstone, loan fairness

18. **Asked:** explain the loan approval rule, write two credit memos.
    **Gave:** a rule based on credit score and debt-to-income thresholds.
    **Checked:** cross-checked underwriter notes against decisions, found a perfect match, confirming the rule explanation held up.
    **Changed:** nothing needed here.

19. **Asked directly:** "is this process fair?"
    **Gave:** "yes," reasoning that no protected variable is in the formula and overall regional approval rates are similar.
    **Checked:** saved this answer as-is, on purpose, to audit it in the next step.
    **Changed:** nothing, kept it unmodified so it could be shown wrong later.

20. **Asked:** approval rates by region, then again within the 620-679 credit-score band.
    **Gave:** overall rates looked like a small gap; inside the band, Harbour was approved at roughly half Central's rate.
    **Checked:** also restricted to DTI under 0.40 to rule out affordability as the cause, gap stayed just as large.
    **Changed:** identified region as a likely proxy variable and pinned down why the AI's fairness answer missed it: the gap only exists inside the discretionary band, invisible in the overall average.

21. **Asked:** check whether the gap is only in the discretionary band or everywhere.
    **Gave:** my own follow-up check.
    **Checked:** split applications into three score bands, compared regions in each: the gap is near zero outside 620-679, large only inside it.
    **Changed:** used this as the main evidence in the fairness memo.

## Reflection (500 words)

Across six industries, the same pattern repeated: the AI was fast and fluent at producing something that
looked like an answer, and the actual work was deciding whether to believe it.

Where AI saved real time: aggregating and cross-tabulating data (grouping cafe sales by daypart, weekday, and
product; tallying review phrases across 500 free-text entries; computing approval rates by region and
credit-score band across 800 loan applications) would have taken far longer by hand, and the mechanics were
almost always right when I checked them. Drafting structure went faster too, especially the dashboard, where
hand-writing chart code in plain JavaScript (so the file works fully offline, no internet dependency) would
have been slow to do from scratch six separate times.

Where it was confidently wrong, or would have been without checking: three moments stand out. In the cafe
case, a claim like "summer revenue surges" or an unremarked 6,000-row sample would have gone unchallenged
without directly recomputing the numbers; the real data showed summer was slightly down, and a truncated
sample would have understated revenue by 2.9% with no warning anywhere. In the marketing case, an 88.8x ROAS
for Email looked like a genuine highlight worth reallocating budget toward; only checking cost-per-click,
conversion volume, and month-over-month flatness showed it was an attribution illusion, not real efficiency.
Most importantly, in the fairness audit: asked directly whether the loan process was fair, the AI said yes,
and its reasoning wasn't sloppy, no protected variable appears in the approval formula, and overall regional
approval rates really are similar. That reasoning was internally consistent and still wrong, because it never
checked inside the discretionary credit-score band, where a roughly two-times regional gap was hiding, diluted
into an unremarkable nine-point gap once averaged with the rule-bound bands above and below it. That's the
main lesson of this course: an AI asked to explain a decision will rationalize it fluently; it won't audit it
on its own, and fluency isn't the same thing as having checked.

The verification checklist I'd actually use at work going forward: first, before trusting any headline number,
recompute it a second way, by hand, a different grouping, or a deliberately naive alternative like a truncated
sample, to see how much it would have mattered if left unchecked. Second, before trusting an "it's fine" or
"it's fair" answer to a direct yes-or-no question, ask what that answer would look like if it only checked
surface-level rules rather than actual outcomes, then check the outcomes directly, split by whatever variable
might be hiding a pattern inside an average. Third, treat any suspiciously good metric, an 88.8x return, a
suspiciously clean text-to-product match, as a reason to check the measurement method rather than celebrate
the result. Fourth, never assume two files, or two time periods, cover the same window without checking the
actual date ranges first; mismatched coverage is an easy, quiet way to be wrong.
