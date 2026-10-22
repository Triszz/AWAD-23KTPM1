# AI-LOG.md — PA#1 (CSC13114)

Team: 23127503 — Trần Thanh Trí · 23127376 — Hồ Gia Huy

Project: LabelCheck

## Data that leaves our system

- **PA#1:** the product does not exist yet and makes no LLM calls. In chats with the assistant we pasted only the course rubric, the course syllabus and our own proposal and self-assessment drafts (these contain our names and student IDs). No API keys, no real user data, no classmate's work.
- **From PA#3 (planned):** only the text or photo of a food label goes to the Anthropic API. A label is product packaging, not personal data. The user's allergy profile is health data: it stays in the browser, is never put in a prompt, and our code applies it to the model's results for all 8 groups.

## 2026-10-19 — reading the rubric and syllabus, planning PA#1

- **Tool:** Claude (chat).

- **Asked for:** an analysis of the PA#1 rubric and the syllabus, and how the six project checkpoints map to PA#1–PA#6.

- **Kept:** the breakdown of what each criterion rewards, and the mapping of the six checkpoints to PA#1–PA#6.

- **Changed:** the assistant guessed milestone dates from the topic order. We replaced them with the real dates from Classroom (PA#2 12/11, PA#3 26/11, PA#4 3/12, PA#5 10/12, PA#6 17/12).

- **Rejected:** the guessed dates.

- **By hand:** reading the Classroom dates and agreeing the split of work.

## 2026-10-20 — choosing a product idea

- **Tool:** Claude (chat, claude.ai).

- **Asked for:** product ideas that score well on the rubric, mainly criteria 1 (named user) and 2 (an LLM feature with a nameable cost of being wrong).

- **Kept:** the method of choosing an idea by those two criteria first, and the habit of checking each idea's data source before committing.

- **Rejected:** (1) ReplyDesk, reply drafts for a small online shop: The team felt that simply copying and pasting customer questions into an LLM, then reviewing the output before sending it back to the customer, was not a good approach. (2) A rental-contract reader for students: real contracts are hard to collect, so evaluation data would have been weak. (3) A university-regulations question tool: only mentioned as an option, never developed.

- **By hand:** the decision to drop each idea and to ask for another.

## 2026-10-20 — shaping LabelCheck

- **Tool:** Claude (chat).

- **Asked for:** an allergen-checking idea whose data we can collect ourselves (photos of labels from a supermarket).

- **Kept:** the single LLM feature (read a label, list ingredients that may come from the user's allergens, quote the label verbatim), and the three results "found / possibly present / not seen on the label". The app never says "safe".

- **Changed:** Claude's first version was a generic checker for Vietnamese labels, for a user allergic to peanuts and shellfish. We changed it in four steps: (a) the user is a Vietnamese person who cannot read English and buys imported food, so the labels are in English; (b) scope covers both Vietnamese and English labels; (c) scope also covers chemical names and additive codes (E-numbers), which is where allergens hide; (d) we corrected Claude's reading that the product only handles egg and peanut: those were examples, and each user picks their own allergens (e.g. milk and peanut).

- **Rejected:** Claude's first version without the English-label context, because imported products with English-only ingredient lists are where Nam's problem is biggest, and they also appear often in stores.

- **By hand:** the problem insight (language barrier plus hidden names) and all four scope decisions above. Claude wrote the sentences; the direction was ours.

## 2026-10-21 — writing the two-page proposal and Word files

- **Tool:** Claude (chat, file creation).

- **Asked for:** a two-page proposal in English covering the six sections the brief lists, as a Word file, rewritten each time our scope changed (several versions).

- **Kept:** the section structure, the cost-of-being-wrong paragraph (health first, not reversible once eaten), the evaluation idea (about 40 labelled labels, count missed allergens and false alarms), the scope out-list.

- **Changed:** scope, user and risks after each change in the entry above. We also asked why the second risk had changed; Claude had swapped the "blurry photos" risk for "hidden names" without saying so, and we kept both.

- **Rejected:** its earlier ReplyDesk text in full.

- **By hand:** The members check together.

## 2026-10-21 — plan, ownership and technology

- **Tool:** Claude (chat).

- **Asked for:** a six-checkpoint plan and one-line technology justifications.

- **Kept:** the checkpoint work items as a starting point; Next.js, SQLite, a free hosting tier.

- **Changed:** this first plan was later replaced; real dates, real owners and the new stack are in the "applying the review" entry below.

- **By hand:** owners and dates; the model provider and its price. Claude's token estimate (about 2,000 input and 200 output tokens per label) is a guess we have not yet checked.

## 2026-10-22 — this log (first pass)

- **Tool:** Claude (chat).

- **Asked for:** a first draft of this file from the chat history, using the course template.

- **Kept:** the entries above after checking each against what we actually did.

- **By hand:** We can explain every entry because the decisions in them were ours.

## 2026-10-22 — scoring the early draft against the rubric, Vietnamese summary

- **Tool:** Claude (chat).

- **Asked for:** our idea summarised in Vietnamese, and a score of the proposal against the PA#1 rubric.

- **Kept:** the list of things to fix: empty placeholders, SQLite not persisting on Vercel, and checkpoint 1 being too heavy for one owner.

- **Changed:** nothing in the document at this step. Claude's estimate (about 83 as written, about 91 once filled in) was later shown to be too optimistic about the plan; see the "applying the review" entry.

- **Rejected:** Claude’s early estimate (about 83 as written, about 91 once filled in) as our number, because the later review showed the plan did not match the course milestones.

- **By hand:** no writing. The scoring was only an input to our decisions.

## 2026-10-22 — scope: chemical names and the 8 groups

- **Tool:** Claude (chat, Word file editing).

- **Asked for:** whether chemical names are inside the scope; later, a fix so the plan matches the scope; and what the "8 allergen groups" are and why 8.

- **Kept:** Claude's answer that chemical names are in scope when they trace to one of the 8 groups; the new out-of-scope line "chemical names or codes unrelated to the 8 groups"; its explanation of the 8 groups (the main regulated allergen groups; the US now lists 9 with sesame, the EU 14), which we kept at 8 to limit the semester's work.

- **Changed:** Claude read our plan as deep support only for milk and peanut, because CP1 and risk 1 named only those two. We corrected it: all 8 groups get the same level of support. Claude then edited CP1, risk 1 and the test-set rule for all 8 groups (first about 60 reference entries, at least 5 labels per group).

- **Rejected:** the milk-and-peanut-only reading, and the alternative Claude offered of keeping 8 groups in the UI but evaluating only two.

- **By hand:** the decision that all 8 groups are supported equally.

## 2026-10-22 — cutting the workload

- **Tool:** Claude (chat, Word file editing).

- **Asked for:** lighter numbers, because more labels per group made checkpoint 1 too hard; then one sentence saying that figures for rare groups are only indicative.

- **Kept:** the lighter numbers (about 40 reference entries, about 5 per group; at least 3 labels per group; still 40 labels in total); then a sentence in "How we would know" naming shellfish and tree nuts as the groups likely to have few examples.

- **Changed:** from the first 60 entries and 5 labels per group, which we judged too much.

- **Rejected:** the heavier numbers (60 entries, 5 labels per group). The idea that shellfish and tree nuts are the hardest to find on Vietnamese shelves is Claude's guess; we have not checked it.

- **By hand:** the decision to cut the workload and to say the limit openly in the proposal.

## 2026-10-22 — applying the review and rebuilding the proposal

- **Tool:** Claude (chat, file creation).

- **Asked for:** complete the proposal using a written review we pasted in and the rubric.

- **Kept:** the plan on PA#1–PA#6 with Trí and Huy as owners; React + Express + PostgreSQL + Docker with CI; Claude Haiku 5.5; the privacy fix (only the label goes to the model, our code applies the profile); the guardrails (lexicon scan, quote check, "unreadable" result); 100+ text-only test strings; the persona wording.

- **Changed:** the stack changed from our first version (Next.js, SQLite) to React + Express + PostgreSQL + Docker with CI, because SQLite on Vercel does not persist. The plan changed from product tasks to the course milestones PA#1–PA#6, with Trí and Huy as owners. The pass threshold (5% missed, 20% false alarms) was proposed by Claude, not taken from our data.

- **Rejected:** Next.js and SQLite on Vercel; Claude's first score estimate, which had missed that our checkpoints did not match the course milestones.

- **By hand:** the real dates from Classroom, student IDs, repository link, and our read-through.

## 2026-10-22 — this log (second pass)

- **Tool:** Claude (chat).

- **Asked for:** add this session's work to the log from the chat history, using the course template.

- **Kept:** all earlier entries as they were, except that the team line is now Trí and Huy and one placeholder in the "plan, ownership and technology" entry points to the "applying the review" entry.

- **Changed:** the data section at the top of this file. It said we send the allergen group names to the model; the final proposal sends only the label and filters in our code.

- **Rejected:** nothing.

- **By hand:** We can explain every entry because the decisions in them were ours.

## 2026-10-22 — shortening to two pages and filling this log

- **Tool:** Claude (chat).

- **Asked for:** a shorter proposal (cut "How we would know", the scope lists and the technology bullets) and a draft of this log from the template.

- **Kept:** the shortened proposal. The assistant also changed the PA#4 row for Huy to "guardrail checks and red-team response", so that Trí builds the guardrails in PA#3 and Huy checks them in PA#4.

- **Changed:** nothing.

- **By hand:** our own read-through of both files. We will not submit anything we cannot explain at the PA#6 oral exam.

## 2026-10-22 — self-assessment

- **Tool:** Claude (chat).

- **Asked for:** a check of whether claiming 85 is justified.

- **Kept:** the total of 85, which the assistant judged to be about right and slightly conservative.

- **Changed:** the split of marks (17, 22, 13, 16, 9, 8) came from the assistant’s draft; we checked each mark against the rubric and updated the evidence lines to match the final proposal.

- **By hand:** the "What I did not manage" section.

## 2026-10-22 — second cut so §6 fits on page 2

- **Tool:** Claude (chat).

- **Asked for:** a further cut of the proposal, because the PDF still ran onto page 3 (only §6).

- **Kept:** the shorter text of §1–§6. The plan, dates, owners, risk triggers and pass thresholds are unchanged. The assistant also removed a stray "it." from the Plan evidence line and updated the evidence lines to the merged "Who is harmed, how much, reversible?" heading.

- **Changed:** nothing.

- **By hand:** our own read-through of the final text.
