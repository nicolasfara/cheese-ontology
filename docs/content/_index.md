+++
title = "🧀 Cheese Ontology"
description = "A rich ontology for cheese and all things related"
outputs = ["Reveal"]

[reveal_hugo]
transition = "slide"
transition_speed = "fast"
custom_theme = "custom-theme.scss"
custom_theme_compile = true

[reveal_hugo.custom_theme_options]
targetPath = "css/custom-theme.css"
enableSourceMap = true
+++

# 🧀

# Cheese Ontology

A rich ontology for cheese and all things related

~ __Castellucci__ | __Farabegoli__ | __Vitali__ ~

---

# Motivations

<ul>
{{% fragment %}}<li>We have <b>domain experts</b> to ask</li>{{% /fragment %}}
{{% fragment %}}<li>There is <b>no available ontology</b> on cheese</li>{{% /fragment %}}
{{% fragment %}}
<li>
An ontology classifying cheeses may be <b>useful</b> for:
<ul>
<li>Cheese factories aiming to standardize data management</li>
<li><em>MIPAAF</em> aiming for improving traceability of cheese's raw material</li>
<li>Local organizations aiming to promote local products</li>
</ul>
</li>
{{% /fragment %}}
{{% fragment %}}<li>We <b>❤</b> Cheese</li>{{% /fragment %}}
</ul>

---

# Alignments

- `EnvO` -- An ontology for the description of environments
- `DBPedia` -- Global and unified access to knowledge graphs
- `FoodOn` -- A farm to fork ontology
- `AGROVOC` -- Covers subject like agriculture, food, etc.
- `FOod in Open Data` -- Italian's food certifications ontology

---

# Statistics

| Metrics | Count |
|---------|-------|
| _Axioms_  | __1099__   |
| _Logical axioms_ | __399__ |
| _Declaration axioms_ | __181__ |
| _Class_ | __70__ |
| _Object property_ | __24__ |
| _Data property_ | __2__ |
| _Individuals_ | __79__ |
| _Annotation Property_ | __10__ |

---

# Main Concepts

The ontology captures the following concepts in the __cheese__ domain

- _Cheese_ types
- _Raw Materials_ and _Milk_ used to make a cheese
- _Locations_ where the cheesemaking has taken place
- _Aging_ or _Ripening_ of a cheese
- _Environment_ where a cheese is matured
- _Certifications_ of a cheese

---

# Overview

<img src="generic-overview.svg" width="85%">

---

# Cheese Classes

![Cheese](cheese.svg)

---

# Cheese Properties

<br>

| Object Property | Domain | Range | Inverse Of|
|-----------------|--------|-------|-----------|
| _hasTexture_      | `Cheese` | `CheeseTexture` | _isTextureOf_ |

---

# Raw Material Class

![Raw material](rawMaterial.svg)

---

# Raw Material Properties

| Object Property | Domain | Range | Inverse Of|
|-----------------|--------|-------|-----------|
| <em>isMadeWith-<br>RawMaterial</em> | `Cheese` | `RawMaterial` | <em>isRawMaterial-<br>UsedIn</em> |
| _isMadeWithMold_        | <code>MoldRipened-<br>Cheese</code> | `Mold` | _isMoldUsedIn_ |
| _isMadeWithMilk_        | `Cheese` | <code>Milk-<br>RawMaterial</code> | _isMilkUsedIn_ |

---

# Milk Classes

<img src="milk.svg" width="80%" alt="milk">

---

# Milk Properties

| Object Property | Domain | Range | Inverse Of|
|-----------------|--------|-------|-----------|
| <em>isMadeWith-<br>RawMaterial</em> | `Cheese` | `RawMaterial` | <em>isRawMaterial-<br>UsedIn</em> |
| _isMadeWithMilk_        | `Cheese` | <code>Milk-<br>RawMaterial</code> | _isMilkUsedIn_ |
| _hasSkimming_ | `Milk` | `MilkSkimming` | _isSkimmingOf_ |

---

# Environment and Event Classes

![Environment](EventEnvironment.svg)

---

# Environment and Event Object Properties

| Object Property | Domain | Range | Inverse Of|
|-----------------|--------|-------|-----------|
| <em>isLocatedIn-<br>Environment</em> | `Event` | `Environment` |<em>isEnvironment-<br>LocationOf</em> |
| _isLocatedIn_        | | <code>Populated-<br>Place</code> | _isLocationOf_ |
| _hasTakenPlaceIn_ | `Event` |  | _isPlacedWhere_ |
| _isProducedIn_ | `Food` | | _isProductionPlaceOf_ |

---

# Environment and Event Data Properties

| Object Property | Domain | Range |
|-----------------|--------|-------|
| _hasAgingDuration_ | `Aging` | `xsd:positiveInteger` |
| _hasRipeningDuration_ | `Ripening` | `xsd:positiveInteger` |

---

# Certifications

![Certifications](certification.svg)

---

# Certifications Properties

| Object Property | Domain | Range | Inverse Of|
|-----------------|--------|-------|-----------|
| _hasProtectedName_ | <code>Protected-<br>Food</code> | <code>Protected-<br>Name</code> | _isProtectedNameOf_ |

---

# SWRL Rules

---

## Rule 1

This rule defines that the __aging__ of a cheese should be between 1 and 30 days

<img src="swrl1.svg" alt="SWRL1" width="60%">

```prolog
hasRipeningDuration(?r, ?d) ^ Ripening(?r) ->
swrlb:greaterThanOrEqual(?d, 1) ^ swrlb:lessThanOrEqual(?d, 30)
```

<small>
⚠️ In the ontology when referring to the ripening period we refer to a period expressed in days
</small>

---

## Rule #2

This rule defines that the __aging__ of a cheese should be greater than one month

<img src="swrl2.svg" alt="SWRL2" width="55%">

```prolog
Aging(?a) ^ hasAgingDuration(?a, ?d) -> swrlb:greaterThanOrEqual(?d, 1)
```

<small>
⚠️ In the ontology when referring to the aging period we refer to a period expressed in months
</small>

---

## Rule #3

This rule defines that a `Cheese` with a cheese certification is a `ProtectedCheese`:

<img src="swrl3.svg" alt="SWRL3" width="70%">

```prolog
obo:FOODON_00001013(?c) ^
food-upper:haDenominazione(?c, ?n) ^
food-cheese:DenominazioneFormaggio(?n) -> food-cheese:Formaggio(?c)
```

<!--small>
⚠️ In the ontology when referring to the aging period we refer to a period expressed in month
</small-->

---

## Rule #4

This rule defines that a Ricotta `Cheese` with a ricotta certification is a `ProtectedRicotta`:

<img src="swrl4.svg" alt="SWRL4" width="80%">

```prolog
obo:FOODON_00001013(?c) ^ 
food-upper:haDenominazione(?c, ?n) ^
food-ricotta:DenominazioneRicotta(?n) -> food-ricotta:Ricotta(?c)
```

<small>
⚠️ Since <b>Ricotta</b> is not a cheese, it is not quite correct to assign it the <code>Cheese</code> class.<br>
Nevertheless, we accept this simplification in our domain.
</small>

---

# SPARQL queries

---

## Query #1

_"Show the number of cheeses produced in each city along with their region"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT DISTINCT ?city ?region (COUNT(?city) AS ?count)
WHERE {
    [] :isProducedIn ?city
  
    SERVICE <https://dbpedia.org/sparql> {
        ?region dbo:type dbr:Regions_of_Italy;
                ^dbo:region ?city.
    }
}
GROUP BY ?city ?region
ORDER BY DESC(COUNT(?city))
```

---

## Query #2

_"Show every cheese which is either made with a vegetal rennet or with skimmed milk"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT DISTINCT ?cheese ?label
WHERE {
    { ?cheese :isMadeWithRawMaterial/a :VegetalRennet }
    UNION
    { ?cheese :isMadeWithMilk/a/rdfs:label "SkimmedMilk"@en }
    
    OPTIONAL { ?cheese rdfs:label ?label }
}
ORDER BY ?label
```

---

## Query #3

_"Show each certified cheese along with their region of production and their protected name"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX food-upper: <http://w3id.org/food/ontology/disciplinare-upper/>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?region ?cheese ?cheeselabel ?name ?namelabel WHERE {
    ?cheese :isProducedIn ?city;
            a/rdfs:label "ProtectedCheese"@en;
            food-upper:haDenominazione ?name.

    SERVICE <https://dbpedia.org/sparql> {
        ?region dbo:type dbr:Regions_of_Italy;
                ^dbo:region ?city.
    }
    OPTIONAL { ?cheese rdfs:label ?cheeselabel }
    OPTIONAL { ?name rdfs:label ?namelabel }
} ORDER BY ?cheeselabel
```

---

## Query #4

_"Show every cheese that has been produced using another cheese as its raw material"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT DISTINCT ?cheese ?label
WHERE {
    ?cheese :isMadeWithRawMaterial+/a/rdfs:label "Cheese"@en
    
    OPTIONAL { ?cheese rdfs:label ?label }
}
ORDER BY ?label
```

---

## Query #5

_"Get all cheese aged in a pit in the province of Forlì-Cesena"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?cheese ?cheeselabel ?city ?citylabel ?pit ?pitlabel WHERE {
    [] :isAgingOf ?cheese;
       :hasTakenPlaceIn ?city;
       :isLocatedInEnvironment ?pit.
    ?pit a/rdfs:label "Pit"@en.
    SERVICE <https://dbpedia.org/sparql> {
        ?city dbo:province dbr:Province_of_Forlì-Cesena
        OPTIONAL {
            ?city rdfs:label ?citylabel
            FILTER(LANG(?citylabel) = "it")
        }
    }
    OPTIONAL { ?cheese rdfs:label ?cheeselabel }
    OPTIONAL { ?pit rdfs:label ?pitlabel }
} ORDER BY ?citylabel
```

---

## Query #6

_"Check whether or not it exist at least a cheese made without an animal rennet"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

ASK
WHERE {
    ?cheese a/rdfs:label "Cheese"@en

    FILTER NOT EXISTS { ?cheese :isMadeWithRawMaterial/a :AnimalRennet }
}
```

---

# Thanks for your attention
