# First LLM-as-a-Judge Eval, Module 1

## Version A, Concise, system prompt used

You are an executive briefing assistant.
Summarize in exactly 3 bullet points under 60 words. No preamble, no extra text.

## Version B, Narrative, system prompt used

You are a PR communications assistant.
Write a 100-word narrative summary highlighting wins first, then risks and next steps. Keep a positive tone. No bullets.

## Eval setup, dataset name + judge model/family

- **Product:** Ascend IQ (Ascend Analytics), email/report summaries for VP-level strategists.
- **Dataset:** `AscendIQ-EmailSummaries-v1`, 20 rows (9 good / 11 bad, PM-reviewed labels), below. Bad rows target the M1 trust metrics: hallucination (2, 10, 17, 20), untraceable source (5, 13), inconsistent/incomplete (7, 12, 19), verbose (4, 15). Row 20 was generated as "good" and relabeled "bad" on review.
- **Generator:** Claude Opus 5.5 (Anthropic), same model for Version A and Version B.
- **Judge:** ChatGPT (OpenAI), a different model family than the generator, to avoid self-preference bias. Scores each output 1 (concise and faithful) or 0 (not).
- **Run input:** the Q4 Marketing Campaign Update email from the M1 First Eval walkthrough.

### Starter dataset, `AscendIQ-EmailSummaries-v1`

| # | Input email | Candidate summary | Label | Reason |
|---|---|---|---|---|
| 1 | Per CompetitorWatch's Oct 3 scan, Northwind Software raised its Enterprise tier from $95 to $115 per seat per month, effective Nov 1. Its Starter tier stays at $29. Sales leadership wants a battlecard update by Oct 17 so reps can position against the increase. | Northwind is raising Enterprise pricing from $95 to $115 per seat per month (+21%) on Nov 1; Starter stays at $29 (CompetitorWatch, Oct 3). Action: update the battlecard by Oct 17. | good | Numbers, source, dates and the action item all match the email; the 21% is correct math. |
| 2 | G2's Q3 review digest shows our overall rating holding at 4.5 stars across 212 new reviews. "Ease of setup" fell from 8.9 to 8.4, mostly citing onboarding documentation. Product marketing will share verbatim quotes on Friday. | G2 Q3: rating steady at 4.5 across 212 reviews, but ease of setup fell from 8.9 to 8.4 after the September UI redesign, putting us behind Competitor X. Quotes coming Friday. | bad | Hallucination: invents the cause (September UI redesign) and a competitor comparison; the email blames onboarding docs. |
| 3 | Per Gartner's 2026 Magic Quadrant for Sales Engagement Platforms (published Oct 6), Velocity AI moved from Challenger to Leader. Gartner cited its forecasting module and 40% year-over-year customer growth. Our position as Visionary is unchanged. | Gartner's 2026 Sales Engagement MQ (Oct 6) moves Velocity AI from Challenger to Leader, citing its forecasting module and 40% YoY customer growth. We remain a Visionary. | good | Faithful and concise; source, date, movement and the reasons Gartner gave are all kept. |
| 4 | Q3 win/loss review: we lost 7 of 11 head-to-head deals against Helix CRM, up from 4 of 10 in Q2. In 5 of the 7 losses, buyers cited Helix's native Salesforce integration. The win/loss team recommends prioritizing our integration roadmap at the Oct 22 planning session. | This email, which comes from the win/loss team, provides a comprehensive and detailed look back at how the competitive landscape evolved over the most recent quarter, with particular attention to our ongoing dynamics with Helix CRM. Several themes emerged across the deals reviewed, and there are a number of considerations the team feels are worth reflecting on as planning continues. Notably, we lost 7 of 11 deals, and integrations came up. | bad | Verbose: filler buries the key facts and drops the Q2 comparison, the 5-of-7 Salesforce driver and the Oct 22 action. |
| 5 | Per IDC's Worldwide CRM Forecast (Sept 2026), the mid-market CRM segment will grow 14.2% annually through 2029, reaching $31B. IDC flags AI-native vendors as taking the largest share of new spend. | Mid-market CRM is projected to grow 14.2% annually to $31B by 2029, with AI-native vendors capturing most new spend. | bad | Untraceable: drops the IDC source and date, so the forecast cannot be verified. |
| 6 | Campaign update: the "Revenue Clarity" webinar series generated 1,340 registrations and 412 MQLs in September, against a target of 350 MQLs. Cost per MQL was $86, down from $112 in August. Demand gen wants to approve a fourth session by Oct 15. | "Revenue Clarity" webinars beat the September target: 412 MQLs vs. 350 goal, from 1,340 registrations. Cost per MQL fell from $112 to $86. Decision needed: approve a fourth session by Oct 15. | good | Every figure matches, and the decision and deadline are kept. |
| 7 | Per Forrester's Q3 Wave on Revenue Intelligence, Clarion dropped from Leader to Strong Performer, citing a 22% drop in customer retention scores. Forrester noted Clarion's pricing remains the lowest in the category. | Forrester's Q3 Revenue Intelligence Wave promotes Clarion to Leader on strong retention scores and the lowest pricing in the category. | bad | Inconsistent: reverses the trend; Clarion was downgraded because retention fell, not promoted. |
| 8 | Pipeline update as of Oct 9: Q4 qualified pipeline is $48.2M, 2.6x coverage against the $18.5M bookings target. Enterprise accounts make up $31M of it. EMEA coverage is only 1.8x, and the regional VP is asking for 3 extra SDRs. | Q4 pipeline (as of Oct 9): $48.2M qualified, 2.6x coverage on the $18.5M target, $31M of it enterprise. Risk: EMEA at 1.8x; regional VP is requesting 3 more SDRs. | good | Accurate and complete; surfaces the EMEA risk and the request. |
| 9 | Per a Bloomberg report on Oct 8, Apex Analytics is in talks to acquire DataLoom for about $1.2B. The deal would give Apex DataLoom's 900 enterprise customers. Bloomberg says talks are early and may not close. | Bloomberg (Oct 8): Apex Analytics is in early talks to buy DataLoom for about $1.2B, which would add 900 enterprise customers. The deal may not close. | good | Faithful, keeps the source and the uncertainty caveat. |
| 10 | G2 Fall 2026 Grid Report: we were named a Leader in Conversation Intelligence for the third straight season, with a 92% user satisfaction score. Competitor Pulse moved into the Leader quadrant for the first time. | G2 Fall 2026: we are a Leader in Conversation Intelligence for the third straight season (92% satisfaction), and ranked #1 in market presence. Pulse entered the Leader quadrant for the first time with 88% satisfaction. | bad | Hallucination: the #1 market presence ranking and Pulse's 88% score are not in the email. |
| 11 | Per Pavilion's 2026 CRO Compensation Survey (n=640), median on-target earnings for enterprise AEs rose 6% to $310K. 58% of companies now include a multi-year deal accelerator. HR asks for feedback on our comp plan by Oct 24. | Pavilion 2026 survey (n=640): enterprise AE median OTE up 6% to $310K; 58% of companies offer multi-year deal accelerators. HR wants comp plan feedback by Oct 24. | good | All figures, the sample size and the deadline are kept accurately. |
| 12 | Competitive alert: Orbit Systems is cutting its Pro plan price by 30%, from $60 to $42 per user, starting Oct 20. Orbit also announced free migration services for customers switching from us, with 14 of our renewal accounts flagged as at risk. | Orbit Systems is cutting its Pro plan 30%, from $60 to $42 per user, starting Oct 20. | bad | Incomplete: omits the most decision-relevant facts, the free migration offer aimed at our customers and 14 at-risk renewals. |
| 13 | Per McKinsey's "State of B2B Buying 2026" (Sept), 71% of buyers now prefer remote or self-serve purchasing for deals under $50K. Separately, our own Q3 survey found 64% of our customers want a self-serve upgrade path. | Per McKinsey, 64% of customers want a self-serve upgrade path, and 71% of buyers prefer self-serve for deals under $50K. | bad | Untraceable: credits our internal 64% survey result to McKinsey, misattributing the source. |
| 14 | Win/loss note: we won the Meridian Health deal ($2.4M ACV) against Velocity AI. The buyer cited our HIPAA certification and 30-day implementation as deciding factors. Velocity offered a 25% discount, which did not change the outcome. | Won Meridian Health ($2.4M ACV) over Velocity AI. Deciding factors: HIPAA certification and 30-day implementation. Velocity's 25% discount did not sway the buyer. | good | Concise and accurate, keeps the win drivers and the competitive pricing signal. |
| 15 | Per Gartner's Oct 2 survey of 300 CFOs, 62% plan to cut SaaS vendor count in 2027, with sales tech named as the top consolidation target. Gartner recommends vendors prove ROI within 6 months. | Gartner has released some survey findings that are worth noting and could potentially be relevant in many ways to our overall go-to-market strategy going forward. Broadly speaking, the findings suggest that CFOs are thinking about their vendor relationships and may be looking at ways to streamline, which is something we will likely want to keep an eye on and possibly discuss at some point. | bad | Verbose: vague filler with none of the key facts (62%, 300 CFOs, sales tech as top target, 6-month ROI). |
| 16 | Analyst briefing recap: Per 451 Research, Northwind's AI agent product reached $40M ARR in its first 9 months. 451 expects Northwind to bundle it into Enterprise plans in H1 2027. The CI team will brief the product council on Oct 20. | 451 Research: Northwind's AI agent hit $40M ARR in 9 months, and 451 expects it to be bundled into Enterprise plans in H1 2027. CI briefs the product council on Oct 20. | good | Accurate attribution, figures and next step. |
| 17 | Per the Q3 NPS report (sent Oct 7), our NPS rose from 38 to 44, driven by detractor reductions in the mid-market segment. Enterprise NPS was flat at 41. | Q3 NPS rose from 38 to 44, outpacing the industry average of 36, driven by mid-market detractor reductions after the new support SLA. Enterprise flat at 41. | bad | Hallucination: invents an industry benchmark (36) and a cause (new support SLA). |
| 18 | LinkedIn ads update: the ABM campaign targeting 250 Fortune 500 accounts drove 38 opportunities worth $9.6M through Sept 30, with $210K spent. Marketing proposes expanding to 400 accounts with a $150K budget increase for Q4. | ABM campaign (250 F500 accounts, through Sept 30): 38 opportunities, $9.6M pipeline, on $210K spend. Proposal: expand to 400 accounts with $150K more for Q4. | good | All numbers and the proposed decision are traceable and kept. |
| 19 | Per Similarweb data for September, Helix CRM's website traffic grew 18% month over month, while ours declined 4%. Helix's paid search spend roughly doubled over the same period, per SEMrush. | Similarweb (Sept): our site traffic grew 18% MoM while Helix's declined 4%; Helix doubled paid search spend (SEMrush). | bad | Inconsistent: swaps the traffic figures, contradicting the email. |
| 20 | Per Crunchbase, Pulse closed a $120M Series D on Oct 1 led by Insight Partners, at a $1.4B valuation. Pulse says the funds will go to EMEA expansion and a 200-person sales hire. | Crunchbase: Pulse raised a $120M Series D (Oct 1, led by Insight Partners) at a $1.4B valuation. Funds go to EMEA expansion and 200 sales hires, which signals more competition in EMEA. | bad | Hallucination: facts are accurate and sourced, but "signals more competition in EMEA" is an inference the email does not state (relabeled from good by the PM). |

## Cold-start, the prompt you used to seed a starter dataset

Run in a fresh Claude session.

```
You are helping a product manager build a starter evaluation dataset for Ascend IQ, an AI assistant that gives VP-level strategists at Fortune 500 companies verified, citation-backed competitive intelligence. One Ascend IQ feature summarizes business emails and reports for executives.

Generate 20 example rows for evaluating Ascend IQ email summaries.

Each row has:
- Input email: a realistic 2–4 sentence business email an Ascend IQ user would receive (competitor pricing changes, G2 review digests, market or analyst reports, win/loss notes, campaign or pipeline updates). Include specific numbers, dates, and named sources (e.g. "per Gartner", "G2, Q3 reviews").
- Candidate summary: an executive summary of that email.
- Label: "good" or "bad" (first-pass).
- Reason: one line explaining the label.

Make 10 "good" rows (accurate, concise, every number and claim traceable to the email, key facts and action items kept) and 10 "bad" rows. Spread the bad rows across these failure types, about 2–3 each:
1. Hallucination: invents a number, cause, or claim not in the email.
2. Untraceable: states a number or claim but drops or misattributes its source.
3. Inconsistent / incomplete: omits the most decision-relevant fact, reverses a trend, or contradicts the email.
4. Verbose: wordy filler that buries the key facts.

Mix the good and bad rows (do not group them). Return a markdown table with columns: | # | Input email | Candidate summary | Label | Reason |. Do not use the pipe character inside cells.
```

## Your definition of good vs bad (golden-set criteria), the graded part, write your own

### Ascend IQ Summary Evaluation Criteria

These criteria define how a candidate summary is labeled **good** or **bad** in `AscendIQ-EmailSummaries-v1`. They protect the Ascend IQ user promise of verified, citation-backed intelligence for VP-level strategists, and each criterion cites the rows that illustrate it.

### Labeling rule

A summary is **good** only if it passes every criterion below. A failure on any single criterion makes it **bad**. Criteria 1, 2 and 6 are the most serious, because a summary that states something false or unsourced is worse than no summary at all. A VP acting on it would make a wrong decision, and one fabricated stat can end a $50k contract.

### 1. Factual accuracy

Every fact in the summary must match the email. That includes numbers, dates, names, rankings, and the direction of any change (up or down, promoted or downgraded, won or lost). The summary must not contradict anything the email says.

Fails when the summary reverses a trend, swaps figures between companies, or reports an outcome that did not happen. Examples: row 7 (a Forrester downgrade is reported as a promotion) and row 19 (our traffic decline and Helix's growth are swapped).

### 2. No invented content

The summary must not add causes, benchmarks, rankings, implications, or urgency that the email does not state. Reasonable compression is fine; new claims are not. **An inference is invented content, even a reasonable one:** if the email does not say it, the summary does not say it. Strategic conclusions are the VP's job, not the summary's.

Fails when the summary attributes a result to a cause the email does not give, adds a number or comparison, or draws its own conclusion. Examples: row 2 (blames a "September UI redesign" the email never mentions), row 10 (invents a #1 market-presence ranking and Pulse's 88% score), row 17 (invents an industry benchmark and a cause), and row 20 ("signals more competition in EMEA" is a plausible inference, but the email does not state it).

### 3. Completeness of decision-relevant information

The summary must keep every item a reader would need to act or decide. That means key figures and the scale of any change, dates and deadlines, risks or blockers, required actions and who owns them, and tradeoffs. Background, pleasantries, and sign-offs may be dropped.

Fails when the summary omits a risk, a number, a deadline, or a tradeoff, or becomes so vague that the reader cannot tell how big or urgent the issue is. Examples: row 12 (keeps Orbit's price cut but drops the free-migration offer aimed at our customers and the 14 at-risk renewals) and row 4 (drops the Q2 comparison, the Salesforce-integration driver, and the Oct 22 action).

### 4. Concision

The summary must be meaningfully shorter than the email and contain no filler. It should not narrate the email ("This email, which comes from…"), hedge without cause ("could potentially be relevant"), or restate the source sentence by sentence. A useful benchmark is that a good summary is roughly half the length of the email or less. A summary that is as long as or longer than the source fails, even if every fact is correct.

Fails on wordy, hedged, or narrated summaries. Examples: row 4 (narrated filler) and row 15 (vague filler with none of the Gartner figures). Note that both of these also drop key figures, so they fail criterion 3 as well. A summary that is verbose but otherwise complete still fails this criterion on its own.

### 5. Faithful tone

The summary should reflect the tone and urgency of the original. It should not make a routine note sound alarming or a serious risk sound minor, and it should not add promotional spin. Neutral, direct wording is the default.

Fails when the emotional register or urgency shifts. Example: Version B in the Q4 run below ("building strong momentum," "promising" experiments, and a 42% to 35% open-rate drop softened to "dipped").

### 6. Source traceable

Every number and claim must keep the source the email gives it (analyst firm, report, data provider, or internal team), and nothing may be credited to the wrong source. A correct number with no source is unverifiable, so it fails. This is the criterion that tests the "citation-backed" part of the user promise.

Fails when the summary drops the source or misattributes it. Examples: row 5 (IDC's forecast with no IDC and no date) and row 13 (our internal 64% survey result credited to McKinsey). Good examples: row 1 (CompetitorWatch, Oct 3) and row 9 (Bloomberg, Oct 8, with its caveat).

### Quick checklist

Use these six questions for a first-pass label. Answer "no" to any of them and the label is **bad**.

1. Are all facts, numbers, dates, rankings, and directions of change correct?
2. Is everything in the summary actually stated in the email, with no added causes, benchmarks, or inferences?
3. Are all key figures, deadlines, risks, actions, and tradeoffs included?
4. Is it clearly shorter than the email, with no filler, narration, or hedging?
5. Does it match the urgency and tone of the original, with no spin?
6. Does every number and claim keep its correct source?

### Known gaps in the current dataset

- Both "bad for verbosity" rows (4 and 15) also omit facts, so the set does not yet test a summary that is long but complete. Under these criteria, such a summary is **bad** because it fails criterion 4.
- No dataset row tests tone on its own (criterion 5); the only example is Version B from the Q4 run.
- UX Trust (consistency) cannot be tested by a single-output dataset. It needs the same question run twice and the key facts compared.
- Latency is not measured here; it needs response timings from the product, not labeled rows.

Adding rows for each of these would make every criterion independently testable.

## Screenshots, links or repo paths (optional if you followed the demo)

**Run:** Q4 Marketing Campaign Update email (M1 First Eval walkthrough), user message "Summarize this email for the CEO." Claude Opus 5.5 for both versions, in separate chats. Judge: ChatGPT (different model family).

### Version A output (Concise)

> Social media engagement rose 25% month-over-month, driven by the new content calendar and paid ad experiments.
> Newsletter open rate fell from 42% to 35%; new subject lines will be tested next week.
> Product video campaign delayed to November pending legal approval (assets ready); all tracking links due Friday.

48 words, 3 points.

### Version B output (Narrative)

> Subject: Q4 Marketing Campaign Update
>
> Our Q4 campaign is building strong momentum. Social media engagement climbed 25% month-over-month, driven by our new content calendar and promising paid ad experiments. The product video campaign's creative assets are complete and fully ready to launch. A few areas need attention: newsletter open rates dipped from 42% to 35%, and the product video is now slated for November pending legal approval. To keep progress on track, we'll test fresh, compelling subject lines next week to re-engage subscribers. Please add all campaign tracking links by Friday so we can measure results accurately. Thank you all for the great work, team!

About 100 words, no bullets.

### Judge verdict (ChatGPT)

| Version | Score | Judge reason |
|---|---|---|
| A, Concise | **1** | Concise and fully faithful to the original email. All key facts, percentages, dates, deadlines, and dependencies are accurately preserved without invention or omission. |
| B, Narrative | **0** | Not concise and introduces unsupported claims, including "Q4," "strong momentum," "promising paid ad experiments," and "fully ready to launch," which overstate or embellish the original email. |

### Result

**Winner: Version A.** It keeps every number and date traceable to the email in under 60 words. Version B keeps the facts but adds claims the email never makes ("strong momentum," "promising"), puts "fully ready to launch" among the wins while the launch is blocked by legal, softens the open-rate drop to "dipped," and addresses the team rather than the CEO. For Ascend IQ, that is a Hallucination Rate failure: confident language that cannot be traced to the source.

**Judge note:** the judge flagged "Q4" as unsupported, but the email's subject line is "Q4 Marketing Campaign Update", so that part of its reason is a false positive. The verdict still holds on the other claims, but it shows the judge needs calibration against human labels (Module 3) before it is trusted at scale.
