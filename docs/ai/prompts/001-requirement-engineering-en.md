
# Requirements Engineering Prompts for Software Architects
AI-Driven Software Architecture Course — Session 3: Requirements Analysis & Architectural Drivers

These prompts are engineered using standard professional prompt design to transform raw inputs (interviews, SRS, PRD) into structured, reliable outputs for architectural decision-making.

---

## 1. Functional Requirements Extraction
**Goal:** Extract behaviors, capabilities, data flows, and system rules from raw texts without premature technical bias.

```text
Role: You are a Senior Systems Analyst and Requirements Engineer.
Context: The text below contains raw notes, transcripts, or project documentation from which operational requirements must be extracted.
Task: Identify and categorize all explicit and implicit functional requirements present in the text.
Constraints:
- Do not dive into technical implementation details or framework selections; focus strictly on behavior, inputs, processes, and outputs.
- Separate qualitative and non-functional requirements from this phase.
Output Format:
A table with the following columns:
[ID (FR-ID) | Requirement Title | Actor | Operation Description & Input/Flow | Pre-conditions & Post-conditions]

[Raw Document / Interview Text]
```

---

## 2. Non-Functional Requirements Extraction
**Goal:** Identify quality attributes, system constraints, and operational expectations that form the foundation of architectural decisions.

```text
Role: You are a Senior System Architect and Software Quality Auditor.
Context: We are extracting and structuring Non-Functional Requirements (NFRs) from input documentation.
Task: Discover all non-functional requirements, constraints, and quality standards mentioned in the text and place them into standard ISO 25010 categories.
Constraints:
- Include generic quality adjectives (e.g., fast, scalable, secure) only if evidenced in the text, and specify their level of explicitness.
- Avoid introducing your own assumptions; stick strictly to stated or directly inferable quality concerns.
Output Format:
A categorized report including:
1. Quality attributes (Performance, Reliability, Security, Scalability, Availability) referencing the text.
2. System constraints (Environmental, mandatory technologies, budget, timeline).
3. Compliance and governance requirements.

[Raw Document / Interview Text]
```

---

## 3. Architectural Drivers Identification
**Goal:** Separate items with high architectural weight that determine the form and style of the system (ASRs).

```text
Role: You are a Senior Software Architect responsible for setting the technical direction of the system.
Context: We are in the initial phase of requirements analysis and need to determine the core architectural decision-making center.
Task: Analyze the input text and extract the primary Architectural Drivers.
Constraints:
- List only items that change modular structure, data model, service boundaries, or technology choices.
- Leave aside routine options and simple CRUD operations.
Output Format:
1. Key Quality Attributes affecting architecture along with reasoning for their impact.
2. Organizational and technical constraints governing decisions.
3. Architecturally Significant Requirements (ASRs) that carry high risk.

[Document / Interview Text]
```

---

## 4. Converting Vague Statements to Measurable Metrics
**Goal:** Move away from qualitative and subjective descriptions to SLAs, SLOs, and quantifiable testable values.

```text
Role: You are a Performance Specialist and Site Reliability Engineer (SRE / Performance Architect).
Context: Stakeholders and business representatives have stated quality requirements using vague qualitative terms.
Task: Identify all qualitative and ambiguous claims that cannot be tested and define engineering-grade metrics for them.
Constraints:
- No vague expression should remain without a metric.
- Metrics must be concrete, numerical, and measurable (SLA/SLO).
Output Format:
A table including:
[Original Vague Statement | Related Quality Attribute | Architectural Risk if Ambiguous | Proposed Metric (SLA/SLO) | Testing Conditions & Measurement Method]

[Requirements Text]
```

---

## 5. Converting Raw Text to Structured User Stories & Scenarios
**Goal:** Standardize chaotic inputs into testable formats understandable by development and product teams.

```text
Role: You are a Technical Business Analyst proficient in agile methodologies.
Context: The text below is a summary of an interview or requirements discovery session with stakeholders.
Task: Convert the available information into precise User Stories along with standard test scenarios.
Constraints:
- Each story must represent independent business value.
- Scenarios must include Happy Path, Edge Cases, and Failure modes.
Output Format:
For each requirement:
- User Story: As a [Role], I want [Action], So that [Business Value].
- Acceptance Criteria in BDD format (Given-When-Then):
  * Main Scenario (Happy Path)
  * Error or Exception Scenario (Edge/Failure Scenario)

[Raw Interview Text / Meeting Notes]
```

---

## 6. Discovering Conflicts, Blind Spots, and Hidden Ambiguities
**Goal:** Challenge documentation and find gaps that would otherwise cause code rewrites or project failure later.

```text
Role: You are a Senior Architectural Auditor and Independent Technical Critic.
Context: These documents will serve as the basis for cost, timeline estimation, and architectural design.
Task: Inspect the text to uncover logical contradictions, knowledge gaps, operational blind spots, and risky assumptions.
Constraints:
- Highlight items whose ambiguity would halt implementation or force a redesign.
- Formulate a specific, challenging question for each flaw.
Output Format:
1. Textual Conflicts: Sections where claims contradict one another.
2. Blind Spots: Critical domains ignored (e.g., data concurrency, monitoring, disaster recovery).
3. Clarifying Questions to raise in a meeting with the PM/Stakeholders.

[SRS / Requirements Text]
```

---

## 7. AI Help vs. Risk Boundary Audit (Reality Check Prompt)
**Goal:** Prevent LLM hallucination errors and separate tasks requiring direct human judgment from routine AI assistance.

```text
Role: You are a realistic AI Advisor well-versed in the cognitive limitations of Large Language Models (LLMs).
Context: We intend to use AI to accelerate the requirements engineering process.
Task: Review the document below and draw the boundary between "safe reliance on AI" and "high-risk areas requiring human adjudication."
Constraints:
- Do not be overly optimistic; transparently state your inherent limitations in grasping unstated business contexts.
- Explicitly name potential hallucination risks within this domain.
Output Format:
1. Safe Zones (Safe for AI): Tasks where text data is sufficient for AI decision-making.
2. Danger Zones (Human-in-the-Loop): Areas where relying on AI could lead to flawed architecture and must be validated by architects and PMs.
3. Sanity Check Checklist for generated outputs.

[Requirements Text]
```
