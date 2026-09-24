 
**Instructor:** Hutchatai Chanlekha  
**Source:** `Knowledge Engineering (01219366)/References/5-Practical Guide for Building OWL Ontology_preclass.pdf`  
**Based on:** Matthew Horridge (2011), *A Practical Guide To Building OWL Ontologies Using Protégé 4 and CO-ODE Tools* (Ed. 1.3)

Workshop deck: pizza ontology in Protégé + the OWL meanings you must write on the exam. Check **Reasoner → Pellet** (install via File → Check for Plugins if missing).

## Outline

1. [[#1. OWL pieces: individuals, properties, classes]]
2. [[#2. Disjoint and enumerated classes]]
3. [[#3. Properties: types, hierarchy, inverse]]
4. [[#4. Property characteristics]]
5. [[#5. Domain and range are axioms]]
6. [[#6. Restrictions]]
7. [[#7. SubClass Of vs Equivalent To]]
8. [[#8. Exercises from the deck]]

---

## 1. OWL pieces: individuals, properties, classes

An OWL ontology is built from **individuals**, **properties**, and **classes**.

**Individuals** are objects in the domain (also called instances). OWL does **not** use the Unique Name Assumption: two names may denote the **same** individual until you say otherwise. Use `owl:sameAs` / `owl:differentFrom`.
![[Screenshot 2026-09-23 at 22.25.24.png|458]]
**Properties** (roles / relations) are **binary** links between individuals, e.g. Matthew `hasSibling` Gemma. They can carry extra **characteristics** (inverse, transitive, symmetric, …).
![[Screenshot 2026-09-23 at 22.25.12.png|462]]

**Classes** are **sets of individuals**, described by conditions for membership (“concept” ≈ class). Subclass = **necessary implication**: if `Cat` ⊑ `Animal`, every cat is an animal, without exception. OWL-DL reasoners can **compute** those subsumptions.

Pizza lab setup on the slides: open `pizza.owl`, change the IRI suffix to `/<id>-pizza-ontology`, save OWL/XML, add an ontology comment on the Active Ontology tab.

**Create Class Hierarchy** (Tools menu) can add a prefix/suffix to many names at once and, if checked, **make sibling classes disjoint**.

---

## 2. Disjoint and enumerated classes

OWL classes **overlap by default**. An individual **can** sit in two classes unless you disjoint them.

Disjoint ⇒ an individual asserted in one **cannot** be in the other. Pizza example: `DeepPanBase` disjoint `ThinAndCrispyBase` — a base cannot be both.

**Enumerated class:** membership is an exact list of individuals, e.g. `DaysOfTheWeek` ≡ `{Sunday, Monday, …, Saturday}`. Create the individuals first, then Equivalent To → Class Expression Editor with that set.

Slides also add `Pasta` ⊑ `Food` and disjoint it from the other food siblings as a Protégé drill.

---

## 3. Properties: types, hierarchy, inverse


![[Screenshot 2026-09-23 at 22.42.53.png|286]]![[Screenshot 2026-09-23 at 22.43.06.png|291]]

| Type           | Links                                        | Role                             |
| -------------- | -------------------------------------------- | -------------------------------- |
| **Object**     | individual → individual                      | the relations in the pizza model |
| **Datatype**   | individual → XML Schema / literal            | e.g. calorie integer             |
| **Annotation** | metadata on classes, properties, individuals | comments, labels                 |

==Properties may have **sub-properties**== (same idea as subclasses). In pizza: `hasTopping` and `hasBase` ⊏ `hasIngredient`.

**Inverse:** if `a hasTopping b` then `b isToppingOf a`. After you set inverse, Protégé **swaps domain/range** for the inverse automatically.

---

## 4. Property characteristics

| Characteristic | Meaning | Pizza / family example |
| --- | --- | --- |
| **Functional** | ≤ 1 object per subject | `hasBirthMother`; `hasBase`. Jean has Peggy and Margaret as mothers ⇒ Peggy **sameAs** Margaret |
| **Inverse functional** | ≤ 1 subject per object | inverse of a functional property is inverse-functional |
| **Transitive** | P(a,b) ∧ P(b,c) ⇒ P(a,c) | `hasAncestor` |
| **Symmetric** | P(a,b) ⇒ P(b,a) | `hasSibling` |
| **Asymmetric** | P(a,b) ⇒ not P(b,a) | `hasChild` |
| **Reflexive** | every individual is P-related to **itself** | `knows`; slides add `sameTaste` as reflexive |
| **Irreflexive** | nothing is P-related to **itself** | nobody is mother of oneself |

**Hard keys on the slides**

- If P is **transitive**, it **cannot** be **functional**.
- If P is transitive, its **inverse is also transitive**.
- Datatype properties: **Functional** is used (one calorie value). Inverse-functional / transitive / symmetric need two individuals — the slide asks you to notice they do **not** apply the same way to data properties.

Pizza probe: make `hasBase` functional, assert `orderpizza` hasBase `crispybase` **and** `crunchybase` → reasoner identifies the two bases.

---

## 5. Domain and range are axioms

`hasTopping`: domain `Pizza`, range `PizzaTopping`.

**Several classes listed as range in Protégé = intersection.** Range `Meat` and `Vegetable` means Meat ⊓ Vegetable. If those two are disjoint, the range is empty.

They are **not** database constraints. Assert `Icecream1 hasTopping almond`:

- Reasoner **infers** ==Icecream1 is a Pizza== (and almond is a topping).
- **Inconsistency only if** `IceCream` disjoint `Pizza`.

Same for untyped individuals: `Dish1 hasTopping Ingredient1` ⇒ Dish1 inferred as Pizza, Ingredient1 as PizzaTopping.

> Domain/range can cause surprising classifications in a large ontology. Some experts avoid them for that reason.

---

## 6. Restrictions

A restriction is an **anonymous class**: the set of individuals that participate in certain relationships. Three families on the slides.

### Quantifiers

| Manchester                          | Also called                  | Meaning                             |
| ----------------------------------- | ---------------------------- | ----------------------------------- |
| `hasTopping some MozzarellaTopping` | existential / someValuesFrom | **at least one** mozzarella topping |
| `hasTopping only VegetableTopping`  | universal / allValuesFrom    | **every** topping is vegetable      |

`some` example for pizza: `hasBase some PizzaBase`. CheesyPizza drill: `hasTopping some CheeseTopping`.

**`only` includes empty.** `hasTopping only MozzarellaTopping` also covers pizzas with **no toppings at all**. VegetarianPizza as “only cheese or vegetable” therefore also contains pizzas with zero toppings. If they **must** have toppings, add `some` as well.

If you want **must have and only** cheese + vegetable:

```
hasTopping some (CheeseTopping or VegetableTopping)
hasTopping only (CheeseTopping or VegetableTopping)
```

### Two `only`s = intersection of fillers

(a) `hasTopping only CheeseTopping` **and** `hasTopping only VegetableTopping`  
≡ `hasTopping only (CheeseTopping and VegetableTopping)`  
Fillers must be in the **overlap**.

(b) `hasTopping only (CheeseTopping or VegetableTopping)` — **union**; the intended vegetarian closure.

If Cheese and Vegetable are **disjoint**, (a) means the pizza **cannot have any topping**.

### Cardinality

For property P: **min** / **max** / **exactly** *n* relationships.

`InterestingPizza`: `hasTopping min 3 …`  
`FourCheesePizza` ⊑ `NamedPizza` and `hasTopping exactly 4 CheeseTopping`. That still allows **other** topping types (open world). To have **only** four cheeses: add `hasTopping only CheeseTopping`.

### Datatype restrictions

`hasCalorificContentValue some integer` — every pizza has at least one calorie value. Functional on a data property ⇒ at most one such value.

Facet: `HighCaloriePizza` with `hasCalorificContentValue some integer [>= 400]`.

### hasValue

`hasCountryOfOrigin value Italy` — filler is the **individual** Italy, not a class. Used for `MozzarellaTopping` “from Italy.”

---

## 7. SubClass Of vs Equivalent To

A restriction draws a cloud of individuals (anonymous class). Where you hang the named class relative to that cloud is the whole point.

### Necessary vs necessary and sufficient (slide summary)

**Necessary condition** (SubClass Of / primitive)

- If an individual is a member of class A, it **must** satisfy the conditions.
- We **cannot** say that any random individual that satisfies these conditions **must** be a member of class A.

**Necessary and sufficient condition** (Equivalent To / defined)

- If an individual is a member of class A, it **must** satisfy the conditions.
- **And** if any individual satisfies these conditions, then it **must** be a member of class A.

### VegetarianPizza — converting SubClass Of → Equivalent To

The slide’s move: same toppings restriction, different header. That is what turns a primitive class into a defined class.

![[Screenshot 2026-09-23 at 23.35.22.png|720]]

| | SubClass Of (left) | Equivalent To (right) |
| --- | --- | --- |
| Protégé | `hasTopping only (CheeseTopping or VegetableTopping)` **and** `Pizza` as two SubClass Of rows | `Pizza and (hasTopping only (CheeseTopping or VegetableTopping))` under **Equivalent To** |
| Read | If x is a VegetarianPizza, then it can have **only** cheese or vegetable (or both) as toppings. | **Same**, **and** if a pizza has only cheese/vegetable toppings, it **is** a VegetarianPizza. |
| Reasoner | Will **not** classify Margherita under VegetarianPizza from matching toppings alone. | **Will** classify any pizza that matches as VegetarianPizza. |

Left = necessary only. Right = necessary **and** sufficient. Convert in Protégé with “Convert to defined class” (or type the `and` expression under Equivalent To).



### SubClass Of = necessary = primitive

```
Pizza  SubClass Of
    hasBase exactly 1 PizzaBase
    hasTopping some PizzaTopping
```

`Pizza` is a **subset** of the anonymous class. Every pizza must obey φ. Something that obeys φ is **not** automatically a pizza.

Margherita on the slides (all under SubClass Of):

- `NamedPizza`
- `hasTopping some MozzarellaTopping`
- `hasTopping some TomatoTopping`
- `hasTopping only (MozzarellaTopping or TomatoTopping)`

Read: **if** it is Margherita, **then** those hold. That is necessary only.

CheesyPizza with only `Pizza` + `hasTopping some CheeseTopping` under SubClass Of: a random pizza with cheese is **not** therefore a CheesyPizza.

### Equivalent To = necessary and sufficient = defined

![[Screenshot 2026-09-23 at 23.28.10.png|589]]

Same φ under Equivalent To: the named class **is** that anonymous class.

- If x is Pizza → x satisfies φ  
- If x satisfies φ → x **is** Pizza  

A class with at least one N+S set is a **defined class**. Primitive = necessary only.

Move conditions with Protégé “Convert to defined class.”

**Why the reasoner cares:** if every member of B satisfies the definition of defined class A, then B ⊑ A. Checking subsumption is a main DL-reasoner job. VegetarianPizza must be **defined** (and usually closed with `only`) before named veg pizzas classify under it.

IceCream drill on the slides: adding `hasTopping some FruitTopping` plus disjointness with Pizza can make IceCream **unsatisfiable** once domain axioms fire.

---

## 8. Exercises from the deck

All four Pizza classes are **Equivalent To** (defined). Assume Meat, Vegetable, Cheese are pairwise **disjoint**. Property = `hasTopping`.

**Pizza1**
```
Pizza1  EquivalentTo
    hasTopping only (VegetableTopping or CheeseTopping)
    and (hasTopping some VegetableTopping)
    and (hasTopping some CheeseTopping)
```

Answer: Pizza1 ⊑ **Pizza2** (has cheese) and Pizza1 ⊑ **Pizza4** (has vegetable). Not ⊑ Pizza3 (the `only` forbids meat). None of 2, 3, 4 is a subclass of Pizza1 (they are weaker).

**Pizza2**
```
Pizza2  EquivalentTo
    hasTopping some CheeseTopping
```

Answer: inferred child **Pizza1**. Pizza2 is **not** under Pizza1 / Pizza3 / Pizza4 (`some Cheese` does not force veg, `only`, or meat).

**Pizza3**
```
Pizza3  EquivalentTo
    hasTopping some VegetableTopping
    and (hasTopping some MeatTopping)
```

Answer: Pizza3 ⊑ **Pizza4** (has vegetable). Not ⊑ Pizza1 (`some Meat` vs `only` veg/cheese). Not ⊑ Pizza2 (no cheese required). No inferred children among the four.

**Pizza4**
```
Pizza4  EquivalentTo
    hasTopping some VegetableTopping
```

Answer: inferred children **Pizza1** and **Pizza3**. Pizza4 is not under Pizza1 / Pizza2 / Pizza3.

---

### 8.2 MyPizza menus

Create under `MyPizza` ⊑ `Pizza`. Add topping classes if missing (`TomatoSauceTopping` ⊑ `SauceTopping`, `Pineapple` ⊑ `FruitTopping`, `Basil` ⊑ `HerbSpiceTopping`).

**Hawaiian** — at least pineapple, ham, tomato sauce, and cheese

Answer:
```
HawaiianPizza  SubclassOf
    MyPizza
    hasTopping some Pineapple
    hasTopping some HamTopping
    hasTopping some TomatoSauceTopping
    hasTopping some CheeseTopping
```

**Spinach Deluxe** — must have **only** spinach and cheese

Answer:
```
SpinachDeluxePizza  SubclassOf
    MyPizza
    hasTopping some SpinachTopping
    hasTopping some CheeseTopping
    hasTopping only (SpinachTopping or CheeseTopping)
```

**Spicy Meat Lover** — at least 4 meats, one is ham; spiciness Hot (`Hot` is an **individual** → `value`)

Answer:
```
SpicyMeatLoverPizza  SubclassOf
    MyPizza
    hasTopping min 4 MeatTopping
    hasTopping some HamTopping
    hasSpiciness value Hot
```

**Pesto Chicken** — must contain chicken and basil; spiciness Medium

Answer:
```
PestoChickenPizza  SubclassOf
    MyPizza
    hasTopping some ChickenTopping
    hasTopping some Basil
    hasSpiciness value Medium
```

**Super Cheese** — **only 5** kinds of cheese

Answer:
```
SuperCheesePizza  SubclassOf
    MyPizza
    hasTopping exactly 5 CheeseTopping
    hasTopping only CheeseTopping
```

---

## Takeaways

- No UNA; disjointness is **opt-in**; enumerated class = explicit individual list.
- Functional: ≤1 filler. Transitive ⇏ functional; inverse of transitive is transitive.
- Domain/range **infer types**; clash only with disjoint.
- `some` = at least one; `only` = all fillers **or none**; two `only`s = intersection of fillers.
- SubClass Of = necessary (primitive, no inferred children). Equivalent To = N+S (defined, reasoner classifies into it).

**Collaboration tools named at the end:** Collaborative Protégé; [WebProtégé](https://webprotege.stanford.edu).
