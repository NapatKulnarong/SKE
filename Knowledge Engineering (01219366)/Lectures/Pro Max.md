# KE Midterm — Pro Max (1st/2026)

**Exam topics:** Property Characteristics · Theoretical Questions · Writing Class Descriptions · Ontology Design · SPARQL · Inferred Class Hierarchy

**How to use this in 2 days**

- **Day 1:** Exam notation (must match the sheet) → Property characteristics → Class descriptions (Manchester) → Inferred hierarchy + OWA
- **Day 2:** Ontology design steps + GreenMart-style walkthrough → SPARQL prefixes/qualifiers/aggregates → Theoretical dump + drill at the end

Write **exactly** in the exam formats below. Markers will look for those boxes, not Protégé screenshots.

---

## 0. Exam instruction sheet (do this first)

### 0.1 Class–subclass tree

Indent under `Thing`. One class per line. Superclass is less indented than subclass.

```
Thing
    Animal
        Dog
        Cat
    Habitat
        Land
        Freshwater
        Seawater
```

**IS-A test:** every instance of the child **must** be an instance of the parent. Dog is-a Animal. Habitat is **not** a child of Animal.

### 0.2 Property table

| Class (Domain) | Property name | Range   | Characteristics       |
| -------------- | ------------- | ------- | --------------------- |
| Animal         | hasName       | string  | —                     |
| Animal         | hasBirthdate  | date    | Functional            |
| Animal         | hasHabitat    | Habitat | —                     |
| Habitat        | hasName       | string  | —                     |
| Habitat        | isHabitatOf   | Animal  | Inverse of hasHabitat |

**Object property:**  range is a **class**. 
**Data property**:     range is a **datatype** (`string`, `date`, `integer`, `decimal`, …).

> [!note] Why `hasBirthdate` is Functional (instruction-sheet example)
> Functional = a given subject has **at most one filler**. One animal has **one** birth date. If Rex is given two different dates, that is inconsistent.
> Two animals **may** share the same date — Functional does **not** mean “the date uniquely identifies the animal” (that would be **inverse functional**).
> The sheet leaves `hasName` as `—` because a name is not forced to be unique or single-valued unless the domain text says so.

### 0.3 Class descriptions (Manchester syntax they give you)

> [!important] SubclassOf vs EquivalentTo — one arrow vs two
> **SubclassOf** = one-way rule for members already in the class (**necessary** only) → **primitive** class. Reasoner will **not** dump other classes under it.
> **EquivalentTo** = full definition (**necessary and sufficient**) → **defined** class. Reasoner **will** classify anything that matches under it.
> Full explanation with pizza walkthrough → [[#3.2 Primitive vs defined (the exam hinge)]]

| Syntax they show | Meaning |
| --- | --- |
| `Prop1 only A` | universal / allValuesFrom — every filler is an `A` (includes **zero** fillers) |
| `Prop2 some B` | existential / someValuesFrom — **at least one** filler is a `B` |
| `Prop3 value k` | hasValue — at least one filler is the **individual** `k` |
| `Prop4 exactly 1 Integer` | exact cardinality to a datatype |
| `Prop5 min 2 C` | at least 2 fillers of class `C` |
| `Prop6 max 5 D` | at most 5 fillers of class `D` |
| `not (Prop7 some B)` | no `Prop7` to a `B` |
| `hasXX some Integer [>=400]` | datatype facet |

**SubclassOf example (Pizza)** — list each condition on its own line (exam sheet style):

```
A  SubclassOf
    Pizza
    Prop1 only A
    Prop2 some B
    Prop3 value k
    Prop4 exactly 1 Integer
    Prop5 min 2 C
    Prop6 max 5 D
    not (Prop7 some B)
```

**EquivalentTo example** — same pieces, but joined with `and` (that is the full definition):

```
A  EquivalentTo
    Pizza
    and (Prop1 only A)
    and (Prop2 some B)
    and (Prop3 value k)
    and (Prop4 exactly 1 Integer)
    and (Prop5 min 2 C)
    and (Prop6 max 5 D)
    and (not (Prop7 some B))
```

### 0.4 SPARQL table convention

- Each table = a schema + example rows.
- **Object Label** column = **subject / domain** of every other column.
- Other column headers = **property names**.
- Use `wdt:<property>` for simple properties, `wd:<class or instance>` for entities.

Qualifiers (complex values): `p:` statement, `ps:` main value, `pq:` qualifier. Details in §5.

---

## 1. Theoretical core (what they can ask in words)

### 1.1 Data → Information → Knowledge

| Layer | What it is | Example |
| --- | --- | --- |
| **Data** | Raw facts, unorganized | scores `85, 42, 90` |
| **Information** | Data in context (organized, calculated) | “section average = 65; 40% fail” |
| **Knowledge** | Know-how from experience + context | “fast teaching before midterm → need remedial” |

### 1.2 Types of knowledge

- **Descriptive / Explicit (“know-what”):** facts, theories; easy to store (parts of a car).
- **Procedural / Tacit (“know-how”):** skills, decisions; experience-based, hard to codify.
- **Acquaintance:** familiarity with objects.

### 1.3 Knowledge Engineering

Field of AI: capture, represent, and **use** human expertise so a computer can do expert tasks.

Four steps:

1. **Acquisition** — facts, rules, strategies from experts
2. **Representation** — formal structure a machine can process
3. **Inference / Reasoning** — draw new conclusions
4. **Explanation** — show why (trust)

**Why infer?** You do **not** store every possible fact (e.g. every kinship pair). Store a **small base** of relations and **infer** the rest. Cost: harder inference = more time.

**Syntax + semantics must be unambiguous.** `Like(Jane, Pizza)` — all pizza, or this pizza?

Flow: World → (translate) Representation → (reason) Conclusion → (translate) Result.

Course focus: **Ontology** as the representation.

### 1.4 What is an ontology?

- **Philosophy:** nature of being / categories of things.
- **CS:** formal, machine-interpretable types, properties, relations.
- Common: **concepts + properties + relations**.

**Ontology + individuals = Knowledge Base.**

| Component | Role | Example |
| --- | --- | --- |
| Classes / concepts | Categories | `Book`, `Pizza` |
| Properties | Attributes / relations | `hasWriter` |
| Restrictions / axioms | Rules on properties | `exactly 1`, `only` |
| Individuals | Specific members | `JaneEyre` |

**Concept vs instance:** concept = general definition; instance = one occurrence **with identity**. Same word can be class **or** individual depending on **intended use**.

Golden Retriever:

- If answers are **individual dogs** → class `GoldenRetriever`
- If answers are **breeds** → individual of `DogBreed`

**Concept vs label:** many labels per concept (synonymy, languages). One label can mean many concepts (polysemy: “tank”). **Never use the same class name for two concepts in one ontology.** Synonyms (`Dog` / `Canine`) are **one class**, extra names go in annotations.

### 1.5 IS-A hierarchy

- `A is-a B` ⇒ `B` superclass of `A`. Every instance of `A` is an instance of `B`.
- **Transitive.** Dog ⊂ Mammal ⊂ Animal ⇒ Dog ⊂ Animal.
- Subclasses **inherit all properties** of the superclass.
- **Acyclic.** A cycle A⊂B and B⊂A means A **equivalent** B.

### 1.6 Two property types

| | Connects | Example |
| --- | --- | --- |
| **Object property** | individual → individual (class ↔ class) | `hasWriter`: Novel → Writer |
| **Data property** | individual → literal | `hasTitle`: Novel → string |
| **Annotation property** | metadata on anything | `rdfs:comment` |

**Domain** = where the arrow starts. **Range** = where it lands.

Attach a property at the **most general class** that always has it (`hasColor` on `Wine`, not only on `RedWine`).

**Sub-property:** `hasTopping ⊏ hasIngredient`. Using the child implies the parent. Child may **narrow** domain and range (`Pizza` → `PizzaTopping`).

**Inverse:** `hasTopping` vs `isToppingOf`. Domain/range swap automatically.

### 1.7 RDF triple + IRI

Every statement: **Subject – Predicate – Object**.

- `ex:Alan  ex:studiesAt  ex:UniversityA`
- IRI uniquely names a class/property/individual (Unicode-capable URI). Prefixes shorten: `torrent:Vendor`.
- RDF is the data model; serializations include RDF/XML, Turtle, OWL/XML.

### 1.8 OWL individuals: no Unique Name Assumption

Two names may be the **same** individual until you say otherwise.

- `owl:sameAs` — same thing
- `owl:differentFrom` / `AllDifferent` — distinct

If `hasBirthMother` is **functional** and Jean has Peggy and Margaret as mothers → reasoner infers Peggy **sameAs** Margaret (unless they were declared different → **inconsistency**).

### 1.9 Disjoint classes

OWL classes **overlap by default**. An individual **can** be in two classes unless you disjoint them.

`DeepPanBase` disjoint `ThinAndCrispyBase` — a base cannot be both.

If two classes are disjoint, an individual in both → inconsistency.

**Enumerated class:** members listed exactly `{Sunday, …, Saturday}` under EquivalentTo.

### 1.10 Domain / range are axioms, not database constraints

If `hasTopping` domain = Pizza, range = PizzaTopping, and you assert `iceCream hasTopping almond`:

- Reasoner **infers** iceCream is a Pizza (and almond is a PizzaTopping).
- **Error only if** IceCream disjoint Pizza.

Multiple classes listed as range in Protégé = **intersection** of those classes. If those classes are disjoint, the range is empty → nothing can use the property consistently.

Some experts avoid heavy domain/range for this reason.

### 1.11 Broad ontology layers

```
Upper / foundational   DOLCE, SUMO, OpenCyc, BFO, GFO   (object, event, relation)
        ↓
Mid-level              MILO, AIRS, STATO                 (Time, Location)
        ↓
Domain                 Wine, Gene, Rice, MESH
        ↓
Application / local    one task, one viewpoint; no consensus required
```

Why ontologies: share structure, reuse knowledge, make assumptions explicit, separate domain knowledge from procedures, enable **reasoning**, integrate data.

### 1.12 Ontology engineering lifecycle

Requirement spec → develop/refine → evaluate → maintain. **Reasoning throughout:** consistency, implied relations, integration.

---

## 2. Property characteristics (exam topic 1)

Object properties can carry extra meaning. **Memorize the inference, not the slogan.**

| Characteristic | If P(a,b) then… | Canonical example | Typical on data property? |
| --- | --- | --- | --- |
| **Functional** | at most **one object** for a given subject | `hasStudentID`, `hasBirthMother`, `hasBase` | **Yes** (one name, one price) |
| **Inverse functional** | at most **one subject** for a given object | `isBirthMotherOf`; two fathers of Morgan ⇒ same person | No (needs two individuals) |
| **Transitive** | P(a,b) ∧ P(b,c) ⇒ P(a,c) | `ancestor`, `locatedIn`, `hasIngredient` | No |
| **Symmetric** | P(a,b) ⇒ P(b,a) | `border`, `hasSibling` | No |
| **Asymmetric** | P(a,b) ⇒ **not** P(b,a) | `birthMother`, `hasChild` | No |
| **Reflexive** | every individual is P-related to **itself** | `knows` (everyone knows themselves) | No |
| **Irreflexive** | nothing is P-related to **itself** | `hasMother` | No |

### 2.1 Functional — single filler per subject

For a given subject, **≤ 1 object**.

- Jean `hasBirthMother` Peggy **and** Margaret ⇒ Peggy = Margaret.
- `hasBirthdate` on Animal (exam sheet): one animal, one date. Data properties **can** be Functional.
- Pizza: `hasBase` is functional (one base). `hasTopping` is **not**.

> [!note] Why `hasBirthdate` is Functional
> One animal → at most **one** date. Data properties **can** be Functional.
> Not inverse functional: many animals can share a date. Inverse functional would mean that date belongs to at most **one** animal.

### 2.2 Inverse functional — single subject per object

TonyStark `hasChild` MorganS and Ironman `hasChild` MorganS ⇒ TonyStark = Ironman.

If P is functional, **inverse(P)** is inverse-functional.

### 2.3 Transitive

John ancestor Jim, Jim ancestor Jame ⇒ John ancestor Jame.

**Hard keys from pizza lecture:**

- If P is **transitive**, it **cannot** be **functional**.
- If P is transitive, **inverse(P) is also transitive**.

`hasIngredient` is the usual transitive candidate. `hasBase` / `hasTopping` usually **not** (a topping of a topping is not a topping of the pizza in the same sense they modeled, unless they set it that way).

### 2.4 Symmetric vs asymmetric

Thailand `border` Laos ⇒ Laos `border` Thailand.

Jane `birthMother` Jim ⇒ Jim **cannot** `birthMother` Jane.

Symmetric ≠ “can go both ways sometimes”. It **always** infers the reverse.

### 2.5 Reflexive vs irreflexive

- Reflexive: `sameTaste` / `knows` — George `knows` George **must** hold.
- Irreflexive: nobody `motherOf` themselves.

### 2.6 Exam writing

In the Characteristics column write the names: `Functional`, `InverseFunctional`, `Transitive`, `Symmetric`, `Asymmetric`, `Reflexive`, `Irreflexive`, or `Inverse of X`. Combine if needed: `Functional, Inverse of belongsTo`.

**Pick characteristics from the English:**

| Phrase in the story | Characteristic |
| --- | --- |
| unique code / exactly one name / exactly one company | Functional (data or object) |
| each X belongs to exactly one Y (from Y’s side: Y is identified by X) | Inverse functional on the “has” direction, or Functional on `belongsTo` |
| nested location / part-of chains | Transitive |
| shares a border / sibling / married (if modeled symmetric) | Symmetric |
| parent, birth mother, contains | Asymmetric (and usually irreflexive) |

---

## 3. Writing class descriptions (exam topic 3)

A restriction is an **anonymous class**: “the set of individuals that satisfy this relationship.”

Putting it under **SubclassOf** vs **EquivalentTo** changes what the reasoner may infer.

### 3.1 Three families of restrictions

**1. Quantifiers**

- `hasTopping some MozzarellaTopping` — at least one mozzarella topping.
- `hasTopping only VegetableTopping` — **no** topping outside Vegetable (and **zero toppings still OK**).

**2. Cardinality**

- `hasTopping min 2 PizzaTopping`
- `hasTopping max 5 PizzaTopping`
- `hasTopping exactly 4 CheeseTopping`

**3. hasValue**

- `hasCountryOfOrigin value Italy` — Italy is an **individual**, not a class.

Datatype:

- `hasCalorificContentValue some Integer`
- `hasCalorificContentValue some Integer [>=400]`
- `hasPrice exactly 1 xsd:decimal`

### 3.2 Primitive vs defined (the exam hinge)

This is the one idea that unlocks **Writing Class Descriptions** and **Inferred Class Hierarchy**.

#### The everyday analogy

| | What you said | What follows |
| --- | --- | --- |
| **Necessary only** (SubclassOf) | “If you are a KU student, you must have a student ID.” | Having an ID does **not** make you a KU student. |
| **Necessary + sufficient** (EquivalentTo) | “You are a KU student **exactly when** you are a person enrolled at KU.” | Anyone who matches that **is** a KU student. |

- **Necessary** = a rule members must obey (**one arrow:** class → conditions).
- **Sufficient** = matching the conditions is enough to get in (**other arrow:** conditions → class).
- **Defined** = both arrows. **Primitive** = only the first arrow.

#### Same idea on pizza

**Case A — SubclassOf (primitive)**

```
CheesyPizza  SubclassOf
    Pizza
    hasTopping some CheeseTopping
```

Read **only this way:**

> If something is already a CheesyPizza → then it is a Pizza and it has cheese.

You did **not** say the reverse. So if MargheritaPizza has mozzarella, the reasoner **will not** put Margherita under CheesyPizza. You only described members; you did not define “what counts as CheesyPizza.”

**Case B — EquivalentTo (defined)**

```
CheesyPizza  EquivalentTo
    Pizza
    and (hasTopping some CheeseTopping)
```

Read **both ways:**

1. If it is a CheesyPizza → Pizza with cheese. *(necessary)*
2. If it is a Pizza with cheese → it **is** a CheesyPizza. *(sufficient)*

Now Margherita **is** classified under CheesyPizza. The `and` just glues the pieces of that definition together.

#### What the reasoner does (exam)

| You write | Class type | Reasoner puts other classes **under** it? |
| --- | --- | --- |
| `SubclassOf` | **Primitive** | **No** |
| `EquivalentTo` | **Defined** | **Yes** — anything that matches the whole expression |

> [!tip] Exam rule of thumb
> Want the reasoner to **collect** children under a class (VegetarianPizza, ProteinSalad, …)? That class **must** be `EquivalentTo`.
> Only describing what a named pizza must have? `SubclassOf` is enough.

Protégé: “Convert to defined class” moves rows from SubclassOf → EquivalentTo (same conditions, now both arrows).

### 3.3 Universal (`only`) traps

1. **`only` includes empty.** `hasTopping only Veg` = (all toppings are veg) **OR** (no toppings). Always pair with `some` if the thing **must exist**:

```
VegetarianPizza  EquivalentTo
    Pizza
    and (hasTopping only (CheeseTopping or VegetableTopping))
    and (hasTopping some (CheeseTopping or VegetableTopping))
```

2. **Two `only`s = intersection of fillers.**  
   `hasTopping only Cheese` **and** `hasTopping only Vegetable` ≡ `hasTopping only (Cheese and Vegetable)`. If Cheese disjoint Vegetable, **no toppings allowed**. Correct: **one** only with **union**: `only (Cheese or Vegetable)`.

3. **Closure axiom.** Existentials say what you **have**; they do **not** say you have **nothing else** (Open World). To close:

```
hasTopping some A
hasTopping some B
hasTopping only (A or B)     ← closure
```

Closure filler = **union of the existential fillers**.

### 3.4 Cardinality + only

`FourCheesePizza`:

- `hasTopping exactly 4 CheeseTopping` — exactly four cheese toppings, **other topping types still possible** (OWA).
- Need **only cheese** as well: `hasTopping only CheeseTopping` plus `exactly 4 CheeseTopping` (or `exactly 4 PizzaTopping` and `only CheeseTopping`).

### 3.5 Negation

`not (hasTopping some MeatTopping)` = no meat topping asserted as possible — used for vegetarian **if** you also close the world / disjoint meats. Under OWA, missing meat is **not** “no meat”.

### 3.6 Worked descriptions (pizza exercise style)

**Hawaiian** — at least pineapple, ham, tomato sauce, cheese (primitive unless they ask defined):

```
HawaiianPizza  SubclassOf
    NamedPizza
    hasTopping some Pineapple
    hasTopping some HamTopping
    hasTopping some TomatoSauceTopping
    hasTopping some CheeseTopping
```

**Spinach Deluxe** — **must have only** spinach and cheese:

```
SpinachDeluxePizza  SubclassOf
    NamedPizza
    hasTopping some SpinachTopping
    hasTopping some CheeseTopping
    hasTopping only (SpinachTopping or CheeseTopping)
```

**Spicy Meat Lover** — min 4 meats, one is ham, spiciness Hot (Hot = individual → `value`):

```
SpicyMeatLoverPizza  SubclassOf
    NamedPizza
    hasTopping min 4 MeatTopping
    hasTopping some HamTopping
    hasSpiciness value Hot
```

**Super Cheese** — only 5 kinds of cheese:

```
SuperCheesePizza  SubclassOf
    NamedPizza
    hasTopping exactly 5 CheeseTopping
    hasTopping only CheeseTopping
```

### 3.7 Translate English → Manchester (cheat)

| English | Write |
| --- | --- |
| at least one / must have / contains | `some` |
| only / cannot have anything except / may have only these types | `only` (+ `some` if must exist) |
| exactly n | `exactly n` |
| at least n / must have ≥ n | `min n` |
| at most n / no more than | `max n` |
| this specific country/person/hot | `value Individual` |
| price ≥ 0 | `some decimal [>=0]` (or `[>=0]` on the facet they showed) |
| unique / exactly one | `exactly 1` **and** property Functional |
| must not have B | `not (P some B)` |
| belongs to exactly one | `P exactly 1 C` |
| optional types, but if present only these | `P only (A or B or C)` without forcing `some` of the optional ones; force `some` on **required** types |

---

## 4. Ontology design (exam topic 4)

### 4.1 Three laws

1. **No single correct model.** Use + extensions decide.
2. Concepts ≈ **nouns**; relations ≈ **verbs** in domain sentences.
3. **Iterate.** Rough pass → refine with competency questions.

### 4.2 Seven steps (Noy & McGuinness — write these if asked)

1. **Domain and scope**  
   What domain? What for? Who maintains? **Competency questions** (do **not** name instances in CQs).
2. **Reuse** existing ontologies if needed (DBpedia, UNSPSC, …).
3. **Enumerate terms** — dump nouns/verbs; ignore overlap at first.
4. **Classes + hierarchy** — terms with independent existence. Top-down / bottom-up / middle-out. All equally valid.
5. **Properties** — remaining terms; attach at most general class.
6. **Facets** — domain, range, cardinality, value type, characteristics.
7. **Instances** — pick class, create individual, fill properties.

Steps 4 and 5 are intertwined and are the **main** design work.

### 4.3 Competency questions (exam design)

Good CQs drive classes **and** restrictions:

- “I want stir-fried but I cannot eat this meat — what can I order?”
- → class `StirFriedDish`, property `hasMeat` / `containsIngredient`, defined class `VegetarianDish`, disjoint meats.

Bad CQ: “What is Nida’s salary?” — that is an **instance** lookup, not a schema need, unless the exam table is SPARQL.

### 4.4 Hierarchy correctness (tips lecture)

**IS-A / kind-of only.** Every Dog is an Animal. Do **not** put `Student` as sibling of `Male` under `Human` as if they were the same cut — those are **different partition criteria** (sex vs role). Use **multiple inheritance** or separate trees (Person → Male/Female **and** Person → Student/Professor).

**Siblings:** same generality, same “line”. Coffee and Alcoholic are **not** siblings under Beverage. Alcoholic vs NonAlcoholic are.

**Counts:** 1 direct subclass ⇒ incomplete or wrong. \> ~12 ⇒ add intermediate classes (Red/White/Rosé under Wine).

**No cycles.**

**Multiple inheritance is allowed** (Port is RedWine **and** DessertWine). Watch conflicting restrictions from parents.

**When to add a class vs a property value**

Add a **class** if:

- extra properties, different restrictions, or different relations
- the distinction matters for **other** restrictions (red vs white paired with different food)
- experts treat them as different **kinds**
- navigation / terminology hierarchy (diseases)

Keep a **property value** if:

- same behavior for all values (wine-label factory: color is just a slot)
- instances would **jump classes often** (do **not** make `ChilledWine` a class — chilling is extrinsic)

**Instance vs class:** most specific answers to CQs → individuals. Soft-drink recommender: `Cola` is an instance. Inventory of bottles: each bottle is an instance, `Cola` is a class.

**Scope:** not all knowledge — **at most one extra level** of general/specific beyond the app.

### 4.5 Naming (lose easy marks if messy)

- Classes: `PascalCase`, singular (`Wine` not `Wines` unless you pick plural **everywhere**)
- Properties: `camelCase` with `has` / `is` / `of` (`hasMaker`, `makerOf`)
- No `Class`/`Property` in the name; avoid abbreviations
- Sibling names consistent: `RedWine` + `WhiteWine`, not `RedWine` + `White`
- One namespace: don’t name a class and a property the same if the tool forbids it

### 4.6 Design from a table (Exercise 2 / Song)

Columns that repeat as **types of things** → classes. Columns that are attributes → data properties. Columns that point at other things → object properties.

Song table:

**Classes:** `Song`, `Artist`, `Album`, `SongWriter`, `RecordLabel`, `Style` (style could also be a string — if CQs compare styles as kinds, make a class).

**Data properties:** `hasReleaseDate` (Song → date), `hasLength` (Song → string or time).

**Object properties:** `performedBy` Song → Artist; `onAlbum` Song → Album; `writtenBy` Song → SongWriter; `releasedOnLabel` Song → RecordLabel; `artistLabelAtRelease` / `currentLabel` Artist → RecordLabel; `hasStyle` Song → Style.

“Artist’s label during song released” vs “current label” = **two properties**, not one.

### 4.7 Full exam-style design: GreenMart

Use this as the **template** for any domain paragraph.

**Competency questions implied by the text**

- Which company owns a branch? What is the branch code?
- Which departments must a branch have? Which are optional?
- Which product categories does a department offer? (≥5)
- Can this department stock this product?
- Must Household Goods stock a cleaning product?
- Unique name and non-negative price of a product?

**Class tree**

```
Thing
    SupermarketCompany
    Branch
    Department
        DryFoodDepartment
        HouseholdGoodsDepartment
        FreshProduceDepartment
        BeverageDepartment
        PersonalCareDepartment
    ProductCategory
    Product
        FoodProduct
            DryFoodProduct
            FreshProduceProduct
            BeverageProduct
        NonFoodProduct
            HouseholdGoodsProduct
                HouseholdCleaningProduct
            PersonalCareProduct
```

Disjoint (same row = disjoint):  
`FoodProduct, NonFoodProduct`  
`DryFoodDepartment, HouseholdGoodsDepartment, FreshProduceDepartment, BeverageDepartment, PersonalCareDepartment`  
`DryFoodProduct, FreshProduceProduct, BeverageProduct, HouseholdGoodsProduct, PersonalCareProduct`

**Properties**

| Domain | Property | Range | Characteristics |
| --- | --- | --- | --- |
| SupermarketCompany | hasBranch | Branch | Inverse of belongsToCompany |
| Branch | belongsToCompany | SupermarketCompany | Functional |
| Branch | hasBranchCode | string | Functional |
| Branch | hasDepartment | Department | Inverse of belongsToBranch |
| Department | belongsToBranch | Branch | Functional |
| Department | offersCategory | ProductCategory | — |
| Department | stocks | Product | Inverse of stockedBy |
| Product | stockedBy | Department | Functional |
| Product | hasCategory | ProductCategory | Functional |
| Product | hasName | string | Functional |
| Product | hasPrice | decimal | Functional |

**Class descriptions**

```
Branch  SubclassOf
    belongsToCompany exactly 1 SupermarketCompany
    hasBranchCode exactly 1 string
    hasDepartment some DryFoodDepartment
    hasDepartment some HouseholdGoodsDepartment
    hasDepartment only (DryFoodDepartment or HouseholdGoodsDepartment
                        or FreshProduceDepartment or BeverageDepartment
                        or PersonalCareDepartment)

Department  SubclassOf
    belongsToBranch exactly 1 Branch
    offersCategory min 5 ProductCategory

DryFoodDepartment  SubclassOf
    Department
    stocks only DryFoodProduct

HouseholdGoodsDepartment  SubclassOf
    Department
    stocks only HouseholdGoodsProduct
    stocks some HouseholdCleaningProduct

FreshProduceDepartment  SubclassOf
    Department
    stocks only FreshProduceProduct

BeverageDepartment  SubclassOf
    Department
    stocks only BeverageProduct

PersonalCareDepartment  SubclassOf
    Department
    stocks only PersonalCareProduct

Product  SubclassOf
    stockedBy exactly 1 Department
    hasCategory exactly 1 ProductCategory
    hasName exactly 1 string
    hasPrice exactly 1 decimal
    hasPrice some decimal [>=0]
```

If they want **defined** departments (reasoner classifies a department by what it stocks), move the `stocks only …` lines to EquivalentTo **and** keep `Department` in the conjunction.

### 4.8 Menu ontology (Thai dishes) — design pattern

**Do not** make one class per dish if CQs are about **filters**. Dishes are **individuals** (or named subclasses if they want inferred grouping).

```
Thing
    Dish
        SpicySaladDish
        StirFriedDish
        SoupDish
        FriedDish
    Ingredient
        Meat
            Chicken
            Pork
            Prawn
            Fish
        Vegetable
    SpiceLevel
```

Properties: `hasCookingMethod`, `containsMeat` (Dish → Meat), `isSpicy` (boolean or `hasSpiceLevel`).

Defined classes that answer CQs:

```
VegetarianDish  EquivalentTo
    Dish
    and (not (containsMeat some Meat))
    and (containsMeat only Nothing)     # or: not (containsMeat some Meat) + closure

SeafoodDish  EquivalentTo
    Dish
    and (containsMeat some (Prawn or Fish))

ChickenStirFry  EquivalentTo
    StirFriedDish
    and (containsMeat some Chicken)
```

Spicy vs non-spicy: data property `isSpicy` boolean, or `hasSpiceLevel value Spicy`.

---

## 5. SPARQL (exam topic 5)

### 5.1 What SPARQL is

Query language for **RDF triples**, not SQL tables. You match **graph patterns** with variables (`?x`). Supports FILTER, OPTIONAL, JOIN (just more triples), GROUP BY / aggregates, ORDER BY, LIMIT.

### 5.2 Wikidata prefixes (instruction sheet)

| Prefix | Means | Example |
| --- | --- | --- |
| `wd:` | entity (class or instance) | `wd:Thai`, `wd:Q5` (human) |
| `wdt:` | **direct** property (simple fact) | `?a wdt:Nationality ?b` |
| `p:` | **statement node** (when the value has extra fields) | `wd:Nida_Somsuk p:JobName ?jobstruct` |
| `ps:` | **main value** of that statement | `?jobstruct ps:JobName ?a` → Professor / Director |
| `pq:` | **qualifier** on that statement | `?jobstruct pq:JobPlace ?b`, `pq:Salary ?c` |

**Rule:** one cell, one value → `wdt:`. One cell that nests JobPlace + Salary → `p:` + `ps:` + `pq:`.

Exam example: Nida has two jobs.

```
wd:Nida_Somsuk  p:JobName  ?jobstruct .
?jobstruct  ps:JobName  ?a .      # Professor ; Director
?jobstruct  pq:JobPlace ?b .      # Kasetsart_U ; OSC
?jobstruct  pq:Salary   ?c .      # 50000 ; 35000
```

Simple facts:

```
?a  wdt:Nationality  wd:Thai .
?a  wdt:Gender       ?g .
```

Skyscraper table: Object Label = tower. Columns Instance_of, Country, City, Height.

```
SELECT ?typeLabel ?countryLabel ?cityLabel ?height
WHERE {
  wd:Shanghai_Tower
    wdt:Instance_of ?type ;
    wdt:Country ?country ;
    wdt:City ?city ;
    wdt:Height ?height .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
}
```

Output: `skyscraper  China  Shanghai  632`

`SERVICE wikibase:label` fills `?xLabel` from `?x`. Always copy this if they ask for readable names.

### 5.3 Syntax pieces to copy

**Date filter**

```
FILTER("2015-01-01"^^xsd:dateTime <= ?dob && ?dob < "2016-01-01"^^xsd:dateTime)
```

**Path:** instance of a class **or subclass**

```
?city wdt:P31/wdt:P279* wd:Q515 .
```

`P31` = instance of, `P279` = subclass of, `*` = zero or more. Exam tables may write `Instance_of` instead of P31 — **use the column name they printed**.

**Blank node for qualifiers** (US presidents example):

```
?person p:P39 [ ps:P39 wd:Q11696 ; pq:P580 ?start ; pq:P582 ?end ] .
```

**OPTIONAL** — don’t drop the row if missing.

**BIND**

```
BIND(YEAR(?death) - YEAR(?birth) AS ?death_age)
```

**Aggregates** (must GROUP BY every non-aggregated SELECT variable)

| Function | Use |
| --- | --- |
| `COUNT(?x)` / `COUNT(*)` | how many |
| `SUM` `AVG` | numbers only |
| `MIN` `MAX` | numbers or lexical |
| `SAMPLE` | any one value |

```
SELECT ?country (MAX(?population) AS ?maxPopulation)
WHERE {
  ?city wdt:P31/wdt:P279* wd:Q515 ;
        wdt:P17 ?country ;
        wdt:P1082 ?population .
}
GROUP BY ?country
```

```
SELECT ?material ?materialLabel (COUNT(?painting) AS ?count)
WHERE {
  ?painting wdt:P31/wdt:P279* wd:Q3305213 ;
            p:P186 ?matstruct .
  ?matstruct ps:P186 ?material ;
             pq:P518 wd:Q861259 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
}
GROUP BY ?material ?materialLabel
ORDER BY DESC(?count)
LIMIT 20
```

```
ORDER BY DESC(?numberOfLanguage)
LIMIT 100
```

### 5.4 How to write a query from an exam table (algorithm)

1. Identify **Object Label** → subject variable or `wd:Name`.
2. Each other column → `wdt:ColumnName`.
3. If a cell contains **nested fields** (JobPlace, Salary, start/end) → that column uses `p:` / `ps:` / `pq:`.
4. SELECT the variables you need + `?fooLabel` if labels requested.
5. FILTER / OPTIONAL / BIND as the question says.
6. If “how many / maximum / per country” → aggregate + GROUP BY.
7. Add label SERVICE if they used `?xLabel` in the sample.

### 5.5 Mini patterns

People with a job in a place:

```
SELECT ?person ?personLabel ?job ?place
WHERE {
  ?person wdt:Instance_of wd:Person ;
          p:JobName ?js .
  ?js ps:JobName ?job ;
      pq:JobPlace ?place .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
}
```

Filter salary:

```
  ?js pq:Salary ?sal .
  FILTER(?sal >= 40000)
```

---

## 6. Inferred class hierarchy (exam topic 6)

### 6.1 Asserted vs inferred

- **Asserted:** the tree **you** drew (SubclassOf you typed).
- **Inferred:** what the **reasoner** adds by subsumption.
- Computing that tree = **classifying** the ontology.

Protégé 5: Reasoner → Start reasoner. Inconsistencies show as unsatisfiable (red).

**Benefit:** large ontologies + **multiple inheritance** maintained by the machine, fewer human errors.

### 6.2 The golden rule

> A reasoner **never** places a class as subclass of a **primitive** class (only necessary conditions).  
> It **does** classify under **defined** classes (EquivalentTo / N+S).

Why: matching φ is enough only when φ is **sufficient**. If VegetarianPizza is only “if you are one, you have no meat” that does **not** mean “if you have no meat, you are one”.

### 6.3 Open World Assumption (OWA)

OWL does **not** assume “if I didn’t say it, it’s false”. Unsaid = **unknown**, not false.

`MyPizza` has asparagus, mushroom, spinach, mozzarella (all veg) — still **not** classified as VegetarianPizza, because it **might** also have pepperoni that nobody mentioned.

**Fix: closure axiom** `hasTopping only (Asparagus or Mushroom or Spinach or Mozzarella)`.

Closed-world databases: missing meat ⇒ vegetarian. OWL: missing meat ⇒ maybe meat.

### 6.4 How to compute inferred parents by hand

For each class C, look at every **defined** class D ≡ φ:

C ⊑ D if **every** individual of C is **forced** to satisfy φ.

Checklist:

1. Mark which classes are **EquivalentTo** (only those can gain inferred children).
2. Expand φ: some / only / min / not / and / or.
3. Use **disjoint** tables: if C has `some Meat` and D has `only Veg` and Meat disjoint Veg → C **cannot** be under D (possibly unsatisfiable if C also claims to be D).
4. `some A` does **not** imply `only A`.
5. `only A` does **not** imply `some A` (empty allowed).
6. `some A` and `some B` ⇒ at least those two; still not closed.
7. If C ≡ (Pizza and some Cheese) and D ≡ (Pizza and some Cheese) then they are equivalent (same class).
8. If C is **stricter** than D (C has all of D’s conditions plus more) and D is defined → C inferred subclass of D.

**Slide exercise (pattern):**

- Pizza1 ≡ only (Veg or Cheese) **and** some Veg **and** some Cheese  
- Pizza2 ≡ some Cheese  
- Pizza3 ≡ some Veg **and** some Meat  
- Pizza4 ≡ some Veg  

If all are **defined**:

- Pizza1 ⊑ Pizza2 (has cheese) and Pizza1 ⊑ Pizza4 (has veg). Pizza1 is **not** ⊑ Pizza3 (no meat required; only veg/cheese forbids meat if Veg,Cheese,Meat disjoint).
- Pizza3 ⊑ Pizza4 (has veg). Pizza3 **not** ⊑ Pizza2 unless it also has cheese (not said). Pizza3 **not** ⊑ Pizza1 (has meat).
- Pizza2 **not** under Pizza1 (may have only cheese, may have meat).
- Pizza4 **not** under Pizza1 (may have meat).

Output format they want:

```
Parent Class              Inferred subclasses
ClassA                    ClassB, ClassC, ClassD
```

List **only inferred** (not the ones already drawn as asserted children, unless the sheet says list all). If unsure, list every child the reasoner would show in the inferred tree under that parent.

### 6.5 Salad exercise method

You will get: (1) disjoint rows, (2) a tree of salad names, (3) restrictions on those salads (diagram).

1. Copy disjoint groups (Dressing vs Meat vs Topping vs …; Beef vs Chicken vs …).
2. For each **defined** salad (VegetarianSalad, ProteinSalad, HealthySalad, …), write φ in one line.
3. For each named salad (BeefSalad, MixVeggieSalad, …), write its asserted toppings.
4. Ask: given disjointness + closure/`only`, does this named salad **always** satisfy φ?
5. Fill the table Parent → inferred children.

Typical inferred stories:

- Mix veggie / green salad under VegetarianSalad **if** `only Vegetable` (or not some Meat/Egg) **and** VegetarianSalad is defined that way.
- Beef/Chicken/Pork/Seafood under NonVegetarian / Protein **if** those are defined with `some Meat`.
- LowCalSalad under HealthySalad if Healthy ≡ `hasDressing only LowCalDressing` (or calorie facet) and LowCal is defined equivalently or stricter.

Without the picture, **do not memorize salad names** — memorize the **algorithm**.

### 6.6 Unsatisfiable classes (red)

C is empty / inconsistent if its conditions contradict (some Meat **and** only Vegetable, with Meat disjoint Vegetable). Inferred hierarchy: unsatisfiable ⊑ Nothing. On the exam, say the class **cannot have any instance**.

---

## 7. 90-second exam playbook

1. **Read the instruction page** — copy tree indent, property table headers, Manchester keywords.
2. **Design / descriptions:** nouns → classes; “exactly one / unique” → Functional + `exactly 1`; “at least” → `some` / `min`; “only these types” → `only` + union; “must exist and only” → `some` **and** `only`; nested location → Transitive; mutual border → Symmetric.
3. **Defined vs primitive:** if they later ask inferred hierarchy, classes that should **collect** children **must** be EquivalentTo.
4. **SPARQL:** Object Label = subject; nested cell = `p/ps/pq`; count/max = GROUP BY.
5. **Inference:** only defined parents; apply OWA (need `only` to close); disjoint to block/unsatisfy.

---

## 8. Drill (cover answers; write yours first)

**1. Four KE steps + why we infer instead of storing all facts.**  
Acquisition, representation, inference, explanation. Storing all pairs (kinship) is infeasible; infer from a small base; cost is runtime.

**2. Class vs individual for “Cola”.**  
Recommender of drink **types** → Cola is an individual. Store inventory of bottles → Cola is a class, bottles are individuals.

**3. Why not class `ChilledWine`?**  
Chilling is extrinsic; the same bottle would constantly leave the class.

**4. Functional vs inverse functional, one sentence each.**  
Functional: ≤1 object per subject (`hasBirthMother`). Inverse functional: ≤1 subject per object (`hasChild` identifying Tony = Ironman).

**5. Can transitive be functional?**  
No.

**6. Data property: which characteristics?**  
Functional yes. Inverse-functional / transitive / symmetric: no.

**7. `hasTopping only Veg` without `some`.**  
Pizzas with **no toppings** are included.

**8. Two `only` restrictions on the same property.**  
Intersection of fillers; if fillers disjoint, nothing can have a topping.

**9. Domain of `hasTopping` is Pizza; IceCream disjoint Pizza; iceCream hasTopping x. What happens?**  
Inconsistency.

**10. Same, but not disjoint.**  
iceCream inferred to be a Pizza.

**11. Why won’t MyPizza classify under VegetarianPizza?**  
OWA — other toppings still possible. Add closure `only (union of its toppings)`. Reasoner also needs VegetarianPizza **defined**.

**12. Primitive vs defined.**  
Primitive: SubclassOf / necessary. Defined: EquivalentTo / N+S. Only defined classes receive inferred subclasses.

**13. Write SPARQL: all people with nationality Thai.**  

```
SELECT ?a ?aLabel WHERE {
  ?a wdt:Nationality wd:Thai .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
}
```

**14. When `wdt:` vs `p:`?**  
`wdt:` simple triple. `p:` when you need qualifiers (`pq:`) or the statement’s main value via `ps:`.

**15. GreenMart: write the Branch restrictions from memory.**  
exactly 1 company, exactly 1 code, some DryFoodDept, some HouseholdDept, only the five department types.

**16. Sibling mistake.**  
Coffee and Alcoholic under Beverage — different generality / different cut. Use Alcoholic / NonAlcoholic, then Coffee under NonAlcoholic.

**17. OWL Unique Name Assumption?**  
**Does not** use UNA. Names may co-refer; use sameAs / differentFrom.

**18. Disjoint default?**  
Classes overlap until disjointed.

**19. `?x wdt:P31/wdt:P279* wd:Q515` meaning?**  
?x is an instance of City or of any subclass of City.

**20. Super Cheese Pizza description.**  
`hasTopping exactly 5 CheeseTopping` and `hasTopping only CheeseTopping`.

---

## 9. One-page memory dump (night before)

```
IS-A = every child instance is parent instance; inherit properties; no cycles
Object P: class→class    Data P: class→literal
Functional: 1 object        InverseFunc: 1 subject
Transitive: chain           Transitive ⇏ Functional; inverse also transitive
Symmetric: reverse          Asymmetric: reverse forbidden
Reflexive: P(a,a)           Irreflexive: not P(a,a)
Domain/range = axioms → infer type; error only if disjoint
some = ≥1     only = all fillers (∅ ok)     value = that individual
min / max / exactly
SubclassOf = necessary = primitive = NO inferred children
EquivalentTo = N+S = defined = YES inferred children
Closure = only (OR of existentials)
OWA: unsaid ≠ false
UNA: two names ≠ two things
SPARQL: Object Label = subject
wdt: fact    p: statement    ps: value    pq: qualifier
COUNT SUM AVG MIN MAX SAMPLE + GROUP BY
P31/P279* = instance of class-or-subclass
```

Design recipe: CQ → classes (independent nouns) → is-a (kind-of, same sibling grain) → properties at most general class → facets from English numbers → defined classes for every “what counts as X?” question → disjoint sibling partitions → close `only` when the text says “may have only / cannot have other”.
