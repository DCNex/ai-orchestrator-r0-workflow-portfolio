# Case Study – City Intel Map Kernels  
Nuisance Radar / Pet-Friendly Map / Land Radar

[English](./case-city-intel-map-kernels-ai-workflow.md) | [繁體中文 / Traditional Chinese](./case-city-intel-map-kernels-zh-TW.md)

---

## 1. Project Context

- **Project name:** City Intel Map Kernels  
- **Timeframe:** 2024–2025 (multiple MVP / experiment cycles)  
- **Role:** System Designer / R0 Architect / AI Orchestrator  
- **Domain:** City intelligence, life decision support, UGC map tools, one-person company products  

City Intel Map Kernels is a family of **map-based decision tools** built on a shared “city intelligence kernel”.  
So far, I have prototyped several thematic maps, including:

- **Nuisance Map (Nuisance Radar)** –  
  helps users understand “what kinds of nuisance facilities or conditions are near this area?”
- **Pet-Friendly Map** –  
  surfaces places that are actually friendly to pets (cafés, parks, public spaces, etc.).
- **Land Radar / Development Radar** –  
  focuses on land-use changes, development projects, environmental risks, and potential value shifts.

All of these maps share the same **core city / place data model** and only differ in how they interpret and visualize the data.  
The long-term goal is to establish a **reusable City Intel Kernel** that can support different decision tools  
(rent vs buy, where to live, where to run a side business, etc.) without rebuilding from scratch each time.

---

## 2. Problem Definition

In real life, people who need to decide **where to live or operate** face several recurring issues:

1. **Fragmented and inconsistent information**  
   - Nuisance information (pollution, noise, industrial facilities, etc.) is scattered across news, regulations, and random posts.  
   - Pet-friendly places are buried in Facebook groups, Google Maps reviews, and blogs.

2. **Generic maps show “points”, not “values or risks”**  
   - A typical map will show you stores and ratings,  
   - But not:
     - whether this area is noisy or uncomfortable to live in,
     - how friendly it is to pets and their owners,
     - whether nearby land is under development or carries long-term risks.

3. **Each theme tends to become a separate product with no shared kernel**  
   - A “pet map” here, a “nuisance map” there, a “property investment map” somewhere else,  
   - If each product has its own schema and logic, every new feature becomes a new system.

**Core problem:**  
> How can we design a **reusable city intelligence kernel**  
> so that multiple UGC-based maps can share one structure  
> while providing very different thematic views?

---

## 3. My Role: System Designer / R0 / AI Orchestrator

In this project, my responsibility is not just “building maps” but:

- Designing the **SSOT for the City Intel Kernel**:
  - Tables / JSON structures like `cities`, `places`, `place_tags`, `place_attributes`, `place_scores`;
  - Making sure thematic differences are expressed via tags/layers rather than separate incompatible schemas.

- Acting as **R0 (Root Orchestrator)**:
  - Maintaining the **Core Kernel** (city + place fundamentals);
  - Defining the boundaries of each thematic module (Nuisance Radar, Pet-Friendly Map, Land Radar);
  - Managing `modules_map` and integration rules for future modules.

- Serving as an **AI Orchestrator**:
  - Using multiple LLMs to:
    - Suggest schema improvements,
    - Co-design tag / category systems,
    - Generate copy and scenarios for the front-end,
    - Explore future workflows for UGC review and gamification.

- Working as a **one-person experiment operator**:
  - Treating each thematic map as a 4–8 week experiment slot;
  - Shipping minimal but real MVPs and observing how the kernel holds up.

---

## 4. Solution Design & Workflow

### 4.1 Core Data Model (City Intel Kernel)

The City Intel Kernel is centered around these main entities:

- `cities` / `districts`  
  - Core city and sub-region data (e.g., population density, zoning type, etc. if available).

- `places`  
  - Concrete locations (shops, facilities, intersections, etc.) with coordinates and basic attributes.

- `place_tags`  
  - A flexible tagging system that supports multiple themes layered on the same place:  
  - Examples: `nuisance_factory`, `noise_source`, `landfill`,  
               `pet_friendly_cafe`, `dog_park`, `vet_clinic`, etc.

- `place_attributes`  
  - Structured attributes such as opening hours, indoor/outdoor, paid/free, reservation required.

- `place_scores`  
  - Theme-specific scores, such as:
    - “pet-friendliness score”,
    - “living comfort score”,
    - “development / risk index”.

With this kernel:

- Nuisance Radar, Pet-Friendly Map and Land Radar are not three unrelated products;  
- They are **different views and filters** over the same core dataset.

### 4.2 Modules (Themes as Modules)

Each theme is implemented as a module on top of the kernel. For example:

- `M11_NuisanceRadar`  
  - Focus: noise, pollution, heavy traffic, industrial sites, nuisance facilities.  
  - Reads from kernel tables (`places`, `place_tags`, `place_scores`).  
  - Presents a “living comfort / nuisance” view to the user.

- `M12_PetFriendlyMap`  
  - Focus: pet-friendly cafés, restaurants, parks, accommodations, services.  
  - Uses tags and attributes to filter and rank locations.  
  - Can later integrate UGC reviews, photos, and check-ins.

- `M13_LandRadar`  
  - Focus: land-use changes, major development, environmental risk zones.  
  - Combines kernel place data with external sources (e.g., planning / zoning info).  
  - Presents risk / opportunity overlays on the same city map.

Each module is:

- Declared in the SSOT `modules_map` with:
  - Its ID, table prefix (if needed),
  - Dependencies on core kernel tables,
  - Optional module-specific config or extension tables.
- Designed to be **removable** without breaking the kernel.

### 4.3 AI Orchestration in Practice

LLMs are used at multiple layers instead of being a single “magic box”:

1. **L1–L2: Concept & Architecture Exploration**  
   - Ask AI to help synthesize:
     - Needs of couriers, pet owners, renters, side hustlers, etc.  
     - Common patterns from existing public tools (at the conceptual level).  
   - As R0, I use this to decide:
     - Which entities and relationships must exist in the kernel,
     - Which dimensions should be modeled via tags or scores.

2. **L3: Schema / API / UI Drafts**  
   - Let AI propose:
     - Supabase / PostgreSQL table structures,
     - API query patterns for map layers,
     - Basic front-end layout patterns (e.g., left list + right map).  
   - I review:
     - Whether proposals respect the Core vs Module separation,
     - Naming consistency and integrity.

3. **L4: Scenarios & Testing**  
   - Use AI to generate realistic personas and scenarios, such as:
     - A 30-year-old remote worker renting with a dog in Taipei;
     - A noise-sensitive person who needs quiet but urban access;
     - A buyer evaluating risk around a specific district.  
   - Use these scenarios to:
     - Check if the kernel and modules can answer their key questions,  
     - Identify missing attributes or views.

4. **L5: UX Copy & Narrative**  
   - AI helps draft:
     - Map descriptions, tooltips, filter explanations, FAQs.  
   - I revise for tone, local language, and proper positioning.

Overall, AI acts as a **multi-level co-designer / worker**,  
while R0 (me) owns:

- the structural consistency of the kernel,
- the separation between core and modules,
- and the pace and scope of experiments.

---

## 5. Experiments & Iterations

I treat City Intel as a sequence of experiments, not a one-shot “big product”.

### 5.1 Example Experiment Slots

| Slot ID     | Theme                | Scope             | Goal                                           |
|------------|----------------------|-------------------|------------------------------------------------|
| Ex-CITY-01 | Nuisance Map MVP     | Selected Taipei areas | Validate kernel can tag & render nuisance points |
| Ex-CITY-02 | Pet-Friendly Map MVP | Selected districts | Collect first batch of pet-friendly locations   |
| Ex-CITY-03 | Land Radar Concept   | Few key districts | Test data sources + visualization concepts     |

For each experiment, I:

1. Use AI to help write hypotheses and success criteria;  
2. Define minimal viable data collection (e.g., first 30–50 meaningful points);  
3. After launch, observe:
   - My own usage as a primary user,
   - Potential feedback from early testers,
   - Technical load (rendering, querying, maintenance).

At the end of each cycle, AI assists in drafting:

- a short experiment report,
- key learnings,
- and suggestions for next iterations.

---

## 6. Outcomes & Learnings

So far, City Intel Map Kernels have achieved:

- **Technical outcomes**:
  - A reusable **city kernel schema** that can support multiple thematic maps;
  - Successful rendering of several thematic views (nuisance, pet-friendly, etc.) on the same base.

- **Product outcomes**:
  - Deployed working MVPs for personal and limited external testing;
  - Confirmed that “map as a decision tool” is valuable for life and work choices.

- **Methodological outcomes (for R0 / AI Orchestrator)**:
  - Validated that SSOT + Core/Module architecture works well for spatial products;
  - Demonstrated how AI can assist at L2–L5 (schema, scenarios, UX copy, iteration planning),
    while R0 remains responsible for structure and integration.

Key learnings:

1. If the map is “theme-first and structure-later”,  
   adding a second or third theme becomes very painful.  
2. Treating the map as a **decision tool**, not just a visualization,  
   forces better thinking about behaviors, psychology, and tradeoffs.  
3. For a one-person company, a well-designed kernel allows future products  
   to reuse the same backbone instead of rebuilding each time.

---

## 7. Potential for Company Use

The City Intel Map Kernels approach can be adapted to several enterprise contexts:

- **Financial services / insurance**  
  - Use a city/kernel model plus:
    - branch / ATM networks,
    - customer distribution,
    - risk zones and claim history, etc.  
  - Let AI help generate visual reports and strategy narratives,  
    while a central R0 team governs the kernel and rules.

- **Retail / F&B chains**  
  - Maintain a shared “location kernel” for all outlets;  
  - Layer on:
    - expansion analysis modules,
    - event performance modules,
    - customer profile overlays.

- **City planning / ESG reporting**  
  - Combine environmental, noise, traffic, disaster risk data into one kernel;  
  - Allow different internal teams to build modules on top;  
  - Use AI as a narrative and simulation layer.

For me, City Intel Map Kernels is not only a product prototype,  
but also a **testbed for applying the R0 / AI Orchestrator methodology** to spatial decision-making.

In any future role that involves locations, branches, territories, or spatial risk,  
this kernel + module + AI orchestration mindset can be directly reused  
to design internal platforms and decision tools.
