# 🧳 Tourism Ontology Project

This project defines an OWL ontology for modeling a tourism ecosystem. It captures the relationships between **Tourists**, **Destinations**, **Activities**, **Transports**, **Accommodations**, **Events**, and **Guides** using OWL classes and both **object** and **data** properties.

---

## 📦 Ontology Structure
### Ontology Prefixes

|Prefixe   | URL                                            |
|----------|------------------------------------------------|
|`owl`     |http://www.w3.org/2002/07/owl#                  |
|`rdf`     |http://www.w3.org/1999/02/22-rdf-syntax-ns#     |
|`rdfs`    |http://www.w3.org/2000/01/rdf-schema#           |
|`xml`     |http://www.w3.org/XML/1998/namespace            |
|`xsd`     |http://www.w3.org/2001/XMLSchema#               |
---
### 🧍 Classes
- **Person**
  - `Tourist`
  - `Guide`
- **Place**
  - `Destination`
    - `City`
    - `Monument`
    - `Park`
- **Activity**
  - `CulturalActivity`
  - `NatureActivity`
  - `SportActivity`
- **Accommodation**
  - `Hotel`
  - `Hostel`
  - `Camping`
- **Transport**
  - `Bus`
  - `Train`
  - `Plane`
- **Event**

---

### 🔗 Object Properties
- `visits` → `Tourist → Destination`
- `usesTransport` → `Tourist → Transport`
- `hasAccommodation` → `Destination → Accommodation`
- `offersActivity` → `Destination → Activity`
- `attendsEvent` → `Tourist → Event`
- `guides` → `Guide → Tourist`

---

### 📐 Data Properties

| Property         | Domain             | Range        | Description                     |
|------------------|--------------------|--------------|---------------------------------|
| `EventName`      | `Event`            | `xsd:string` | Name of the event               |
| `activityName`   | `Activity`         | `xsd:string` | Name of the activity            |
| `date`           | `Event`            | `xsd:date`   | Date of the event               |
| `location`       | `Place`            | `xsd:string` | Geographical location           |
| `age`            | `Tourist`          | `xsd:integer`| Age of the tourist              |
| `foaf:name`      | `Tourist`          | `xsd:string` | Name of the toursit             |
| `pricePerNight`  | `Accommodation`    | `xsd:decimal`| Cost per night for accommodation|

---

## 🧪 SPARQL Query Examples

### 1. Tourists and the destinations they visit
```sparql
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX ex: <http://www.example.org/tourism#>

SELECT ?tourist ?destination WHERE {
  ?tourist a ex:Tourist ;
           ex:visits ?destination .
}
```
### 2. Tourists and their transport usage
```sparql
PREFIX ex: <http://www.example.org/tourism#>

SELECT ?tourist ?transport
WHERE {
  ?tourist a ex:Tourist .
  ?tourist ex:usesTransport ?transport .
}
```
### 3. Destinations and their accommodations
```sparql
PREFIX ex: <http://www.example.org/tourism#>

SELECT ?destination ?accommodation
WHERE {
  ?destination a ex:Destination .
  ?destination ex:hasAccommodation ?accommodation .
}
```
### 4. Destinations and their activities
```sparql
PREFIX ex: <http://www.example.org/tourism#>

SELECT ?destination ?activity
WHERE {
  ?destination a ex:Destination .
  ?destination ex:offersActivity ?activity .
}
```

## 🔧 Contraintes OWL Principales

Cette section décrit les principales contraintes définies dans l'ontologie OWL du projet.

---

### 🎯 Types de Contraintes

#### 📌 Cardinalité

- **Destination** doit avoir **exactement une** `localisation`  
  *(owl:qualifiedCardinality = 1)*

- **Tourist** doit avoir **exactement un** `âge`  
  *(owl:qualifiedCardinality = 1)*

#### 🧭 Hiérarchie

- **Destination** est une **sous-classe** de `Place`  
  → Elle doit proposer **au moins une** `Activity`  
  *(owl:someValuesFrom Activity)*

- **Tourist** est une **sous-classe** de `Person`  
  → Elle doit être **liée obligatoirement** à des `Activity` ou `Event`

#### 🧾 Typage de Données

- `EventName` et `activityName` sont typés comme des **chaînes de caractères**  
  *(xsd:string)*

- La `date` d’un événement est au format **xsd:date**

#### 🚫 Exclusivité

- Les classes **Tourist** et **Destination** sont **disjointes**  
  → Un même individu **ne peut pas** appartenir aux deux classes  
  *(owl:disjointWith)*

#### 🧩 Sous-Classes Spécialisées

- **Accommodation** comprend des **sous-classes exclusives** :  
  `Hotel`, `Hostel`, `Camping`

- **Transport** est divisé en :  
  `Bus`, `Train`, `Plane`


## 📚 Règles SWRL Détaillées

### 1. Règle de Participation Automatique aux Activités
**Logique Métier**  
Si un touriste visite une destination offrant une activité, il y participe automatiquement.

```swrl
Tourist(?t) ∧ visits(?t, ?d) ∧ Destination(?d) ∧ offersActivity(?d, ?a) → attends(?t, ?a)
```

### 2. Règle de Lien Culture-Monument
**Logique Métier**  
Toute activité culturelle implique la visite d'un monument.

```swrl
CulturalActivity(?a) ∧ attends(?t, ?a) → visits(?t, ?m) ∧ Monument(?m)
```
### 3. Règle de Détection d'Hébergement Haut de Gamme
**Logique Métier**  
Un hébergement coûteux (>100€/nuit) est classifié comme Hôtel.

```swrl
Accommodation(?a) ∧ pricePerNight(?a, ?p) ∧ swrlb:greaterThan(?p, 100) → Hotel(?a)
```
### 4. Règle d'Identification d'Événement National
**Logique Métier**  
Un événement contenant "Nationale" dans son nom est une activité culturelle.

```swrl
Event(?e) ∧ EventName(?e, ?name) ∧ swrlb:contains(?name, "Nationale") → CulturalActivity(?e)
```
