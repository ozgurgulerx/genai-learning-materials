# Generative AI Use-Cases – Question Candidates

Below are 15 detailed Q&A entries illustrating how GenAI can unlock value across diverse domains.

---

### Q1: How can GenAI streamline blog, social, and video script production while preserving brand voice?

**Category:** Use-Case  
**Difficulty:** Easy  
**Tags:** Marketing, Content Automation

**Sample Answer:**
1. **Brand Voice Embedding:** Fine-tune an LLM on historical posts and style guides.
2. **Brief → Outline:** User supplies campaign brief; model outputs headline and outline.
3. **Draft Generation:** LLM expands outline into long-form article, 3 tweet variants, and video script.
4. **Style QA:** A classifier checks tone, banned words, and reading level.
5. **Human Review Loop:** Editors accept/edit; edits feed back for continual SFT.
Result: 60-80 % reduction in copywriter hours while maintaining on-brand consistency.

---

### Q2: Outline a GenAI workflow that triages support tickets, drafts responses, and learns from live agent edits.

**Category:** Use-Case  
**Difficulty:** Easy-Medium  
**Tags:** Customer Support, Workflow Automation

**Sample Answer:**
• **Intent & Sentiment Detection:** Classifier routes urgent vs. standard tickets.
• **Context Retrieval:** Fetch customer history and relevant KB articles.
• **Draft Composer:** LLM drafts reply with placeholders for sensitive data.
• **Agent UI:** Live agents tweak reply; changes captured as reinforcement signals.
• **Continuous Fine-Tuning:** Weekly SFT on edited drafts improves accuracy; track deflection rate and CSAT.

---

### Q3: What risks and safeguards are needed when using LLMs to migrate legacy codebases?

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Code Refactor, Legacy Systems

**Sample Answer:**
Risks: syntactic errors, hidden side-effects, inconsistent styling, security regressions.
Safeguards:
1. **Unit-Test Harvesting:** Auto-extract tests from old repo to catch behavior drift.
2. **AST Diff Checks:** Ensure structural equivalence.
3. **Staged Rollout:** Canary refactored modules behind feature flags.
4. **Secure Coding Lint:** Run static analyzers (Snyk, Semgrep) post-generation.

---

### Q4: Describe how generative models can propose novel molecules and what validation pipeline follows.

**Category:** Use-Case  
**Difficulty:** Medium-Hard  
**Tags:** Drug Discovery, Molecule Generation

**Sample Answer:**
1. **SMILES VAE/Transformer** generates candidate molecules optimized for binding score via RL.
2. **In-silico Filtering:** Docking sims, ADMET predictors prune 99 %.
3. **Lab Automation:** Synthesizable hits sent to robotic lab; assay data logged.
4. **Active Learning Loop:** Wet-lab results retrain property predictors and reward model.
Cycle compresses years-long discovery to months while reducing wet-lab spend.

---

### Q5: Explain a retrieval-augmented architecture that answers questions over earnings reports with high compliance.

**Category:** Use-Case  
**Difficulty:** Hard  
**Tags:** Finance, RAG, Compliance

**Sample Answer:**
• **Document Parsing:** Convert 10-K PDFs to structured chunks; store in vector DB with fiscal metadata.
• **Query Understanding:** LLM rewrites investor’s query into canonical form.
• **Retriever:** Dense + sparse hybrid fetches clauses; generator cites paragraph/page.
• **Compliance Layer:** Regex + rule engine ensure no forward-looking statements or speculation; answers logged for audit.

---

### Q6: How would you use LLMs to generate real-time, individualized product descriptions in an e-commerce app?

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Retail, Personalization

**Sample Answer:**
1. **Input Signals:** User’s recent browses, locale, and psychographic segment.
2. **Prompt Template:** ``You are a product copywriter…`` plus dynamic attributes.
3. **Cache & AB Test:** Common segments cached; A/B on conversion uplift.
4. **Guardrails:** Brand tone classifier + banned claims filter.

---

### Q7: Discuss using diffusion + LLMs to create dynamic storylines and NPC dialogue.

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Gaming, Narrative

**Sample Answer:**
LLM generates quest outline and dialogue trees; diffusion model renders scene art and character portraits on the fly. A state machine tracks player choices; prompt includes game state so narrative remains coherent. Safety filters stop NSFW content.

---

### Q8: What constraints shape a GenAI voice assistant for hands-free industrial tasks?

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Voice, Edge AI

**Sample Answer:**
Constraints: sub-200 ms latency, offline fallback, noisy environment, gloves preventing screen use.
Solution: On-device keyword spotting, edge-quantized LLM (4-bit) for commands; cloud fallback for complex queries. Mesh network syncs models during Wi-Fi windows.

---

### Q9: Which guardrails ensure hallucinations don’t introduce liability when suggesting legal clause edits?

**Category:** Use-Case  
**Difficulty:** Hard  
**Tags:** LegalTech, Guardrails

**Sample Answer:**
• **Citation Requirement:** Model must cite source clause/precedent; UI disables accept until citation present.
• **Diff-Only Output:** Suggests redlines not free-text.
• **Expert Verification Workflow:** Senior counsel approves before merge.
• **Audit Log:** Store prompt/response for each edit for e-discovery compliance.

---

### Q10: Propose a multi-agent GenAI tutor that adapts to a learner’s gaps and motivation.

**Category:** Use-Case  
**Difficulty:** Hard  
**Tags:** Education, Adaptive Learning

**Sample Answer:**
Agents:
1. **Diagnoser:** Bayesian knowledge tracing + LLM to detect misconceptions.
2. **Content Generator:** Crafts personalized explanations and practice questions.
3. **Motivator:** Uses reinforcement signals (streaks, praise) to sustain engagement.
4. **Supervisor:** Verifies accuracy, aligns with curriculum standards.
System logs learning gains and updates learner model each session.

---

### Q11: How can GenAI assist human moderators to scale throughput without over-blocking?

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Content Moderation, Safety

**Sample Answer:**
Stacked approach: fast transformer classifier flags likely policy hits; borderline cases routed to LLM for policy reasoning with chain-of-thought; outputs a rationale and severity score to moderator UI. Active-learning loop retrains on moderator decisions.

---

### Q12: Describe a co-creative workflow where text→image→3D model generations accelerate product design.

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Design, Multimodal

**Sample Answer:**
Designer enters verbal concept → diffusion model outputs style boards → LLM suggests material specs → point-E or DreamFusion converts to rough 3D mesh → CAD engineer refines. Iterations tracked in PLM system; reduces concept-to-prototype time by 50 %.

---

### Q13: Outline how an LLM can translate natural-language questions into SQL and visualize results securely.

**Category:** Use-Case  
**Difficulty:** Hard  
**Tags:** Data Analytics, NL2SQL

**Sample Answer:**
Pipeline: NL query → LLM generates parameterized SQL with schema awareness → execution in read-only warehouse role → JSON result sent to Vega-Lite chart generator. RBAC ensures no PII columns accessible; query cost estimator warns users.

---

### Q14: Suggest a GenAI pipeline to auto-compile ESG reports from scattered internal data.

**Category:** Use-Case  
**Difficulty:** Hard  
**Tags:** ESG, Reporting

**Sample Answer:**
1. **Data Connectors:** Ingest utilities, HR, procurement sheets.
2. **Schema Harmonization:** LLM maps fields to ESG taxonomy.
3. **Metrics Calculator:** Pandas or SQL compute KPIs.
4. **Narrative Generator:** LLM drafts sections with citations to data source IDs.
5. **Compliance Checker:** Validates against GRI/SASB frameworks.

---

### Q15: How could GenAI improve accessibility for visually impaired users consuming complex charts?

**Category:** Use-Case  
**Difficulty:** Medium  
**Tags:** Accessibility, Multimodal

**Sample Answer:**
• Use computer-vision encoder to parse chart elements → LLM produces concise textual summary.
• Interactive Q&A mode lets user ask follow-up questions (trends, outliers).
• Output formatted for screen readers with ARIA landmarks.

1. **Content Marketing Automation:** How can GenAI streamline blog, social, and video script production while preserving brand voice?
2. **Customer Support Copilots:** Outline a GenAI workflow that triages tickets, drafts responses, and learns from live agent edits.
3. **Code Refactoring at Scale:** What risks and safeguards are needed when using LLMs to migrate legacy codebases?
4. **Drug Discovery:** Describe how generative models can propose novel molecules and what validation pipeline follows.
5. **Financial Document QA:** Explain a retrieval-augmented architecture that answers questions over earnings reports with high compliance.
6. **Retail Personalization:** How would you use LLMs to generate real-time, individualized product descriptions in an e-commerce app?
7. **Game Narrative Design:** Discuss using diffusion + LLMs to create dynamic storylines and NPC dialogue.
8. **Voice Assistants for Field Workers:** What constraints (latency, offline mode) shape a GenAI solution for hands-free industrial tasks?
9. **Legal Contract Review:** Which guardrails ensure hallucinations don’t introduce liability when suggesting clause edits?
10. **Education Tutoring Systems:** Propose a multi-agent GenAI tutor that adapts to a learner’s knowledge gaps and motivation.
11. **Content Moderation:** How can GenAI assist human moderators to scale throughput without over-blocking?
12. **Design Ideation:** Describe a co-creative workflow where text→image→3D model generations accelerate product design.
13. **Data Analytics Co-pilot:** Outline how an LLM can translate natural-language questions into SQL and visualize results securely.
14. **Sustainability Reporting:** Suggest a GenAI pipeline to auto-compile ESG reports from scattered internal data sources.
15. **Multimodal Accessibility:** How could GenAI improve accessibility for visually impaired users consuming complex charts?
