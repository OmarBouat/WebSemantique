# 🧳 Tourism Ontology Project

This project defines an OWL ontology for modeling a tourism ecosystem. It captures the relationships between **Tourists**, **Destinations**, **Activities**, **Transports**, **Accommodations**, **Events**, and **Guides** using OWL classes and both **object** and **data** properties.

---

## 📦 Ontology Structure

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
- `hasAccommodation` → `Tourist → Accommodation`
- `offersActivity` → `Tourist → Activity`
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

### 1. Retrieve all events with their names and dates
```sparql
SELECT ?event ?name ?date WHERE {
  ?event a ex:Event ;
         ex:EventName ?name ;
         ex:date ?date .
}
