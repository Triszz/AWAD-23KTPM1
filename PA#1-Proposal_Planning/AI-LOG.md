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

## 2026-10-20 — choosing the idea

- **Tool:** Claude (chat).

- **Asked for:** candidate ideas for a standalone web app with an LLM feature where a wrong answer has a cost, small data, and an easy demo.

- **Kept:** the selection questions (a named user, a cost of being wrong we can name, whether we can build an eval set in about two weeks).

- **Rejected:** every idea the assistant proposed or recommended: order intake for sellers, scam check, expense-claim review, boarding-house maintenance triage, CV tailoring and others. We rejected them because of data size, platform integration and fit. The assistant also declined our request to invent a real interview, so Nam is labelled a persona.

- **By hand:** LabelCheck came from our own discussion, not from the assistant, and write who proposed it.

## 2026-10-21 — evaluating and revising the proposal

- **Tool:** Claude (chat).

- **Asked for:** an estimated rubric score for our LabelCheck draft, and what to fix.

- **Kept:** the plan table aligned to PA#1–PA#6 with the two of us as owners; PostgreSQL instead of SQLite; Docker and CI in the stack; the guardrails (lexicon scan alongside the LLM, quote check, "unreadable" result); keeping the allergy profile out of prompts; adding 100 text-only strings to the eval set.

- **Changed:** added the sentence that Nam is a persona, the PA#6 date, and đậu phộng; and we cut §2, §3 and §6 to fit two pages.

- **Rejected:** nothing.

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

- **Changed:** updated the evidence lines to match the final proposal.

- **By hand:** the "What I did not manage" section.
