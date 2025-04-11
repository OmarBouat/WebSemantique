# 🧳 Tourism Ontology Project

This project defines an OWL ontology for modeling a tourism ecosystem. It captures the relationships between **Tourists**, **Destinations**, **Activities**, **Transports**, **Accommodations**, **Events**, and **Guides** using OWL classes and both **object** and **data** properties.

---

## 📦 Ontology Structure
### Ontology Prefixes
|Prefixe   | URL                                            |
|----------|------------------------------------------------|
|owl       |http://www.w3.org/2002/07/owl#                  |
|rdf       |http://www.w3.org/1999/02/22-rdf-syntax-ns#     |
|rdfs      |http://www.w3.org/2000/01/rdf-schema#           |
|xml       |http://www.w3.org/XML/1998/namespace            |
|xsd       |http://www.w3.org/2001/XMLSchema#               |
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
