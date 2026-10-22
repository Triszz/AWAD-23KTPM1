# LabelCheck: allergen checking for Vietnamese and English food labels

PA#1 — Proposal and planning · CSC13114

**Team:** 23127503 — Trần Thanh Trí · 23127376 — Hồ Gia Huy · **Repository:** https://github.com/Triszz/AWAD-23KTPM1

## 1. Product and users

**User (a persona, not an interview):** Nam, a second-year student living in a dormitory, allergic to milk and peanuts. He buys snacks, sauces and baking mixes at supermarkets and reads little English; imported products often have the full list only in English, with a shorter Vietnamese sticker.

**Problem:** Nam cannot tell whether a label contains milk or peanut, because the allergen hides under other names (đậu phộng, đạm sữa, whey, sodium caseinate, arachis oil) or under an E-number with no stated source (E322 lecithin may come from soy, sunflower or egg).

**Today:** He reads Vietnamese labels by eye and translates English words one by one with Google Translate, which does not say that caseinate is a milk protein; or he asks a friend, or skips the product.

**Product:** A web app. The user picks allergens once from 8 groups (kept in the browser), then pastes or photographs a label and gets a per-allergen result in Vietnamese with the label wording behind it.

## 2. The LLM feature and the cost of being wrong

**Feature:** Given a Vietnamese or English label (text, later a photo), the LLM finds every ingredient that is or may be derived from the 8 allergen groups (plain words, chemical names, E-numbers), quotes the label verbatim (translated into Vietnamese if English), and returns per group: "found"; "possibly present" (ambiguous E-numbers, "may contain traces" / "có thể chứa vết"); "not seen on the label"; or "unreadable, do not rely on this".

**Who is harmed, how much, reversible?** Nam, and anyone he shares food with. A missed allergen can cause a reaction from a rash to difficulty breathing and emergency care, and eaten food cannot be taken back. So it never says "safe", only "not seen on the label", shows the label beside each result, and warns that labels may omit cross-contamination.

**Guardrails:** The dangerous error, a missed allergen in correctly read text, is not caught by a "confirm the text" step. So (1) a deterministic lexicon scan runs beside the LLM and a hit from either is flagged (more false alarms, fewer misses); (2) every quoted phrase must appear in the label text; (3) unreadable input gets its own result.

**How we would know:** 40 team-photographed labels (half Vietnamese, half English; at least 15 with chemical names or E-numbers; each group in at least 3) plus at least 100 text-only strings, all hand-marked, including hidden names and blurry photos. We measure missed allergens (worst error) and false alarms per group; proposed threshold: at most 5% missed, at most 20% false alarms, every miss reviewed. Small groups (likely shellfish, tree nuts) are indicative only.

## 3. Scope

**In this semester:** Vietnamese and English labels; 8 groups (milk, egg, peanut, tree nuts, soy, wheat, fish, shellfish) with plain names, chemical names and E-numbers/INS; paste-text, then photo input with a confirm-the-text step; Vietnamese output with a verbatim quote; browser-stored profile; test set and evaluation script; Docker deployment with logs.

**Deliberately out:** Other allergens; judging additives as harmful in general; other label languages; medical advice; dish recognition; barcode scanning; user accounts; mobile app.

## 4. Plan and ownership

| Milestone | Date  | Trí                                                                   | Huy                                                                 |
| :-------- | :---- | :-------------------------------------------------------------------- | :------------------------------------------------------------------ |
| PA#1      | 22/10 | Repo, AI-LOG.md, §1–3                                                 | §4–6, self-assessment, start 40-entry reference list                |
| PA#2      | 12/11 | Harness: convention files, lint, tests, blocking CI                   | App skeleton, profile, paste-text UI, Playwright smoke test         |
| PA#3      | 26/11 | LLM pipeline: streaming, structured output, quote check, lexicon scan | Names and E-number knowledge base, retrieval, lookup_additive tool  |
| PA#4      | 3/12  | Evaluation script, results, thresholds                                | Guardrail checks, red-team response; photo upload with confirm step |
| PA#5      | 10/12 | Incident drill, post-mortem, final report                             | Docker deploy, logs, cost and latency                               |
| PA#6      | 17/12 | Explains Huy's half in the oral exam                                  | Explains Trí's half in the oral exam                                |

_Each member photographs about 20 labels between PA#2 and PA#3._

## 5. Risks

**1. The LLM misses hidden names, chemical names or E-numbers.** This is the app's whole value; finding only "milk" or "sữa" is no better than Google Translate. _This week:_ Huy writes 40 reference entries (about 5 per group, e.g. đậu phộng, lạc, đạm sữa, E-numbers) and tests whether the model recognises each and admits uncertainty on ambiguous codes; if it misses more than 1 in 10, the list goes into the prompt and the lexicon scan becomes the mandatory cross-check.

**2. The LLM misreads blurry or small photos.** One misread digit changes an E-number's meaning. _This week:_ photograph 10 labels in bad conditions (dim, tilted, curved); if fewer than 8 of 10 are read correctly, the confirm-text step becomes mandatory and unclear text returns "unreadable".

## 6. Technology choices

- **React + Express:** skills we have; the server holds the API key and runs the lexicon scan and quote check.
- **PostgreSQL + Docker + CI:** Postgres stores evaluation runs and the reference list (SQLite on Vercel does not persist); the course examines Docker and rollback; CI blocks merges from PA#2.
- **Claude Haiku 5.5 (Anthropic, vision):** one model reads photos, handles both languages and recognises hidden names. About 2,000 input and 200 output tokens per label; 40 labels × 10 runs ≈ 0.8M input and 0.08M output ≈ 3,000–5,000 VND at listed rates (text only; photo cost to be measured).
- **Privacy:** only the label goes to the provider; our code applies the browser-held profile to the results, so the profile is never in a prompt. Photos are resized in the browser.
