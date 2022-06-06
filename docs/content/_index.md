+++
title = "🧀 Cheese Ontology"
description = "A Hugo theme for creating Reveal.js presentations"
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

A detailed cheese ontology

~ __Castellucci__ | __Farabegoli__ | __Vitali__ ~

---

# Motivations

- We have _domain experts_ to ask
- There is no available ontology on **cheese**
- An ontology classifying cheeses may be useful
- We **❤** Cheese

---

# Alignments

  - `OBO` -- TODO
  - `GeoNames` -- Used to get places
  - `FOODON` -- Used to take all food classes
  - `Agrovoc` -- TODO
  - `disciplinare` -- Used to modelling certifications

---

# Overview

![Overview diagram](generic-overview.svg)

---

# Cheese

![Cheese](cheese.svg)

---

# Raw Material

![Raw material](rawMaterial.svg)

---

# Milk

<img src="milk.svg" width="80%" alt="milk">

---

# Environment

![Environment](environment.svg)

---

# Event

![Event](event.svg)

---

# Certifications

![Certifications](certification.svg)

---

# Statistics

| Metrics | Count |
|---------|-------|
| _Axioms_  | __452__   |
| _Logical axioms_ | __214__ |
| _Declaration axioms_ | __149__ |
| _Class_ | __74__ |
| _Object property_ | __23__ |
| _Data property_ | __2__ |
| _Individuals_ | __49__ |
| _Annotation Property_ | __4__ |


---

# SWRL Rules

---

## Rule #1

With this rule we __prevent__ a certified cheese from being made from uncertified milk:

```prolog
obo:FOODON_00001013(?cheese) ^
isMadeWithMilk(?cheese, ?milk) ^
ProtectedMilkRawMaterial(?milk) -> food-cheese:Formaggio(?cheese)
```
<small>
⚠️ This rule implies that if a cheese is made from certified milk, then the cheese itself is also certified
</small>

Due to the _open world assumption_, we are unable to tight this constraint even with __SWLR__.

---

## Rule #2

This rule defines that the __ripening__ of a cheese should be between 1 and 30 days.


```prolog
hasRipeningDuration(?r, ?d) ^ Ripening(?r) ->
swrlb:greaterThanOrEqual(?d, 1) ^
swrlb:lessThanOrEqual(?d, 30)
```
<small>
In the ontology when referring to the ripening period we refer to a period expressed in days
</small>

---

## Rule #3

This rule defines that the __aging__ period should be at least one month:

```prolog
Aging(?a) ^ hasAgingDuration(?a, ?d) -> swrlb:greaterThanOrEqual(?d, 1)
```
<small>
In the ontology when referring to the aging period we refer to a period expressed in month
</small>

---

# SPARQL queries

---

## Query #1

_"Find all cheeses with ripeness between 5 and 20 days"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?cheese ?label ?duration
WHERE {
    ?cheese :hasRipening/:hasRipeningDuration ?duration.

    OPTIONAL { ?cheese rdfs:label ?label }
    FILTER (?duration > 5 && ?duration < 20)
}
ORDER BY ?label
```

---

## Query #2

_"Find all cheeses with a certification"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?cheese ?label ?protectedname
WHERE {
    ?cheese a/rdfs:label "ProtectedCheese"@en.
	
    OPTIONAL { ?cheese :hasProtectedName ?protectedname }
    OPTIONAL { ?cheese rdfs:label ?label }
}
ORDER BY ?label
```

---

## Query #3

_"Get all cheese made from animal milk"_

```sql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?cheese ?cheeselabel ?milk ?milklabel ?animal WHERE {
    ?cheese :isMadeWithMilk ?milk.
    { 	?milk a/rdfs:label "CowMilk"@en.
    	VALUES ?animal { "Formaggio di mucca" }
  	} UNION { 
    	?milk a/rdfs:label "SheepMilk"@en.
    	VALUES ?animal { "Formaggio di pecora" }
  	} UNION { 
    	?milk a/rdfs:label "GoatMilk"@en.
    	VALUES ?animal { "Formaggio di capra" }
  	} UNION { 
    	?milk a/rdfs:label "BuffaloMilk"@en.
    	VALUES ?animal { "Formaggio di bufala" }
  	}

    OPTIONAL { ?milk rdfs:label ?milklabel }
    OPTIONAL { ?cheese rdfs:label ?cheeselabel }
} ORDER BY ?cheeselabel
```
---