
**1. Recap: Ontology** is the study of things by explaining relationships between concepts within those things. Example: "Pencil" can be explained by:

- **Definition**: a writing utensil with a graphite lead embedded in a wooden shaft, used for writing and drawing
- **Parts**: Eraser, Ferrule, Body, Wood, Lead
---
**2. Components in an Ontology**

- **Classes/Concepts** — entities related to the topic (e.g., pencil, lead, pencil_body, ferrule, eraser)
- **Relations** — links between entities (e.g., _lead is a part of pencil_, _pencil is a writing_utensil_)
- **Individuals/Instances** — a specific example or single occurrence of a concept
---
**3. Ontology Relations**

- **Is-A relation**: forms a hierarchy tree (superclass–subclass); must be an acyclic graph
- **Property relation**: explains parts of a class
    - **Object property** (part-of): Class → Class (domain: Class, range: Class)
    - **Data property** (attribute-of): Class → Data (domain: Class, range: Data type)
- **TypeOf (instance-of) relation**: links a class to its individuals

| Pattern                                    | Example                 |
| ------------------------------------------ | ----------------------- |
| Class IS-A Class                           | Dog IS-A Animal         |
| Class(Domain) Object Property Class(Range) | Person hasParent Person |
| Class(Domain) Data Property Data(Range)    | Person hasAge Integer   |
| Class Typeof/Instance-of Individual        | Dog typeof Rex          |

---
**4. Protégé — Ontology Editor Tool**

- Developed by Stanford's Center for Biomedical Informatics Research
- Free, open-source ontology editor and knowledge management system
- Supports OWL 2 Web Ontology Language
- Direct in-memory connections to reasoners (HermiT, Pellet)
- Current version: 5.5.0+ — [download here](https://protege.stanford.edu/products.php#desktop-protege)
---
**5. RDF Triples** Every ontological statement follows a **Subject–Predicate–Object** triple pattern:
![[rdfTriples.png|606]]

---
**6. Specific Terms**

- **URI/IRI**: (Internationalized) Uniform Resource Identifiers — make classes/relations specific and referable
	- **URI**: Unique string identifying a resource (class, property, individual). Not always a real web address — just a unique ID.
	- **IRI**: Same as URI but supports Unicode (non-English characters). Protégé uses IRIs internally (OWL 2 standard).
	- Class names use a URI with an **RDF prefix**
- **RDF** (Resource Description Framework): an XML-based markup language for encoding ontological details

Every class/property/individual in Protégé gets an auto-generated IRI, e.g.:

```
http://www.semanticweb.org/torrent#Vendor
```

 **Why classes/relations must be specific & referable**: Ontologies need to be **unambiguous and shareable**. A unique IRI per class/relation lets you:

- Reference it from other ontologies without naming clashes
- Merge/link datasets using the same IRI
- Reuse or import definitions safelyv

**Class name = URI + prefix**: Full IRIs are long, so RDF uses **prefixes** as shorthand:

|Full IRI|Short form|
|---|---|
|`http://www.semanticweb.org/torrent#TOR`|`torrent:TOR`|
|`http://www.w3.org/...rdf-syntax-ns#type`|`rdf:type`|

Protégé shows the short name in the **Entities** tab, but it's bound to the ontology's base IRI/prefix (set when creating the ontology).

**RDF (Resource Description Framework)**: 
- **Data model** behind ontologies, everything is a **Subject–Predicate–Object** triple.
- RDF is serialized as **RDF/XML**, an XML-based markup format for storing these triples (also: Turtle, OWL/XML, Manchester syntax).
---
**7. Ontologies vs. Simple Class Diagrams**

*   **Concept Linking:** Ontologies connect many independent concepts across multiple hierarchical trees, rather than just one.
*   **Relation Naming:** "Part-of" and "Attribute-of" are treated as relation *types*, requiring each specific relation to have its own unique name.
*   **Dynamic Roles:** Concepts can play different roles relative to one another, and some ideas may fall outside the ontology's scope.
---

**8. Property Characteristics**

|                        | Description                                                             | Example                                                                                  |
| ---------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Functional**         | Single value (object) per instance                                      | `[John] has_StudentID [64119910]`                                                        |
| **Inverse functional** | Single value (subject) per instance; shared object implies same subject | `[TonyStark] has_Child [MorganS]`, `[Ironman] has_Child [MorganS]` → TonyStark = Ironman |
| **Transitive**         | X relates Y, Y relates Z → X relates Z                                  | `[John] ancestor [Jim]`, `[Jim] ancestor [Jame]` → John ancestor Jame                    |
| **Symmetric**          | X relates Y → Y relates X                                               | `[Thailand] border [Laos]` → Laos border Thailand                                        |
| **Asymmetric**         | X relates Y → Y cannot relate X the same way                            | `[Jane] birth_mother [Jim]`                                                              |

---
**9. Worked Example: Place Ontology**
![[placeOntology.svg|640]]

|                       |                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------ |
| **Classes**           | City, Country, Continent, Language                                                   |
| **Object properties** | `isLocatedIn`, `isCapitalOf` (with `isCapitalOf` as a sub-property of `isLocatedIn`) |
| **Data properties**   | Language (string), Population (integer)                                              |

---
**10. In-Class Exercise: Pet Shop Ontology** 

_"You are a Pet Shop Owner — build an ontology in Protégé"_ based on a product catalog spanning:

- Food (dog/cat/bird food & treats)
- Grooming (shampoo, conditioner)
- Hygiene (litter)
- Reptile/aquarium supplies (UVB lighting, heat lamps, hygrometers, filters, air pumps, flea/tick treatments)
---
