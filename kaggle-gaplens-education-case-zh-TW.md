
---

## （English version）

```markdown
# Series · Part 4: Kaggle Education Case – GapLens Learning Loss Scanner  
An AI Orchestrator case study with Gemini 3 Pro + Google AI Studio

[English](./kaggle-gaplens-education-case.md) | [繁體中文 / Traditional Chinese](./kaggle-gaplens-education-case-zh-TW.md)

---

## 0. Where this note sits in the series

This is Part 4 of my **AI Orchestrator / R0 series**.

The first three parts are:

1. **Part 1 – Arin’s R0 / AI Orchestrator Methodology**  
   - SSOT, Core Kernel + Modules, and the L1–L10 workflow layers.

2. **Part 2 – AI Orchestrator Communication Map (L1–L10)**  
   - Uses L1–L10 as a ladder for AI-usage maturity and provides a communication playbook.

3. **Part 3 – AI Orchestrator MVP→Launch Workflow Guide**  
   - An end-to-end SOP from topic selection to MVP, demo, launch and iteration.

You can think of them as:

- Architecture & rules,  
- People & communication map,  
- Execution workflow.

**This note (Part 4)** is a concrete case:

> How I used Gemini 3 Pro + AI Studio + Kaggle  
> to build a small but explainable **education MVP** under a 1-week hackathon constraint.

---

## 1. Case overview: what is GapLens?

### 1.1 Hackathon context

- Competition: **Vibe Code with Gemini 3 Pro** (Kaggle hackathon)  
- Track: **Education & Learning**  
- Requirements:
  - Build the app inside **Google AI Studio – Build**;
  - Make real use of **Gemini 3 Pro’s reasoning + native multimodality**;
  - Submit a Kaggle writeup, a public AI Studio app link, and a <2-min demo video.

### 1.2 Product idea in one sentence

**GapLens – Learning Loss Scanner** is:

> A teacher-first “learning loss scanner” that turns uploaded exams  
> into a map of conceptual / procedural / careless gaps,  
> then generates differentiated practice and homework in one click.

It is not meant to be a full LMS.  
The focus is to automate a very specific teacher workflow:

1. Look at graded papers;  
2. Guess where the student’s understanding actually broke;  
3. Design extra practice or homework.

GapLens asks Gemini 3 Pro to:

- classify each wrong answer into **Conceptual / Procedural / Careless**;  
- tag underlying skills;  
- summarize per-student gaps and synthesize:
  - a **Student Summary** and **Personal Homework Pack**;
  - a **Class View** with shared gaps and suggested small groups.

---

## 2. R0-style problem analysis: 5 recurring issues in AI Studio builds

While building GapLens, I treated AI Studio as another “module” under my R0 / SSOT discipline.  
I ended up with a list of **5 principle-level problems** and matching **prompt design rules**.

The goal:  
Even if you don’t care about this particular app,  
you can reuse these principles for your own education MVPs.

Each item includes:

- The underlying issue (abstracted);  
- Why it’s risky in an education setting;  
- Prompt-level mitigation.

### 2.1 Language mismatch: mixed Chinese/English, inconsistent voice

**Issue**

- Exams may be in Chinese, but the model eagerly preserves parts of the original wording, producing phrases like:  
  `Find the value of A 減去 B 的倒數`.
- For international judges or students, this breaks immersion and comprehension.

**Risk in education**

- Students might only know one language. Mixed text increases cognitive load.  
- For a hackathon, inconsistent language is an immediate “low polish” signal.

**Prompt rule**

Make the language policy explicit:

> `All user-facing content in this app must be in English only, even if the input exams or notes are not.`  
> `When exams or notes are in Chinese, read and interpret them, then restate all problems, errors and explanations in clear English.`  
> `Do not mix languages inside a single sentence and do not output any non-English characters in the UI.`

---

### 2.2 Internal leakage: developer comments / JSON / system text in the UI

**Issue**

- The same prompt often contains:
  - JSON schemas, field descriptions,
  - developer instructions, and
  - user exam content.  
- Without guardrails, Gemini happily echoes dev comments or JSON into user-facing fields.

**Risk**

- Students and judges see text like  
  `"This is excellent and follows JSON schema rules..."`  
  inside a question card.
- It instantly feels like a broken prototype.

**Prompt rule**

Explicitly forbid internal concepts:

> `You are generating content for teachers and students.`  
> `Never mention prompts, JSON schemas, developer notes, or system messages in any user-facing field.`  
> `Do not output JSON, code blocks, or backticks in questions, explanations, or summaries.`  

And define what each field is allowed to contain:

> `The 'question' field contains only the problem statement.`  
> `The 'explanation' field contains only the explanation for a student, with no internal comments.`  

---

### 2.3 Overloaded fields: one block containing meta + solution + commentary

**Issue**

- Models like to be “helpful” and pack everything into one response:
  - multiple questions and answers,
  - meta commentary for the teacher,
  - even developer-level reflections.

**Risk**

- The UI becomes unreadable, especially on small screens.  
- Teachers can’t quickly see **what the student actually did wrong**.

**Prompt rule**

Constrain both structure and length:

> `Limit each explanation to at most 3 short sentences: (1) what went wrong, (2) the correct idea, (3) one practical tip.`  
> `Do not restate the full problem text inside explanations.`  

---

### 2.4 Scope confusion: MVP supports one flow, UI pretends to be a full LMS

**Issue**

- In my head, GapLens could one day have:
  - advanced dashboards,  
  - groups management,  
  - per-topic mastery over time.  
- In this 1-week hackathon, the realistic scope is:
  - **Exam Mode** for individual students;  
  - a simple **Class View** once there are ≥2 students.

**Risk**

- If the UI contains “Coming soon”, fake student names, or locked buttons everywhere,  
  judges may assume core things are broken.

**Prompt rule**

Nail the MVP scope inside the prompt:

> `For this MVP, focus on Exam Mode for individual students and a lightweight Class View based on uploaded exams.`  
> `Do not render any tabs or buttons for features that are not described below.`  

Future ideas can live in the side copy (HOW IT WORKS), not as half-implemented buttons.

---

### 2.5 Usability: narrow columns with inner scrollbars

**Issue**

- A common pattern is:
  - place error lists inside a narrow column,  
  - add another scrollbar inside each card.

**Risk**

- Demo recording becomes painful.  
- On phones, it’s almost unusable.

**Prompt rule**

Encode basic layout principles:

> `Design the UI with a single vertical scroll.`  
> `Cards should expand to fit their content; do not use inner scrollbars inside cards.`  
> `For longer content, use collapsible sections with a one-line summary when collapsed.`  

---

## 3. MVP design: data structure & teacher flow

### 3.1 Roles and modes

**Roles**

- **Teacher / Educator** – primary operator of the app.  
- **Student** – sees outputs (reports, homework) but doesn’t operate the tool in this MVP.

**Modes**

1. **Exam Mode – Diagnose and remediate from graded papers**

   - Teacher uploads photos of graded exams (same subject, same class).  
   - Gemini extracts questions and answers, classifies error types:  
     - Conceptual, Procedural, Careless.  
   - Output:
     - Student Summary + Personal Homework Pack per student;
     - Class View with shared gaps and small-group suggestions.

2. **Note Mode – Extend learning from notes**

   - Teacher or student uploads photos of class notes / board work.  
   - Gemini generates:
     - a concept map,  
     - practice items,  
     - optional extension material.  

   For the hackathon, Exam Mode is the primary story; Note Mode is a secondary, lighter demo.

---

### 3.2 Conceptual data structure

A simplified conceptual structure (I did **not** ask Gemini to return this exact JSON, but I used it to think and to explain constraints):

```jsonc
{
  "student": {
    "id": "S01",
    "alias": "Student A",
    "grade": "9",
    "subject": "Algebra"
  },
  "exam": {
    "id": "E2025-12-Midterm",
    "questions": [
      {
        "qId": "Q1",
        "label": "Q1",
        "topic": "Linear equations",
        "skills": ["equation setup", "one-variable solving"]
      }
    ]
  },
  "responses": [
    {
      "studentId": "S01",
      "questionId": "Q1",
      "rawAnswer": "…",
      "errorType": "Conceptual | Procedural | Careless",
      "errorTags": ["equation setup", "sign error"],
      "explanation": "Short student-facing explanation."
    }
  ],
  "studentSummary": {
    "studentId": "S01",
    "score": 18.5,
    "identifiedGaps": [
      "Consistency in subject–verb agreement and noun plurality.",
      "Accurate use of different word forms."
    ],
    "homeworkPack": [
      {
        "qId": "HW1",
        "prompt": "Rewrite these sentences with correct subject–verb agreement.",
        "mode": "text"
      }
    ]
  },
  "classSummary": {
    "topGaps": [
      { "topic": "Quadratic factoring", "severity": 0.8 },
      { "topic": "Sign errors", "severity": 0.6 }
    ],
    "groupSuggestions": [
      {
        "groupLabel": "Factoring Recovery",
        "students": ["S01", "S03", "S05"],
        "sessionPlan": "10min GCF recap, 20min worksheet."
      }
    ]
  }
}
