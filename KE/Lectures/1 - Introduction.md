**1. Data → Information → Knowledge Hierarchy**

![[knowledgePyramid.png|520]]

- **Data**: Raw facts/figures, unorganized (e.g., a number, a picture)
- **Information**: Data with context — organized, categorized, calculated
- **Knowledge**: Understanding built from experience, insight, and contextualized information — the "know-how"

**2. Types of Knowledge**

- **Descriptive (Explicit) knowledge** — "knowing what": facts, theories; easy to formalize, store, and retrieve (e.g., parts of a car, parts of a plant)
- **Procedural (Tacit) knowledge** — "knowing how": skills, decision-making, execution; intuitive, experience-based, hard to codify
- **Acquaintance knowledge**: familiarity/perception of objects

**3. What is Knowledge Engineering?** A field within AI focused on capturing, representing, and using human expertise so computers can perform tasks that normally require human intelligence. In short: _making computers "think" like domain experts._

**4. Key Steps in Knowledge Engineering**

1. **Knowledge Acquisition** — gather facts, rules, and strategies from human experts
2. **Knowledge Representation** — convert that knowledge into a formal, computer-processable structure
3. **Inference and Reasoning** — build mechanisms to draw new conclusions from stored knowledge
4. **Explanation and Justification** — enable the system to explain its reasoning (important for trust)

**5. Why Reasoning/Inference Matters** Storing every possible fact explicitly is neither feasible nor desirable. Instead, systems should **infer new knowledge from a smaller set of base facts/relations** (e.g., deriving kinship relationships rather than storing every possible pair). Trade-off: more complex inference takes more computation time.

**6. Representation Requires Well-Defined Syntax & Semantics** To translate real-world knowledge into a representation reliably, meaning must be unambiguous. Example: does `Like(Jane, Pizza)` mean Jane likes _all_ pizza, or just the _specific_ pizza she ate? Clear semantics avoid this ambiguity.

**7. Process Flow** `Knowledge about the world` → (translate) → `Representation` → (reasoning) → `Conclusion` → (translate) → `Result`

**8. Real-World Applications**

- Expert/diagnosis systems (e.g., EasyDiagnosis, Isabel Symptom Checker)
- Recommendation systems
- Representing complex relational data (e.g., GRAMENE, MISO)

**9. Focus of This Course** Representing knowledge using **Ontology** — a formal, symbolic structure for knowledge that supports reasoning.