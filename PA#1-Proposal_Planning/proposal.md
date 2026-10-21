# **LabelCheck: allergen checking for Vietnamese and English food labels**

PA\#1 — Proposal and planning · CSC13114

**Team:**

23127503 — Trần Thanh Trí

23127376 — Hồ Gia Huy

**Repository:** https://github.com/Triszz/AWAD-23KTPM1

## **1\. Product and users**

**User:** Nam, a second-year student living in a dormitory, allergic to milk and peanuts. He reads little English and does not know food vocabulary. He buys local and imported snacks, sauces and baking mixes from supermarkets; local labels are in Vietnamese, imported ones are often in English only.

**Problem:** Nam cannot tell from a label whether it contains milk or peanut, because the allergen often hides under another name (lạc, đạm sữa, whey, casein, sodium caseinate, arachis oil) or under an E-number that does not state its source (E322 lecithin may come from soy, sunflower or egg).

**Today:** He reads Vietnamese labels by eye and translates English words one by one with Google Translate, which gives a word but not that caseinate is a milk protein; or he asks a friend; or he does not buy the product.

**Product:** A web app. The user picks their allergens once from 8 main groups (kept in the browser, no accounts), then pastes or photographs a Vietnamese or English label and gets, in Vietnamese, a per-allergen result with the exact label wording it is based on (and a Vietnamese translation when the label is in English).

## **2\. The LLM feature and the cost of being wrong**

**Feature:** Given a label (text, later a photo) in Vietnamese or English, the LLM finds every ingredient that is or may be derived from any of the 8 allergen groups, including plain words, chemical names and E-numbers, quotes the label wording verbatim (with a Vietnamese translation for English text), and returns one of four results per group: “found”; “possibly present” (ambiguous E-numbers and “may contain traces” / “có thể chứa vết” lines); “not seen on the label”; or “unreadable, do not rely on this”.

**Who is harmed, how much:** Nam, and anyone he shares food with. A missed milk or peanut ingredient can cause a reaction from a rash to difficulty breathing and emergency care. The cost is health first, money second.

**Reversible?** No. Food that has been eaten cannot be taken back. So the app never says “safe”, only “not seen on the label”; it shows the original label beside the result and says that labels may omit cross-contamination.

**Guardrails:** The dangerous error is a missed allergen in correctly read text, which a “confirm the text” step cannot catch. So (1) a deterministic lexicon scan runs alongside the LLM and a hit from either is flagged (more false alarms, fewer misses: the right trade); (2) every quoted phrase is checked to appear in the label text; (3) unreadable input gets its own result.

**How we would know:** 40 real labels photographed by the team (about half Vietnamese, half English; at least 15 with chemical names or E-numbers; each group in at least 3 labels) plus at least 100 text-only ingredient strings, all with a hand-marked correct answer. We measure missed allergens (worst error) and false alarms (nuisance), per group, on hard cases too: hidden names in both languages, ambiguous E-numbers, “may contain” lines, blurry or curved photos. Proposed pass threshold: at most 5% of true allergen mentions missed and at most 20% false alarms; every miss is reviewed. For groups with few examples (likely shellfish and tree nuts) figures are indicative only.

## **3\. Scope**

**In this semester:** Vietnamese and English labels; 8 groups (milk, egg, peanut, tree nuts, soy, wheat, fish, shellfish) with the same support for all; plain names, chemical names and E-numbers/INS linked to those groups; paste-text input, then photo input with a step where the user confirms the text; Vietnamese output with a verbatim quote; the allergen profile stored in the browser; test set and evaluation script; a Docker deployment with logs.

**Deliberately out:** Allergens outside the 8 groups; chemical names or codes unrelated to them; judging whether an additive is harmful in general; other label languages (Korean, Japanese, Thai, Chinese); medical advice; recognising dishes from photos; barcode scanning; user accounts (the profile never leaves the browser); mobile app.

## **4\. Plan and ownership**

| Milestone | Date  | Trí                                                                          | Huy                                                                             |
| :-------- | :---- | :--------------------------------------------------------------------------- | :------------------------------------------------------------------------------ |
| PA\#1     | 22/10 | Repo, AI-LOG.md, sections 1–3 of this proposal                               | Sections 4–6, self-assessment, start the 40-entry reference list                |
| PA\#2     | 12/11 | Harness: convention files, lint, tests, CI that blocks merges                | App skeleton, allergen profile, paste-text UI, Playwright smoke test            |
| PA\#3     | 26/11 | LLM pipeline: streaming, structured output, quote check, lexicon cross-check | Knowledge base of allergen names and E-numbers, retrieval, lookup_additive tool |
| PA\#4     | 3/12  | Evaluation script, results, thresholds                                       | Guardrails, photo upload with confirm-text step, red-team response              |
| PA\#5     | 10/12 | Incident drill and post-mortem, final report                                 | Docker deploy, logs, cost and latency                                           |
| PA\#6     | TBD   | Explains Huy’s half in the oral exam                                         | Explains Trí’s half in the oral exam                                            |

_Each member photographs about 20 labels between PA\#2 and PA\#3. The PA\#6 date is set by the course._

## **5\. Risks**

**1\. The LLM misses hidden names, chemical names or E-numbers.** This is the whole value of the app: if it only finds “milk” or “sữa”, it is no better than Google Translate. _This week:_ Huy writes 40 entries (about 5 per group: plain names, chemical names, E-numbers) and tests whether the model recognises each and admits uncertainty on ambiguous codes; if it misses more than 1 in 10, the list goes into the prompt and the lexicon scan becomes the mandatory cross-check.

**2\. The LLM misreads blurry or small photos.** One misread digit changes the meaning of an E-number. _This week:_ photograph 10 labels in bad conditions (dim light, tilted, curved) and count correct reads; if fewer than 8 of 10, the confirm-text step becomes mandatory and unclear text returns “unreadable”.

## **6\. Technology choices**

- **React \+ Express:** skills we already have; the server keeps the API key and runs the lexicon scan and quote check.

- **PostgreSQL:** stores evaluation runs and the reference list; serverless file systems (SQLite on Vercel) do not persist.

- **Docker \+ CI:** the course examines Docker, release and rollback, and CI blocks merges from PA\#2.

- **Claude Haiku 5.5 (Anthropic, vision):** one model reads photos, handles both languages, translates and recognises hidden names, which a translation tool cannot. About 2,000 input and 200 output tokens per label; 40 labels × 10 runs ≈ 0.8M input and 0.08M output tokens ≈ 3,000–5,000 VND at listed rates (text only; photos and the 100 extra strings add some, to be measured).

- **Privacy:** an allergy profile is health data, so only the label text or photo is sent to the provider. The model returns results for all 8 groups and our code filters them by the browser-held profile; the profile is never in a prompt.

- **Photo size:** photos are resized in the browser before upload to stay inside request limits.
