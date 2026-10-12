# First LLM-as-a-Judge Eval, Module 1

## Version A, Concise, system prompt used

You are an executive briefing assistant.
Summarize in exactly 3 bullet points under 60 words. No preamble, no extra text.

## Version B, Narrative, system prompt used

You are a PR communications assistant.
Write a 100-word narrative summary highlighting wins first, then risks and next steps. Keep a positive tone. No bullets.

## Eval setup, dataset name + judge model/family

# Email Summary Evaluation Set

| # | Input email | Candidate summary | Label | Reason |
|---|---|---|---|---|
| 1 | Q3 revenue hit $4.2M, 8% above forecast. Churn rose to 3.1%. Finance will present a retention plan on Oct 20. | Q3 revenue $4.2M (8% over forecast); churn up to 3.1%; retention plan due Oct 20. | good | Captures every key figure and the next step concisely. |
| 2 | Server migration is scheduled for Saturday 2–6 AM. Expect brief downtime for the customer portal. Contact IT with concerns. | Server migration Sunday; no downtime expected. | bad | Wrong day and contradicts the stated downtime. |
| 3 | We've hired two senior engineers starting Nov 3. Onboarding buddies are needed; reply by Wednesday. | Two senior engineers start Nov 3; volunteer as onboarding buddy by Wednesday. | good | Faithful, brief, and keeps the action item and deadline. |
| 4 | The vendor contract renewal is due Dec 1. Pricing increases 12%. Legal is reviewing terms. | In this email, the sender wanted to let everyone know about something important regarding the vendor contract, which is coming up for renewal. They mentioned some kind of change in pricing and that a team is looking at it. It seems like a significant update people should be aware of. | bad | Verbose filler that drops the Dec 1 date and the 12% increase. |
| 5 | Office closed Monday for the holiday. Payroll will process Friday instead of Monday. | Office closed Monday; payroll moves to Friday. | good | Complete and accurate in minimal words. |
| 6 | Customer NPS rose from 41 to 47. Top complaint remains slow support response. Support is adding weekend coverage in November. | NPS rose to 47 thanks to faster support response times. | bad | Invents a cause that contradicts the email's stated top complaint. |
| 7 | 2027 budget requests are due Oct 31. Use the new template on the finance wiki. Late submissions won't be considered. | 2027 budget requests due Oct 31 using the new wiki template; late submissions rejected. | good | Keeps deadline, instruction, and consequence. |
| 8 | The beta launch slipped one week to Oct 28 due to a payment bug. QA expects a fix by Friday. | Beta launched successfully on Oct 21. | bad | Fabricates a launch that the email says was delayed. |
| 9 | Complete mandatory security training by Oct 25. Completion is tracked; managers will follow up with stragglers. | Mandatory security training due Oct 25; completion tracked and managers will follow up. | good | Accurate and concise with the deadline intact. |
| 10 | Sales closed 14 deals this month worth $860K. Q4 pipeline is $3.4M. Two enterprise deals are at risk due to procurement delays. | Sales closed 14 deals worth $860K. | bad | Omits the pipeline figure and the at-risk deals, the most decision-relevant item. |
| 11 | The Austin office lease expires in June. Leadership is evaluating a smaller space or a hybrid model. Employee survey closes Friday. | Austin lease ends in June; leadership weighing smaller space vs. hybrid; survey closes Friday. | good | Covers the situation, options, and action item faithfully. |
| 12 | Thanks everyone for a great offsite! Photos are in the shared drive. Feedback survey closes Tuesday. | URGENT: Mandatory survey must be completed immediately or offsite photos will be deleted. | bad | Invents urgency and a consequence; distorts a friendly tone. |
| 13 | Marketing spend was $210K vs. a $250K budget. Underspend is due to a paused LinkedIn campaign. Funds will be reallocated to Q4 events. | Marketing spent $210K of $250K (LinkedIn campaign paused); surplus moving to Q4 events. | good | Preserves numbers, cause, and plan. |
| 14 | Shipping delays from our Shenzhen supplier will push 30% of holiday inventory to mid-December. Ops is exploring air freight at added cost. | Holiday inventory delayed; ops exploring options. | bad | Too vague: drops the 30% scope, mid-December timing, and cost implication. |
| 15 | The board meeting moved from Nov 12 to Nov 14. Deck drafts are due to the CEO's office by Nov 7. | Board meeting moved to Nov 14; deck drafts due to CEO's office Nov 7. | good | Both dates correct and the action is clear. |
| 16 | Engineering velocity dropped 15% in September due to on-call load. We're proposing a dedicated SRE hire. | Engineering velocity rose 15% in September; SRE hire proposed. | bad | Reverses the direction of the key metric. |
| 17 | Our SOC 2 Type II audit passed with no exceptions. The report is available to customers on request. | SOC 2 Type II passed with no exceptions; report available to customers on request. | good | Accurate and complete. |
| 18 | Pricing page A/B test: Variant B increased signups 9% but lowered paid conversion 4%. We recommend running two more weeks before deciding. | Variant B won with 9% more signups; roll it out. | bad | Omits the conversion tradeoff and contradicts the recommendation to keep testing. |
| 19 | Jamie is on parental leave Nov 1 to Jan 31. Alex will cover vendor approvals; Sam will cover hiring. | Jamie out Nov 1–Jan 31; Alex covers vendor approvals, Sam covers hiring. | good | Dates and coverage assignments all correct. |
| 20 | Support ticket volume is up 40% after the v5 release. Most tickets relate to the new login flow. A hotfix ships Thursday. | Hello, this email is about support tickets. The support team has noticed there have been more tickets than usual recently, and they believe it might possibly be connected to the most recent release, version five, which changed how users log in. A fix is coming later this week, likely Thursday. | bad | Wordy and hedged, and it drops the 40% figure. |

## Cold-start, the prompt you used to seed a starter dataset

Generate 20 example rows for evaluating email-summary quality.
Each row: an input email + a candidate summary + a first-pass label ("good" or "bad") + a one-line reason.
Make roughly half concise/faithful ("good") and half verbose or inaccurate ("bad").
Return it as a markdown table.

## Your definition of good vs bad (golden-set criteria), the graded part, write your own

# Email Summary Evaluation Criteria

These criteria define how a candidate summary is labeled **good** or **bad** in the email summary evaluation set. They are derived from the reasons given for the 20 labeled examples, and each criterion cites the rows that illustrate it.

## Labeling rule

A summary is **good** only if it passes every criterion below. A failure on any single criterion makes it **bad**. Criteria 1 and 2 are the most serious, because a summary that states something false is worse than no summary at all. A reader acting on it would make a wrong decision.

## 1. Factual accuracy

Every fact in the summary must match the email. That includes numbers, dates, days of the week, names, and the direction of any change (up or down, delayed or launched, passed or failed). The summary must not contradict anything the email says.

Fails when the summary gets a day wrong, reverses a trend, or reports an outcome that did not happen. Examples: row 2 (Saturday becomes Sunday, and "brief downtime" becomes "no downtime"), row 8 (a delayed launch is reported as a successful one), and row 16 (velocity "dropped" becomes "rose").

## 2. No invented content

The summary must not add causes, consequences, recommendations, or urgency that the email does not state. Reasonable compression is fine; new claims are not. Interpretive framing (such as a "bottom line") is acceptable only when the email directly supports it.

Fails when the summary attributes a result to a cause the email does not give, adds threats or deadlines, or reverses the sender's recommendation. Examples: row 6 (credits faster support for an NPS gain when slow support is the top complaint), row 12 (turns a friendly thank-you into an "URGENT" mandate with an invented consequence), and row 18 ("roll it out" when the email recommends two more weeks of testing).

## 3. Completeness of decision-relevant information

The summary must keep every item a reader would need to act or decide. That means key figures and the scale of any change, dates and deadlines, risks or blockers, required actions and who owns them, and tradeoffs. Background, pleasantries, and sign-offs may be dropped.

Fails when the summary omits a risk, a number, a deadline, or a tradeoff, or becomes so vague that the reader cannot tell how big or urgent the issue is. Examples: row 10 (omits the two at-risk deals), row 14 ("inventory delayed" without the 30% scope, mid-December timing, or added cost), and row 18 (omits the drop in paid conversion).

## 4. Concision

The summary must be meaningfully shorter than the email and contain no filler. It should not narrate the email ("In this email, the sender…"), hedge without cause ("might possibly"), or restate the source sentence by sentence. A useful benchmark is that a good summary is roughly half the length of the email or less. A summary that is as long as or longer than the source fails, even if every fact is correct.

Fails on wordy, hedged, or narrated summaries. Examples: row 4 (vague filler about "something important") and row 20 (wordy and hedged). Note that both of these also drop key figures, so they fail criterion 3 as well. A summary that is verbose but otherwise complete still fails this criterion on its own.

## 5. Faithful tone

The summary should reflect the tone and urgency of the original. It should not make a routine note sound alarming or a serious risk sound minor. Neutral, direct wording is the default.

Fails when the emotional register or urgency shifts. Example: row 12 (a friendly reminder rewritten as an urgent threat).

## Quick checklist

Use these five questions for a first-pass label. Answer "no" to any of them and the label is **bad**.

1. Are all facts, numbers, dates, and directions of change correct?
2. Is everything in the summary actually stated in, or directly supported by, the email?
3. Are all key figures, deadlines, risks, actions, and tradeoffs included?
4. Is it clearly shorter than the email, with no filler, narration, or hedging?
5. Does it match the urgency and tone of the original?

## Known gap in the current dataset

Every "bad for verbosity" example (rows 4 and 20) also omits facts, so the set does not yet test a summary that is long but complete. Under these criteria, such a summary is **bad** because it fails criterion 4. Likewise, the set has few summaries that are short and accurate but leave out a single key item. Adding a few rows of each type would make criteria 3 and 4 independently testable.

## Screenshots, links or repo paths (optional if you followed the demo)

_…_

