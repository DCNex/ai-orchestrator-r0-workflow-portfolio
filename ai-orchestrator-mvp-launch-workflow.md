# AI Orchestrator MVP→Launch Workflow Guide

[English](./ai-orchestrator-mvp-launch-workflow.md) | [繁體中文 / Traditional Chinese](./ai-orchestrator-mvp-launch-workflow-zh-TW.md)

---

## 0. Series map: where this guide fits

This repository currently has three core documents:

1. **Series Part 1: Arin’s R0 / AI Orchestrator Methodology**  
   - Defines R0, SSOT (Single Source of Truth), the Core Kernel + Modules structure, and the L1–L10 work layers.  
   - File: `r0-ai-orchestrator-methodology.md`

2. **Series Part 2: AI Orchestrator Communication Map (L1–L10)**  
   - Uses L1–L10 to describe how mature people/teams are in using AI, and gives a playbook for AI Product/Project roles.  
   - File: `ai-orchestrator-communication-map.md`

3. **Series Part 3: this guide (MVP→Launch workflow)**  
   - Focuses on execution:  
     - Topic selection → MVP design → demo production → development & deployment → launch communication → observation & iteration  
   - Explains:  
     - How R0, Core and Modules share responsibilities  
     - Which AI tools are used at each stage, and why

You can think of the three as:

- Part 1 – **Architecture & rules**  
- Part 2 – **People & communication map**  
- Part 3 – **End-to-end execution SOP**

---

## 1. Roles and use cases

### 1.1 Who this workflow is for

This workflow is intended for:

- People using multiple AI tools to deliver internal projects or process improvements  
- People turning side projects into usable, presentable products  
- Anyone moving toward an AI Orchestrator / AI Workflow Architect / AI PM role

### 1.2 Typical topics

- Subscription manager, digital asset library, personal finance dashboards  
- City intelligence maps (e.g. nuisance facilities, pet-friendly places)  
- Internal performance / incentive dashboards and workflow tools  
- Remote Life OS modules and public demos

---

## 2. Tool roles (structured, not random)

### 2.1 Planning & structure

- **ChatGPT**  
  - Uses: requirements breakdown, workflow design, data structure drafts (JSON / schema), PRD and technical documentation, prompt design.  

- **Claude**  
  - Uses: reading/refactoring code, debugging, larger implementation refactors.  

- **Perplexity**  
  - Uses: market/competitor/technical research to support design and business assumptions.  

- **NotebookLM**  
  - Uses: project knowledge base for a specific topic (docs, notes, conversations).

### 2.2 UI / visual

- **Google AI Studio** – fast app/web mock screens and flows.  
- **Gemini** – covers, icons, UI style and layout suggestions.

### 2.3 Video & audio

- **Sora / Kling / Hailuo** – product demo or scenario video generation.  
- **Suno** – background music.  
- **TTSMaker** – voice-over from scripts (multi-language).  
- **Canva** – editing, subtitles, info cards, multi-format export.

### 2.4 Localization & culture

- **Doubao** – Mainland China style copy and phrasing for localization.

---

## 3. Seven-stage workflow from the R0 perspective

Below is the workflow I actually use for internal projects and side projects.  
Each stage describes:

- What **R0** is responsible for  
- Which **AI tools** are typically involved  
- What **artifacts** should be produced

---

### Stage 0 – Topic & direction

**Goal**  
Decide what to build in this cycle, for whom, and within what time box (e.g. 4–8 weeks).

**R0 responsibilities**

- Create an experiment card in SSOT containing:  
  - Topic name  
  - Target audience / persona  
  - Time window (e.g. 4–8 weeks)  
  - Rough success criteria (e.g. 1 demo video + 1 shareable URL)

**AI usage**

- ChatGPT: topic breakdown, personas and pain points.  
- Perplexity: existing solutions and competitors.  
- Doubao: alternate framing for Mainland audiences to see differences in narrative.

---

### Stage 1 – Initial Problem / Market Fit check

**Goal**  
Decide whether the topic deserves an MVP.

**R0 responsibilities**

- Record hypotheses and success/failure conditions in SSOT, such as:  
  - If we reach certain usage or feedback within N weeks, the experiment is considered successful.

**AI usage**

- Perplexity: list competitors, typical use cases, pricing patterns.  
- ChatGPT: assemble a one-page market snapshot and angle-of-entry summary.

Result: a short Markdown/Notion page you can later reference as part of your case.

---

### Stage 2 – MVP & experience design (with cultural angle)

**Goal**  
Describe v0.1 clearly in text, focusing on one critical work scenario.

**R0 responsibilities**

- Assign or create a module (e.g. `M02_Subscription`, `M03_CityIntel`).  
- Define:  
  - Required core fields for this cycle  
  - Module–Core interfaces and permissions

**AI usage**

- ChatGPT:  
  - MVP feature list and step-by-step flow.  
- ChatGPT with personas:  
  - Simulated flows for different cultures/roles; highlight friction points.  
- Doubao:  
  - Adjust key copy and flow descriptions for Mainland China.

Suggested output: an “MVP script + flow diagram” that design and dev can read directly.

---

### Stage 3 – Prototype (mock UI) & demo storyline

**Goal**  
Provide screens and story before any real code is written.

**R0 responsibilities**

- Check alignment between user flows and data flows; stay neutral on colors and visual details.

**AI usage**

- Google AI Studio: mock app/web screens and basic interactions.  
- Gemini: refine visuals, color hints and basic layout.  
- ChatGPT: draft demo narration and storyline (often bilingual).

---

### Stage 4 – Demo video production

**Goal**  
Produce a demo video for slides, GitHub and social channels.

**R0 responsibilities**

- Ensure the demo does not oversell beyond realistic technical and business scope.

**AI usage**

- Sora / Kling / Hailuo: generate operation or scenario footage.  
- TTSMaker: turn scripts into voice-over.  
- Suno: create background music.  
- Canva: edit, add subtitles and annotations, export to multiple aspect ratios (X / Threads / IG / Shorts).

---

### Stage 5 – Development & deployment

**Goal**  
Ship a working MVP to a public, shareable URL.

**R0 responsibilities**

- Update SSOT:  
  - Schema (tables)  
  - APIs  
  - `modules_map` and `integration_registry`  
- Review AI-generated code for:  
  - Respecting module boundaries  
  - Safety and maintainability

**AI usage**

- ChatGPT:  
  - Help design tables/APIs and write initial code.  
- Claude:  
  - Read, refactor, debug.  
- NotebookLM:  
  - Store key snippets and design decisions as a technical knowledge base.

---

### Stage 6 – Launch communication

**Goal**  
Make the right people understand what the tool does and why it matters.

**R0 responsibilities**

- Decide the primary communication goal for this cycle:  
  - Feedback, portfolio proof, preparing for a next phase, etc.

**AI usage**

- ChatGPT:  
  - Write README / landing copy, short posts, and a one-line tagline.  
- Perplexity:  
  - Suggest domain-relevant keywords and search terms.  
- Doubao:  
  - Finalize Mainland-facing copy.  
- Gemini + Canva:  
  - Produce visuals: promo images, diagrams, slide pages.

---

### Stage 7 – Data, feedback & next decisions

**Goal**  
Use real usage and feedback to decide whether to double down, adjust or pause.

**R0 responsibilities**

- Compare results with hypotheses and success criteria in SSOT,  
  then choose: continue, pivot, or stop.

**AI usage**

- ChatGPT:  
  - Summarize metrics and feedback into:  
    - Functional / UX issues  
    - Cultural / communication issues  
- NotebookLM:  
  - Turn this cycle into a reusable case for future projects and external storytelling.

---

## 4. How to combine this with the other series documents

- For **why** R0, SSOT, Core+Modules and L1–L10 levels exist →  
  read `r0-ai-orchestrator-methodology.md`.

- For **how to talk to people** at different AI maturity levels and what to deliver to each →  
  read `ai-orchestrator-communication-map.md`.

- For **how to actually run one full cycle from idea to MVP to launch and iteration** →  
  use this guide as an operational checklist.

Together, the three documents form:

1. Architecture & protocol (R0 / SSOT / L1–L10 work layers)  
2. Communication & stakeholder management (L1–L10 AI maturity)  
3. Execution workflow (0 → MVP → launch → iteration)
