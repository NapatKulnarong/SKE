
## **1. Modeling Domain Knowledge** 

- Ontology building starts by identifying important objects (concepts) and their relations in a domain. 
- **Example**: rice diseases — _Rice Variety_, _Disease_, _Pathogen_, _Symptom_, _Pesticide_, _Insect_ — linked by relations like `susceptible-to`, `cause`, `resistant-to`, `treatment`, `damage`, `prevent`, `eradicate`.
- **Goal:** Model concepts, relations, properties, axioms, and restrictions in a way that's easy to extend, share, and reuse.
---
## 2. What is Ontology?
![[knowledge_graph_company_project.svg|563]]
##### **Philosophy**: 
studies the nature of being/existence and categories of things
##### **CS/Information Science**: 
formal, machine-interpretable definition of types, properties, and relationships of entities
##### **Common ground**: 
study of **concepts**, their **properties**, and **relations**

---
## **3. Components of an Ontology**

| Component                   | Description                                          | Example   |
| --------------------------- | ---------------------------------------------------- | --------- |
| **Concepts (Classes)**      | Categories in the domain                             | Book      |
| **Properties**              | Features/attributes/relations of a concept           | hasWriter |
| **Restrictions**            | Rules, axioms, cardinality constraints on properties | —         |
| **Individuals (Instances)** | Specific members of a class                          | —         |
Ontology + Instances = **Knowledge Base**

---
## **4. Concept vs. Instance**
![[concept_vs_instance.png]]
**Concept:** General definition in the mind
**Instance:** A specific occurrence with identity

**Note:** same thing can be a _class_ or an _instance_ depending on intended use 
(e.g., "GoldenRetriever" as a class of all golden retrievers, vs. as an instance of class Dog denoting the breed)

---
## **5. Concepts vs. Labels**

![[conceptDefinition.png|431]]

- One concept can have **multiple labels** (**synonymy**, language-dependent)
- One label can refer to multiple concepts (**polysemy/homonymy**)
   E.g. "Tank" = armored vehicle, water tank, or fish tank
- **Rule**: never use the same label for different concepts _within one ontology_
---
## **6. Class Hierarchy (IS-A relation)**

- "A is a B" → B is superclass of A
- Subclasses **must inherit** all properties of their superclass
- **Example:** Fruit → Orange, Durian, Mango
---
## **7. Properties**

**Object property**: 
- Relates class/individual → class/individual 
- E.g. `hasWriter`: Novel → Writer
**Data property**: 
- Relates class/individual → literal data type 
- E.g., `hasTitle`: Novel → string

**Example:**
![[propertiesExample-chroma-2026-08-28T07-52-02-098Z.png]]

**Note:** Properties are inherited from superclass to subclass

**Example:**
![[propertiesInherit.png]]

---
## **8. Relations Types**

| Explanations                                                                                                                                                                                                                                                                    | Diagram                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Semantic relations**: link concepts (City → Country) <br>**Instance relations**: link instances; can be contextual, not generalizable (e.g., only Bangkok has `capitalCityOf` → Thailand)<br>**Terminological relations**: relationships between terms/labels (e.g., synonym) | ![[Screenshot 2026-08-28 at 15.03.12-chroma-2026-08-28T08-03-20-897Z.png]] |
**Example:**
![[ontologyComponents.png|591]]

---
## **9. Notation for Ontology Diagrams**
![[ontologyNotations.png]]
- Arrow (is-a) = vertical relation → superclass/subclass
- p/o (property/object) = horizontal relation → core class to constraint class
- a/o (attribute/object) = core class to data type
---
## **10. Ontology Example**

![[ontologyPencil1.png|483]]
![[ontologyPencil2.png|524]]

---

## **11. Broad Classes of Ontologies**

|Level|Description|Examples|
|---|---|---|
|**Upper Ontology**|Domain-independent, universal/common-sense concepts|DOLCE, SUMO, OpenCyc, BFO, GFO|
|**Mid-level Ontology**|Bridges upper and domain ontologies; intermediate detail|AIRS, MILO, STATO|
|**Domain Ontology**|Domain-specific concepts/relationships|Gene, Wine, Rice, Disease ontology, MESH|
|**Application/Local Ontology**|Specialized for one task/user viewpoint; no shared consensus needed|—|

---
## **12. Why Ontologies Matter**

- Clarify structure of knowledge
- Enable **knowledge sharing & reuse** (avoid repeating analysis)
- Enable **automated reasoning** (consistency checking, inference)
- Support **data integration** across systems/formats
- Used in AI for natural-language disambiguation and knowledge-based problem solving (diagnosis, planning, design)
---
## **13. Ontology Engineering Process**

1. Requirement specification
2. Development & refinement (knowledge elicitation, formalization)
3. Evaluation (test usefulness)
4. Maintenance (keep up with changing knowledge)
5. **Reasoning** — used throughout: test for contradictions, infer relations, check consistency, support integration across ontologies
---