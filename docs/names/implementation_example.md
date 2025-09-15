# ROR `names` to Schema.org Implementation Example

## Overview

Practical implementation examples showing how to map ROR organizational names to Schema.org properties using the content-primary approach.

**Mapping Approach**: Content-primary with type-based property mapping
**Target Institution**: ETH Zurich (multilingual European institution)
**Focus**: Real JSON-LD output examples with step-by-step implementation

---

## Source ROR Record

Real ROR data structure for ETH Zurich:

```json
{
  "id": "https://ror.org/05a28rw58",
  "names": [
    {
      "value": "ETH Zurich",
      "types": ["ror_display"],
      "lang": "en"
    },
    {
      "value": "Swiss Federal Institute of Technology Zurich",
      "types": ["label"],
      "lang": "en"
    },
    {
      "value": "Eidgenössische Technische Hochschule Zürich",
      "types": ["label"],
      "lang": "de"
    },
    {
      "value": "École Polytechnique Fédérale de Zurich",
      "types": ["label"],
      "lang": "fr"
    },
    {
      "value": "ETH",
      "types": ["acronym"],
      "lang": "en"
    },
    {
      "value": "ETHZ",
      "types": ["alias"],
      "lang": "en"
    }
  ]
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Simple English-focused mapping:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "legalName": "Swiss Federal Institute of Technology Zurich",
  "alternateName": ["ETH", "ETHZ"]
}
```

### Enhanced Implementation

With multilingual support preserved:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "legalName": "Swiss Federal Institute of Technology Zurich",
  "alternateName": ["ETH", "ETHZ"],
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "multilingual-label",
      "value": {
        "@type": "StructuredValue",
        "name": "Eidgenössische Technische Hochschule Zürich",
        "inLanguage": "de"
      }
    },
    {
      "@type": "PropertyValue",
      "name": "multilingual-label",
      "value": {
        "@type": "StructuredValue",
        "name": "École Polytechnique Fédérale de Zurich",
        "inLanguage": "fr"
      }
    }
  ]
}
```

---

## Step-by-Step Implementation

### Step 1: Extract Primary Name

```javascript
// Input: ROR names array
const rorNames = [
  { "value": "ETH Zurich", "types": ["ror_display"], "lang": "en" },
  { "value": "Swiss Federal Institute...", "types": ["label"], "lang": "en" }
  // ... more names
];

// Extract primary name (ror_display value)
const primaryName = rorNames.find(n => n.types.includes("ror_display"))?.value;
// Result: "ETH Zurich"
```

### Step 2: Extract Legal Name

```javascript
// Find label type in English
const legalName = rorNames
  .filter(n => n.types.includes("label") && n.lang === "en")
  .map(n => n.value)[0];
// Result: "Swiss Federal Institute of Technology Zurich"
```

### Step 3: Extract Alternate Names

```javascript
// Find acronyms and aliases
const alternateNames = rorNames
  .filter(n => n.types.includes("acronym") || n.types.includes("alias"))
  .map(n => n.value);
// Result: ["ETH", "ETHZ"]
```

### Step 4: Handle Multilingual Names

```javascript
// Find non-English labels for additionalProperty
const multilingualLabels = rorNames
  .filter(n => n.types.includes("label") && n.lang !== "en")
  .map(n => ({
    "@type": "PropertyValue",
    "name": "multilingual-label",
    "value": {
      "@type": "StructuredValue",
      "name": n.value,
      "inLanguage": n.lang
    }
  }));
```

---

## Real-World Examples

### Example 1: Simple Organization

**ROR Input:**
```json
{
  "names": [
    {
      "value": "University of California",
      "types": ["ror_display"],
      "lang": "en"
    },
    {
      "value": "UC",
      "types": ["acronym"],
      "lang": "en"
    }
  ]
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "University of California",
  "alternateName": ["UC"]
}
```

### Example 2: Complex Multilingual Organization

**ROR Input:**
```json
{
  "names": [
    {
      "value": "CERN",
      "types": ["ror_display"],
      "lang": "en"
    },
    {
      "value": "European Organization for Nuclear Research",
      "types": ["label"],
      "lang": "en"
    },
    {
      "value": "Organisation européenne pour la recherche nucléaire",
      "types": ["label"],
      "lang": "fr"
    }
  ]
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "ResearchOrganization",
  "name": "CERN",
  "legalName": "European Organization for Nuclear Research",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "multilingual-label",
    "value": {
      "@type": "StructuredValue",
      "name": "Organisation européenne pour la recherche nucléaire",
      "inLanguage": "fr"
    }
  }
}
```

---

## Implementation Code Examples

### Python Implementation

```python
def map_ror_names_to_schema(ror_names):
    schema_org = {}

    # Primary name (ror_display or first value)
    display_name = next((n for n in ror_names if "ror_display" in n["types"]), None)
    if display_name:
        schema_org["name"] = display_name["value"]
    else:
        schema_org["name"] = ror_names[0]["value"]

    # Legal name (English label)
    legal_names = [n for n in ror_names if "label" in n["types"] and n.get("lang") == "en"]
    if legal_names:
        schema_org["legalName"] = legal_names[0]["value"]

    # Alternate names (acronyms and aliases)
    alternate_names = [n["value"] for n in ror_names
                      if any(t in n["types"] for t in ["acronym", "alias"])]
    if alternate_names:
        schema_org["alternateName"] = alternate_names

    return schema_org
```

### JavaScript Implementation

```javascript
function mapRorNamesToSchema(rorNames) {
  const schemaOrg = {};

  // Primary name
  const displayName = rorNames.find(n => n.types.includes("ror_display"));
  schemaOrg.name = displayName ? displayName.value : rorNames[0].value;

  // Legal name
  const legalName = rorNames.find(n =>
    n.types.includes("label") && n.lang === "en"
  );
  if (legalName) {
    schemaOrg.legalName = legalName.value;
  }

  // Alternate names
  const alternateNames = rorNames
    .filter(n => n.types.some(t => ["acronym", "alias"].includes(t)))
    .map(n => n.value);
  if (alternateNames.length > 0) {
    schemaOrg.alternateName = alternateNames;
  }

  return schemaOrg;
}
```

---

## Validation Examples

### Test Case 1: Verify Primary Name Mapping
```javascript
const input = [
  { "value": "MIT", "types": ["ror_display"], "lang": "en" },
  { "value": "Massachusetts Institute of Technology", "types": ["label"], "lang": "en" }
];

const result = mapRorNamesToSchema(input);
console.log(result.name); // "MIT"
console.log(result.legalName); // "Massachusetts Institute of Technology"
```

### Test Case 2: Handle Missing Display Name
```javascript
const input = [
  { "value": "Stanford University", "types": ["label"], "lang": "en" },
  { "value": "Stanford", "types": ["alias"], "lang": "en" }
];

const result = mapRorNamesToSchema(input);
console.log(result.name); // "Stanford University" (falls back to first)
console.log(result.alternateName); // ["Stanford"]
```

---

*Example created: 2024-09-15*
*Based on mapping rules: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:22-27*
*Related analysis: names/analysis.md*