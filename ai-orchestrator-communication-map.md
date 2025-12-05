# AI Orchestrator Communication Map (L1–L10)

_Companion note to: [Arin’s R0 / AI Orchestrator Methodology](./r0-ai-orchestrator-methodology.md)_

This document adds a **second axis** to the R0 / AI Orchestrator methodology:

- The original methodology describes **workflow layers (R0, L1–L10)** for tasks and responsibilities.
- This note defines **L1–L10 as maturity levels of people or teams using AI**, and
- Provides a **communication playbook** for AI Application PMs who must work with stakeholders at different levels.

The goal:  
When I say “this stakeholder is at L2” or “this team operates at L6–L7”, I immediately know:

- how deep I can go into orchestration concepts,
- what kind of artifacts to use (slides, PRD, demo, logs…),
- and what I realistically want to achieve from this conversation.

---

## 1. L1–L10: AI Usage & Orchestration Maturity

This L1–L10 ladder is about **how a person or team actually uses AI in daily work**, from “basic consumer” to “org-level orchestrator”.

| Level | Label                    | Short description                                                                 |
| ----- | ------------------------ | --------------------------------------------------------------------------------- |
| L1    | Tool Consumer            | Uses AI for ad-hoc Q&A, translation, rewriting; no workflow or prompt structure. |
| L2    | Single-Task User         | Reliably uses AI for a few repetitive tasks (emails, summaries, drafts).         |
| L3    | Multi-step Player        | Manually runs multi-step conversations and task breakdowns with AI.              |
| L4    | Personal SOP / Prompt Designer | Maintains personal prompt library / SOPs; stable for self-use.           |
| L5    | Semi-Automation Designer | Connects AI with tools (n8n/Make/scripts) to build semi-automated flows.         |
| L6    | AI Application PM        | Designs AI features, PRDs, evaluation metrics; works with Eng/DS.                |
| L7    | AI Orchestrator PM       | Designs multi-agent, multi-tool orchestration across systems.                    |
| L8    | AI Product Architect     | Builds shared AI capability platforms across products.                            |
| L9    | Org-level AI Enablement  | Designs training, governance, and rollout across departments.                    |
| L10   | AI-First Transformation Leader | Uses AI as a core element of company strategy and business model.       |

This scale is deliberately **orthogonal** to the workflow layers in the R0 document:

- There, L1–L10 describes “what kind of work is being done” (problem framing, architecture, implementation, QA…).
- Here, L1–L10 describes “how mature the person/team is in using and orchestrating AI”.

---

## 2. Universal 4-Step Communication Framework

Before we go level-by-level, I use the same 4-step routine whenever I enter a meeting as an AI Application PM.

### Step 1 – Estimate the stakeholder’s level

I do not need an exact number; a band is enough: **L1–L2 / L3–L4 / L5–L6 / L7–L8 / L9–L10**.

Three probing questions usually suffice:

1. “What do you currently use AI for in your work?”
2. “Do you already have any fixed prompts, templates, or SOPs that you reuse?”
3. “Have you connected AI with any tools or data sources (e.g. n8n, scripts, APIs)?”

### Step 2 – Decide the communication goal (not just the depth)

- With L1–L2 I don’t care about model choice; I care about **pain points** and **success picture**.
- With L3–L4 I want to extract their existing **implicit SOP** and turn it into something shareable.
- With L5–L6 I want a clear **API / data contract** and **evaluation metrics**.
- With L7–L8 I want to place this request inside the **overall AI platform / architecture**.
- With L9–L10 I want alignment on **strategy, governance and investment**.

### Step 3 – Choose the right artifacts

Different maturity levels respond better to different artifacts:

- L1–L2 → screenshots, simple diagrams, live demo, before/after examples.
- L3–L4 → step lists, prompt templates, checklists.
- L5–L6 → PRDs, sequence diagrams, table schemas, KPI tables.
- L7–L8 → capability maps, architecture diagrams, roadmap slides.
- L9–L10 → one-pagers, business cases, portfolio overviews.

### Step 4 – Run two structured conversations

For almost every project I separate:

1. **Discovery conversation (requirements)**  
   - How is the task done today?  
   - Where does it hurt the most?  
   - What would “success” look like in 3–6 months?  
   - What is acceptable to keep manual?  
   - What kinds of AI errors are acceptable vs. unacceptable?

2. **Delivery conversation (execution & ownership)**  
   - Who owns the SOP / prompt / config in the long run?  
   - Who looks at logs and metrics?  
   - What is the rollback path if things go wrong?  
   - How often do we review and adjust (cadence)?

The rest of this document is a **level-by-level playbook** that plugs into this 4-step framework.

---

## 3. Level-by-Level Playbook

Each level below follows the same structure:

- **Profile** – how this person typically behaves with AI.  
- **PM goals** – what I want to get from the interaction.  
- **Key questions** – discovery prompts.  
- **Execution focus** – how to move from talk to action.  
- **Artifacts / outputs** – what we should produce together.  
- **Typical risks** – failure patterns to watch for.

### L1 – Tool Consumer

- **Profile**  
  Uses AI for quick questions, translation, or small rewrites. No concept of prompts or workflows.
- **PM goals**  
  - Extract concrete pain points and desired outcomes.  
  - Understand manual tolerance and risk tolerance.
- **Key questions**  
  - “Walk me through how you currently do this task.”  
  - “Which step feels most annoying or time-consuming?”
- **Execution focus**  
  - Do not talk about models or orchestration.  
  - Prototype a single-button experience (paste → click → result).
- **Artifacts / outputs**  
  - 1–2 concrete use-case descriptions.  
  - Simple before/after examples and a minimal flow diagram.
- **Typical risks**  
  - Magical thinking (“AI will solve everything”).  
  - Extremely vague requirements.  
  - No understanding that human review is still needed.

---

### L2 – Single-Task User

- **Profile**  
  Frequently uses AI for a few recurring tasks (emails, summaries, wording), but still one task at a time.
- **PM goals**  
  - Cluster their ad-hoc usage into **a small set of repeatable scenarios**.  
  - Define clear entry points in the product.
- **Key questions**  
  - “What are the top three things you ask AI to do most often?”  
  - “In each of these, which part is still painful?”
- **Execution focus**  
  - Design dedicated “shortcuts” or templates for those scenarios.  
  - Let them help name features and decide where buttons live.
- **Artifacts / outputs**  
  - Use-case cards.  
  - Short how-to notes and UI sketches.
- **Typical risks**  
  - Too many similar entry points.  
  - No one maintaining the library of scenarios.

---

### L3 – Multi-step Player

- **Profile**  
  Breaks tasks into steps and runs multi-turn conversations; relies on memory to keep the flow.
- **PM goals**  
  - Externalize their **mental SOP** into a documented flow.
- **Key questions**  
  - “How many steps do you usually take from start to finish?”  
  - “What do you usually tell the AI at each step?”
- **Execution focus**  
  - Co-create a flow diagram on a whiteboard or Miro.  
  - Highlight decision points, inputs, and outputs.
- **Artifacts / outputs**  
  - Standardized flow chart.  
  - Step-by-step checklist and prompt drafts for each step.
- **Typical risks**  
  - Flow only exists in their head; disappears if they leave.  
  - Heavy reliance on improvisation → unstable output quality.

---

### L4 – Personal SOP / Prompt Designer

- **Profile**  
  Maintains a personal Notion page, prompt library, or macro collection; can consistently reproduce results for themselves.
- **PM goals**  
  - Turn personal best practices into **team-level standards**.  
  - Assign ownership for template maintenance.
- **Key questions**  
  - “Which prompts or SOPs do you use every week?”  
  - “What would a new colleague need to know to use them safely?”
- **Execution focus**  
  - Break prompts into structured fields: input slots, tone, output format, warnings.  
  - Define when **not** to use a given template.
- **Artifacts / outputs**  
  - Shared template collection in Notion / product.  
  - Scene-based usage guide for each template.
- **Typical risks**  
  - Template sprawl and version chaos.  
  - No one responsible for updates.

---

### L5 – Semi-Automation Designer

- **Profile**  
  Builds flows with n8n/Make/scripts; connects AI to forms, sheets, CRMs, etc.
- **PM goals**  
  - Make **boundaries and error handling explicit**.  
  - Ensure there is logging and recovery, not just “it works”.
- **Key questions**  
  - “Which steps are fully automated, which require manual approval?”  
  - “How do you notice when AI output is wrong? What do you do then?”
- **Execution focus**  
  - Design both happy path and error paths.  
  - Specify log fields and retention.
- **Artifacts / outputs**  
  - Flow diagrams, sequence diagrams.  
  - Runbook with recovery steps and log schema.
- **Typical risks**  
  - Zero logging → impossible to debug.  
  - No rollback plan.  
  - Knowledge concentrated in one person.

---

### L6 – AI Application PM

- **Profile**  
  Writes AI-related PRDs, understands basic model and infra constraints, collaborates with Eng/DS.
- **PM goals**  
  - Converge on **MVP scope** and **success metrics**.  
  - Align on required resources.
- **Key questions**  
  - “What is the primary metric for this feature?”  
  - “What must exist in the MVP, and what can wait?”  
  - “What data/infra do we absolutely need?”
- **Execution focus**  
  - Use roadmaps and metrics tables, not feature wish-lists.  
  - Ensure someone owns post-launch monitoring.
- **Artifacts / outputs**  
  - PRD, roadmap, KPI spec.  
  - Iteration plan (POC → MVP → Beta → GA).
- **Typical risks**  
  - Scope creep.  
  - Nobody is accountable for killing or adjusting features based on data.

---

### L7 – AI Orchestrator PM

- **Profile**  
  Works with multi-agent systems, tool routing, and cross-system workflows.
- **PM goals**  
  - Fit the new demand into the existing **agent + tool landscape**, not create isolated islands.
- **Key questions**  
  - “How are agent roles currently split?”  
  - “Which agent should own this task? Do we really need a new one?”
- **Execution focus**  
  - Align on routing rules, memory boundaries, and tool permissions.  
  - Identify when the orchestrator should call humans.
- **Artifacts / outputs**  
  - Multi-agent architecture diagrams.  
  - Task routing rules and agent responsibility docs.
- **Typical risks**  
  - Creating a new agent for every use case.  
  - Overlapping responsibilities and race conditions.  
  - No one watching system-level health.

---

### L8 – AI Product Architect

- **Profile**  
  Responsible for the shared AI platform used by many products.
- **PM goals**  
  - Abstract current needs into **reusable capabilities** with stable interfaces.
- **Key questions**  
  - “What capability is this really? Retrieval? Classification? Summarization?”  
  - “Which other products will likely reuse this in the next 6–12 months?”
- **Execution focus**  
  - Separate one-off shortcuts from platform capabilities.  
  - Define versioning and compatibility strategy.
- **Artifacts / outputs**  
  - Capability map, platform roadmap.  
  - API/service boundaries with owners.
- **Typical risks**  
  - Hacking the platform for special cases and breaking others.  
  - Growing technical debt and tight coupling.

---

### L9 – Org-level AI Enablement

- **Profile**  
  Works on training, governance, rollout and budget across multiple departments.
- **PM goals**  
  - Translate project ideas into **organizational investments** and capability gaps.
- **Key questions**  
  - “Where are most employees on the L1–L10 ladder today?”  
  - “Where do we want different roles to be in 1–2 years?”  
  - “What training, tools, and policies are needed to get there?”
- **Execution focus**  
  - Propose a **capability roadmap** per role.  
  - Connect projects to policy, risk, and budget.
- **Artifacts / outputs**  
  - One-page summaries, capability matrices.  
  - Training plans and platform investment priorities.
- **Typical risks**  
  - Chasing quick wins only, no foundation.  
  - No governance → inconsistent or risky AI usage.

---

### L10 – AI-First Transformation Leader

- **Profile**  
  Thinks about strategy, business models, ecosystems, and long-term advantage.
- **PM goals**  
  - Position AI projects as part of a **portfolio of strategic bets**.
- **Key questions**  
  - “Is this initiative efficiency-driven or growth-driven?”  
  - “How does it fit into the overall AI investment portfolio and risk appetite?”
- **Execution focus**  
  - Provide clear views on upside, downside, and dependencies.  
  - Highlight a few leverage-point projects rather than many scattered experiments.
- **Artifacts / outputs**  
  - Executive decks, portfolio maps, risk/return analyses.
- **Typical risks**  
  - Chasing buzzword projects without building capabilities.  
  - No explicit exit criteria for failing bets.

---

## 4. How This Connects Back to R0

In the main R0 methodology:

- R0 guards the **SSOT and Core Kernel**,
- workflow layers (R0, L1–L10) structure **what kind of work** AI vs. humans do.

This communication map complements it by:

- giving a shared language for **how mature each stakeholder is** in working with AI, and
- giving me, as an AI Orchestrator PM, a **repeatable playbook** for discovery and delivery conversations.

Together, they make it easier to scale:

- from “one person + one AI”  
  to “many people + many systems + many AIs, under one coherent protocol”.
