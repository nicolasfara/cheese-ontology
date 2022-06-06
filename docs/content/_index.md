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

---

# Raw Material

---

# Milk

---

# Environment

---

# Event

---

# Certifications

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

# SPARQL queries

Some example query

---

# Query #1
_"Find all cheeses with ripeness between 5 and 20 days"_

```sql
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdf: <http://wwv.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://wuwv.w3.org/2000/01/rdf-schema#>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?cheese (STR(?lab) AS ?label) ?duration
WHERE {
    ?cheese :hasRipeness ?ripeness.
    ?ripeness :hasRipenessDuration ?duration.

    OPTIONAL {?cheese rdfs:label ?lab}
    FILTER (?duration > "5"^^xsd:integer && ?duration < "20"^^xsd:integer)
}
ORDER BY ?label
```

---

# Query #2
_"Find all cheeses with a genereic certification"_

```sql
PREFIX owl: <http://www.w3.Org/2002/07/owl#>
PREFIX rdf: <http://www.w3.0rg/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.Org/2000/01/rdf-schema#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX protected: <http://w3id.org/food/ontology/disciplinare-formaggio/>
PREFIX : <https://github.com/nicolasfara/cheese-ontology/>

SELECT ?x (STR(?lab) AS ?label) WHERE {
    ?x rdf:type protected:Formaggio.
    OPTIONAL {?x rdfs:label ?lab}
}
ORDER BY ?label
```