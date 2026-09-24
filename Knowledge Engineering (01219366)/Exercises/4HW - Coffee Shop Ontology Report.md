# Exercise Report — Coffee Shop Ontology

**Format:** `References/4-Example of exercise report format.docx`  
**Topic:** Coffee shop

> [!warning] Rules from the format
> - Class hierarchy lists **classes only** — **no instances**.
> - Property table columns are **Property · Domain · Range**.
> - The format covers Steps **1, 3, 4, 5** only (Step 2 reuse and Steps 6–7 are not reported).

## Group Member

1. 
2. 
3. 
4. 

---

## Step 1: Define domain and scope

**Domain:** Beverages sold in a coffee shop — the drinks, what they are made of, and how they are served.

**Scope:** Covers: ==drink kinds== (coffee, tea, chocolate, fruit), their ==ingredients== (bean, milk, sweetener, topping), ==serving size==, ==roast level==, ==price==, and ==serving options== (iced, decaf). Does **not** cover baristas, shifts, branches, suppliers, loyalty accounts, or bakery production.

**Competency questions:**

1. What kind of milk can a drink contain?
2. Which drinks are dairy-free?
3. Which drinks contain no caffeine?
4. Which drinks are made from espresso, and which are brewed?
5. What sizes is a drink offered in?
6. What is the price of a drink in a given size?
7. Which sweeteners and toppings can be added to a drink?
8. Which type of coffee bean and roast level is a coffee drink made from?
9. Which drinks can be served iced?

---

## Step 3: Enumerate important terms

drink, coffee drink, espresso, espresso drink, latte, cappuccino, americano, mocha, brewed coffee, drip coffee, pour-over coffee, cold brew, tea, green tea, black tea, herbal tea, chocolate drink, fruit drink, smoothie, juice, ingredient, coffee bean, arabica, robusta, roast level, light roast, medium roast, dark roast, milk, dairy milk, plant milk, oat milk, almond milk, soy milk, sweetener, sugar, syrup, vanilla syrup, caramel syrup, topping, whipped cream, powder topping, sauce topping, size, small, medium, large, price, name, shot count, iced, decaf, volume, caffeine

*(Step 3 is a raw dump — it is fine that some of these later become individuals rather than classes.)*

---

## Step 4: Class and class hierarchy

- Drink
    - CoffeeDrink
        - EspressoDrink
            - Espresso
            - Americano
            - Latte
            - Cappuccino
            - Mocha
        - BrewedCoffee
            - DripCoffee
            - PourOverCoffee
            - ColdBrewCoffee
    - TeaDrink
        - GreenTea
        - BlackTea
        - HerbalTea
    - ChocolateDrink
    - FruitDrink
        - Smoothie
        - Juice
- Ingredient
    - CoffeeBean
        - ArabicaBean
        - RobustaBean
    - Milk
        - DairyMilk
        - PlantMilk
    - Sweetener
        - Sugar
        - Syrup
    - Topping
        - WhippedCreamTopping
        - PowderTopping
        - SauceTopping
- Size
- RoastLevel

**Why the cuts are consistent**

| Sibling row | Single criterion used |
| --- | --- |
| CoffeeDrink / TeaDrink / ChocolateDrink / FruitDrink | base ingredient of the drink |
| EspressoDrink / BrewedCoffee | how the coffee is extracted (pressure vs drip/steep) |
| DairyMilk / PlantMilk | origin of the milk |

`Iced` and `Decaf` are **not** classes — they are extrinsic serving options, so they are data properties (a hot latte should not change class when ordered iced).

### Which things are instances (NOT submitted — format says classes only)

Rule: it is an **instance** when it is the most specific answer you need and you attach **no restrictions** to it. Same pattern as the pizza ontology's `hasSpiciness value Hot` and `hasCountryOfOrigin value Italy`.

| Instance | Instance of | Why |
| --- | --- | --- |
| `[Small]` `[Medium]` `[Large]` | Size | closed list of named values |
| `[LightRoast]` `[MediumRoast]` `[DarkRoast]` | RoastLevel | closed list of named values |
| `[WholeMilk]` `[SkimMilk]` | DairyMilk | actual products; nothing subdivides them |
| `[OatMilk]` `[AlmondMilk]` `[SoyMilk]` | PlantMilk | actual products |
| `[VanillaSyrup]` `[CaramelSyrup]` | Syrup | actual products |
| `[Arabica]` `[Robusta]` | CoffeeBean | named varieties, no restrictions on them |

`Latte` / `Cappuccino` / `Americano` stay **classes** even though they look like menu answers: they carry restrictions (`hasMilk some Milk`) and must be classifiable under `DairyFreeDrink`. Restrictions and reasoner classification only work on classes.

That is why `Size` and `RoastLevel` appear above with **no children** — their members are individuals, so the format excludes them from the hierarchy.

---

## Step 5: Property

**Object property**

| Property | Domain | Range |
| --- | --- | --- |
| hasIngredient | Drink | Ingredient |
| hasMilk | Drink | Milk |
| hasSweetener | Drink | Sweetener |
| hasTopping | Drink | Topping |
| madeFromBean | CoffeeDrink | CoffeeBean |
| hasRoastLevel | CoffeeBean | RoastLevel |
| hasSize | Drink | Size |

`hasMilk`, `hasSweetener`, and `hasTopping` are **sub-properties** of `hasIngredient`.  
Ranges name the **parent** class (`Milk`, not `DairyMilk, PlantMilk`) — children are already covered by is-a.

**Data property**

| Property | Domain | Range |
| --- | --- | --- |
| hasName | Drink | string |
| hasPrice | Drink | decimal |
| hasShotCount | EspressoDrink | integer |
| isIced | Drink | boolean |
| isDecaf | Drink | boolean |
| hasVolume | Size | integer |

---

## Appendix (not part of the report format)

Kept for midterm practice — Step 6 facets / class descriptions:

```
Drink  SubclassOf
    hasName exactly 1 string
    hasPrice exactly 1 decimal
    hasPrice some decimal [>=0]
    hasSize exactly 1 Size

EspressoDrink  SubclassOf
    CoffeeDrink
    hasShotCount some integer [>=1]

Latte  SubclassOf
    EspressoDrink
    hasMilk some Milk

Americano  SubclassOf
    EspressoDrink
    not (hasMilk some Milk)

DairyFreeDrink  EquivalentTo
    Drink
    and (not (hasMilk some DairyMilk))

CaffeineFreeDrink  EquivalentTo
    Drink
    and (isDecaf value true)
```

Functional: `hasName`, `hasPrice`, `hasSize`, `hasShotCount`, `isIced`, `isDecaf`, `hasRoastLevel`.  
Disjoint: CoffeeDrink / TeaDrink / ChocolateDrink / FruitDrink · EspressoDrink / BrewedCoffee · DairyMilk / PlantMilk.
