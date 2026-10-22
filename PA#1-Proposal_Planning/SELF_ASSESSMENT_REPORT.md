# Self-assessment — PA#1

Submitted by:

23127503 — Trần Thanh Trí

23127376 — Hồ Gia Huy

Total I claim: 85 / 100

**Repository:** https://github.com/Triszz/AWAD-23KTPM1

| Criterion                                       | Max | I claim | Evidence                                                                                                                                                                                                                                                   |
| :---------------------------------------------- | :-- | :------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Problem and users                               | 20  | 17      | Proposal §1: named persona (Nam, dormitory student, milk and peanut allergy), one-sentence problem, and "Today" paragraph on what he does instead (Google Translate, asking a friend, not buying). Labelled as a persona, not an interview.                |
| The LLM feature, and the cost of it being wrong | 25  | 22      | Proposal §2: one feature with four result states; "Who is harmed, how much" (rash to emergency care); "Reversible?" (no); "Guardrails" (lexicon scan, quote check, unreadable state); "How we would know" (40 labels \+ 100 text strings, pass threshold). |
| Scope: in and out                               | 15  | 13      | Proposal §3: explicit "In this semester" and "Deliberately out" lists. Consistent with §4 (Docker deploy, photo upload) and with §6 (profile kept in the browser, so "no user accounts" holds).                                                            |
| Plan and ownership                              | 20  | 16      | Proposal §4: PA\#1–PA\#6 table, work for both Trí and Huy at every milestone, dates 22/10, 12/11, 26/11, 3/12, 10/12, 17/12. it.                                                                                                                           |
| Risks                                           | 10  | 9       | Proposal §5: (1) missed hidden names or E-numbers; (2) misread photos. Each has a mitigation that starts this week and a numeric trigger (more than 1 in 10 missed; fewer than 8 of 10 read correctly).                                                    |
| Technology choices                              | 10  | 8       | Proposal §6: each choice has a one-line project-specific reason; model named (Claude Haiku 5.5) with a cost estimate (about 3,000–5,000 VND for the test runs); privacy and photo-size handling stated.                                                    |

## What I did not manage

- No real user interview. Nam is a persona built from the team's own observations.
- The 40-label set and the 40-entry reference list are only planned. Neither has been collected yet, and the lexicon scan and the model's recognition of hidden names have not been tested.
- The cost figure covers text only. I have not measured how many tokens a label photo adds, and I have not re-checked the Claude Haiku 5.5 price on the provider's pricing page.
- The pass threshold (at most 5% missed, at most 20% false alarms) is a proposal. It is not yet justified by data and may change after the first evaluation run.
