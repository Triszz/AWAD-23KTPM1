# LabelCheck: allergen checking for Vietnamese and English food labels

PA#1 — Proposal and planning · CSC13114

**Team:** 23127503 — Trần Thanh Trí · 23127376 — Hồ Gia Huy

**Repository:** https://github.com/Triszz/AWAD-23KTPM1

## 1. Product and users

**User (a persona, not an interview):** Nam, a second-year student living in a dormitory, allergic to milk and peanuts. He reads little English and does not know food vocabulary. He buys local and imported snacks, sauces and baking mixes from supermarkets; local labels are in Vietnamese, imported ones are often in English only.

**Problem:** Nam cannot tell from a label whether it contains milk or peanut, because the allergen often hides under another name (đậu phộng, đạm sữa, whey, casein, sodium caseinate, arachis oil) or under an E-number that does not state its source (E322 lecithin may come from soy, sunflower or egg).

**Today:** He reads Vietnamese labels by eye and translates English words one by one with Google Translate, which gives a word but not that caseinate is a milk protein; or he asks a friend; or he does not buy the product.

**Product:** A web app. The user picks their allergens once from 8 main groups (kept in the browser, no accounts), then pastes or photographs a Vietnamese or English label and gets, in Vietnamese, a per-allergen result with the exact label wording it is based on (and a Vietnamese translation when the label is in English).

## 2. The LLM feature and the cost of being wrong

**Feature:** Given a label (text, later a photo) in Vietnamese or English, the LLM finds every ingredient that is or may be derived from any of the 8 allergen groups (plain words, chemical names, E-numbers), quotes the label wording verbatim (with a Vietnamese translation for English text), and returns one of four results per group: "found"; "possibly present" (ambiguous E-numbers, "may contain traces" / "có thể chứa vết"); "not seen on the label"; or "unreadable, do not rely on this".

**Who is harmed, how much, reversible?** Nam, and anyone he shares food with. A missed milk or peanut ingredient can cause a reaction from a rash to difficulty breathing and emergency care: health first, money second. Food that has been eaten cannot be taken back, so the app never says "safe", only "not seen on the label"; it shows the original label beside the result and says that labels may omit cross-contamination.

**Guardrails:** The dangerous error is a missed allergen in correctly read text, which a "confirm the text" step cannot catch. So (1) a deterministic lexicon scan runs alongside the LLM and a hit from either is flagged (more false alarms, fewer misses); (2) every quoted phrase must appear in the label text; (3) unreadable input gets its own result.

**How we would know:** 40 labels photographed by the team (half Vietnamese, half English; at least 15 with chemical names or E-numbers; each group in at least 3) plus at least 100 text-only ingredient strings, all with a hand-marked correct answer, including hard cases (hidden names, ambiguous E-numbers, blurry photos). We measure missed allergens (worst error) and false alarms per group. Proposed threshold: at most 5% of true allergen mentions missed and at most 20% false alarms; every miss is reviewed. Groups with few examples (likely shellfish, tree nuts) are indicative only.

## 3. Scope

**In this semester:** Vietnamese and English labels; 8 groups (milk, egg, peanut, tree nuts, soy, wheat, fish, shellfish) with equal support; plain names, chemical names and E-numbers/INS linked to them; paste-text input, then photo input with a confirm-the-text step; Vietnamese output with a verbatim quote; profile stored in the browser; test set and evaluation script; Docker deployment with logs.

**Deliberately out:** Allergens outside the 8 groups; judging whether an additive is harmful in general; other label languages; medical advice; recognising dishes from photos; barcode scanning; user accounts; mobile app.

## 4. Plan and ownership

| Milestone | Date  | Trí                                                                          | Huy                                                                         |
| :-------- | :---- | :--------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| PA#1      | 22/10 | Repo, AI-LOG.md, sections 1–3                                                | Sections 4–6, self-assessment, start the 40-entry reference list            |
| PA#2      | 12/11 | Harness: convention files, lint, tests, CI that blocks merges                | App skeleton, allergen profile, paste-text UI, Playwright smoke test        |
| PA#3      | 26/11 | LLM pipeline: streaming, structured output, quote check, lexicon cross-check | Knowledge base of names and E-numbers, retrieval, lookup_additive tool      |
| PA#4      | 3/12  | Evaluation script, results, thresholds                                       | Guardrail checks and red-team response; photo upload with confirm-text step |
| PA#5      | 10/12 | Incident drill and post-mortem, final report                                 | Docker deploy, logs, cost and latency                                       |
| PA#6      | 17/12 | Explains Huy's half in the oral exam                                         | Explains Trí's half in the oral exam                                        |

_Each member photographs about 20 labels between PA#2 and PA#3._

## 5. Risks

**1. The LLM misses hidden names, chemical names or E-numbers.** This is the whole value of the app: if it only finds "milk" or "sữa", it is no better than Google Translate. _This week:_ Huy writes 40 reference entries (about 5 per group, e.g. đậu phộng, lạc, đạm sữa, E-numbers) and tests whether the model recognises each and admits uncertainty on ambiguous codes; if it misses more than 1 in 10, the list goes into the prompt and the lexicon scan becomes the mandatory cross-check.

**2. The LLM misreads blurry or small photos.** One misread digit changes the meaning of an E-number. _This week:_ photograph 10 labels in bad conditions (dim light, tilted, curved) and count correct reads; if fewer than 8 of 10, the confirm-text step becomes mandatory and unclear text returns "unreadable".

## 6. Technology choices

- **React + Express:** skills we already have; the server keeps the API key and runs the lexicon scan and quote check.
- **PostgreSQL + Docker + CI:** Postgres stores evaluation runs and the reference list (SQLite on Vercel does not persist); the course examines Docker, release and rollback; CI blocks merges from PA#2.
- **Claude Haiku 5.5 (Anthropic, vision):** one model reads photos, handles both languages, translates and recognises hidden names, which a translation tool cannot. About 2,000 input and 200 output tokens per label; 40 labels × 10 runs ≈ 0.8M input and 0.08M output tokens ≈ 3,000–5,000 VND at listed rates (text only; photos and the 100 extra strings add some, to be measured).
- **Privacy:** only the label text or photo is sent to the provider; the model returns results for all 8 groups and our code filters them by the browser-held profile, so the allergy profile is never in a prompt. Photos are resized in the browser before upload.
