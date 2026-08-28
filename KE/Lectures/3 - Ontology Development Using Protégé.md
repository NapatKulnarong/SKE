
**1. Recap: Ontology** Ontology = the study of things by explaining relationships between concepts within those things. Example: "Pencil" can be explained by:

- **Definition**: a writing utensil with a graphite lead embedded in a wooden shaft, used for writing and drawing
- **Parts**: Eraser, Ferrule, Body, Wood, Lead

**2. Components in an Ontology**

- **Classes/Concepts** — entities related to the topic (e.g., pencil, lead, pencil_body, ferrule, eraser)
- **Relations** — links between entities (e.g., _lead is a part of pencil_, _pencil is a writing_utensil_)
- **Individuals/Instances** — a specific example or single occurrence of a concept

**3. Ontology Relations**

- **Is-A relation**: forms a hierarchy tree (superclass–subclass); must be an acyclic graph
- **Property relation**: explains parts of a class
    - **Object property** (part-of): Class → Class (domain: Class, range: Class)
    - **Data property** (attribute-of): Class → Data (domain: Class, range: Data type)
- **TypeOf (instance-of) relation**: links a class to its individuals

**4. Protégé — Ontology Editor Tool**

- Developed by Stanford's Center for Biomedical Informatics Research
- Free, open-source ontology editor and knowledge management system
- Supports OWL 2 Web Ontology Language
- Direct in-memory connections to reasoners (HermiT, Pellet)
- Current version: 5.5.0+ — [download here](https://protege.stanford.edu/products.php#desktop-protege)

**5. RDF Triples** Every ontological statement follows a **Subject–Predicate–Object** triple pattern:

|Pattern|Example|
|---|---|
|Class IS-A Class|Dog IS-A Animal|
|Class(Domain) Object Property Class(Range)|Person hasParent Person|
|Class(Domain) Data Property Data(Range)|Person hasAge Integer|
|Class Typeof/Instance-of Individual|Dog typeof Rex|

**6. Specific Terms**

- **URI/IRI**: (Internationalized) Uniform Resource Identifiers — make classes/relations specific and referable
- Class names use a URI with an **RDF prefix**
- **RDF** (Resource Description Framework): an XML-based markup language for encoding ontological details

**7. What's Different From Earlier (Simple) Class Diagrams?**

- Ontologies link **many independent concepts**, not just one — forming multiple hierarchical trees (trees only capture is-a relations)
- Part-of and Attribute-of are relation _types_, not relation _names_ — each relation needs its own specific name
- Concepts can play different **roles** relative to one another; some conceptualized ideas may fall outside the ontology's scope

**8. Property Characteristics**

|Characteristic|Description|Example|
|---|---|---|
|Functional|Single value (object) per instance|`[John] has_StudentID [64119910]`|
|Inverse functional|Single value (subject) per instance; shared object implies same subject|`[TonyStark] has_Child [MorganS]`, `[Ironman] has_Child [MorganS]` → TonyStark = Ironman|
|Transitive|X relates Y, Y relates Z → X relates Z|`[John] ancestor [Jim]`, `[Jim] ancestor [Jame]` → John ancestor Jame|
|Symmetric|X relates Y → Y relates X|`[Thailand] border [Laos]` → Laos border Thailand|
|Asymmetric|X relates Y → Y cannot relate X the same way|`[Jane] birth_mother [Jim]`|

**9. Worked Example: Place Ontology**

- Classes: City, Country, Continent, Language
- Object properties: `isLocatedIn`, `isCapitalOf` (with `isCapitalOf` as a sub-property of `isLocatedIn`)
- Data properties: Language (string), Population (integer)
- Sample data tables provided for Southeast Asian countries/cities (capital, language, borders, population) — meant as source data for building the ontology in Protégé

**10. RDF/XML Example** Shows raw RDF markup for a CD record (`cd:artist`, `cd:country`, `cd:company`, `cd:price`, `cd:year`) wrapped in `<rdf:Description rdf:about="...">` tags — illustrating how individuals are serialized in RDF.

**11. In-Class Exercise: Pet Shop Ontology** The deck includes a hands-on exercise: _"You are a Pet Shop Owner — build an ontology in Protégé"_ based on a product catalog spanning:

- Food (dog/cat/bird food & treats)
- Grooming (shampoo, conditioner)
- Hygiene (litter)
- Reptile/aquarium supplies (UVB lighting, heat lamps, hygrometers, filters, air pumps, flea/tick treatments)

**12. Protégé GUI Walkthrough (annotated screenshots)**

- **Class hierarchy panel**: buttons for _Add sub-entity_, _Add entity at same level_, _Delete current entity_ — builds the class tree
- **Class description panel**: _Add more of this detail_ (+ button), _Delete the current detail_ (X), _Edit the current detail_ (pencil icon), _Add annotations_ (@ icon for comments/labels) — used for SubClassOf, EquivalentTo, and anonymous ancestor axioms
- **Ontology header/metrics panel**: shows Ontology IRI, version IRI, and live metrics (axiom count, class count, object/data property count, etc.)

---

Since the pet shop products list looks like the in-class exercise, want me to actually build out that ontology (class hierarchy, object/data properties) as a worked example, or walk through how you'd structure it in Protégé step by step?