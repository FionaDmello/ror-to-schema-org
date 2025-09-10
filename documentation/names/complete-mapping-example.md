# Complete ROR to Schema.org Mapping Example

## Overview

This document provides a comprehensive example of mapping a real ROR institution record to Schema.org using the refined content-primary approach documented in `ror-names-mapping-analysis.md`.

**Mapping Approach**: Content-primary with conditional display logic
**Target Institution**: ETH Zurich (multilingual European institution)
**Focus**: Demonstrates multilingual name handling and semantic type mapping

---

## Source ROR Record (Simplified)

Based on ROR schema v2.1 structure:

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
      "value": "Scuola Politecnica Federale di Zurigo",
      "types": ["label"], 
      "lang": "it"
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
  ],
  "established": 1855,
  "types": ["education"],
  "locations": [
    {
      "geonames_details": {
        "name": "Zurich",
        "country_name": "Switzerland", 
        "country_code": "CH",
        "lat": 47.3769,
        "lng": 8.5417
      }
    }
  ],
  "external_ids": [
    {
      "type": "wikidata",
      "all": ["Q11942"],
      "preferred": "Q11942"
    }
  ]
}
```

---

## Schema.org Mapping Output

### Basic Implementation (Recommended)

Using core Schema.org properties with English as primary language:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "legalName": "Swiss Federal Institute of Technology Zurich", 
  "alternateName": ["ETH", "ETHZ"],
  "foundingDate": "1855",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Zurich",
    "addressCountry": "CH"
  },
  "geo": {
    "@type": "GeoCoordinates", 
    "latitude": 47.3769,
    "longitude": 8.5417
  },
  "sameAs": "https://www.wikidata.org/entity/Q11942"
}
```

### Enhanced Implementation with Multilingual Support

Preserving all language variants through structured metadata:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization", 
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "legalName": "Swiss Federal Institute of Technology Zurich",
  "alternateName": ["ETH", "ETHZ"],
  "foundingDate": "1855",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Zurich",
    "addressCountry": "CH"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 47.3769, 
    "longitude": 8.5417
  },
  "sameAs": "https://www.wikidata.org/entity/Q11942",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "ror-multilingual-label",
      "value": {
        "@type": "StructuredValue",
        "name": "Eidgenössische Technische Hochschule Zürich",
        "inLanguage": {
          "@type": "Language",
          "alternateName": "de"
        },
        "additionalType": "ror:label"
      }
    },
    {
      "@type": "PropertyValue",
      "name": "ror-multilingual-label",
      "value": {
        "@type": "StructuredValue",
        "name": "École Polytechnique Fédérale de Zurich",
        "inLanguage": {
          "@type": "Language",
          "alternateName": "fr"
        },
        "additionalType": "ror:label"
      }
    },
    {
      "@type": "PropertyValue",
      "name": "ror-multilingual-label",
      "value": {
        "@type": "StructuredValue", 
        "name": "Scuola Politecnica Federale di Zurigo",
        "inLanguage": {
          "@type": "Language",
          "alternateName": "it"
        },
        "additionalType": "ror:label"
      }
    },
    {
      "@type": "PropertyValue",
      "name": "ror-institution-type",
      "value": "education"
    }
  ]
}
```

---

## Mapping Logic Breakdown

### 1. Primary Name Resolution

**Applied Rule**: `ROR:names.value` → `schema:name`

```
Source: names[0].value = "ETH Zurich"  
Target: "name": "ETH Zurich"
Confidence: 0.95 (exact content match)
```

**Rationale**: The `value` field contains actual name content, making it the semantic primary regardless of the `ror_display` type.

### 2. Display Name Evaluation  

**Applied Rule**: `ROR:names.ror_display` → conditional mapping

```
Source: names[0] with types=["ror_display"], value="ETH Zurich"
Condition: ror_display value matches primary name value
Result: No separate mapping needed (already covered by names.value)
```

**Alternative scenario**: If `ror_display` contained different text, it would map to `schema:alternateName`.

### 3. Type-Based Semantic Mapping

**Legal Name**: `ROR:names[types=label]` → `schema:legalName`
```
Source: "Swiss Federal Institute of Technology Zurich" (English label)
Target: "legalName": "Swiss Federal Institute of Technology Zurich"
```

**Alternate Names**: `ROR:names[types=acronym|alias]` → `schema:alternateName`
```
Source: ["ETH", "ETHZ"] 
Target: "alternateName": ["ETH", "ETHZ"]
```

### 4. Multilingual Name Preservation

**Core Properties**: Use primary language (English) forms
**Extended Metadata**: Preserve all language variants in `additionalProperty`

This approach ensures:
- Search engines get clean, primary language data
- Full multilingual information is preserved for specialized applications
- ROR type semantics are maintained

---

## Implementation Benefits

### Semantic Accuracy
- **Content drives mapping**: `names.value` correctly maps to `schema:name`
- **Type preservation**: ROR semantic distinctions maintained through appropriate Schema.org properties
- **Conditional logic**: Handles complex display scenarios appropriately

### Implementation Simplicity  
- **Direct property mapping**: No complex nested structures required
- **Standard Schema.org patterns**: Follows established organizational markup conventions
- **Multilingual flexibility**: Core properties remain simple, extended data available when needed

### Search Optimization
- **Primary language focus**: Core properties use widely-understood English forms
- **Structured data compliance**: Follows Schema.org best practices for educational organizations
- **Rich snippets support**: Provides data in format expected by search engines

---

## Alternative Language Strategies

### HTML Microdata with Language Tags

```html
<div itemscope itemtype="https://schema.org/EducationalOrganization">
  <span itemprop="name" lang="en">ETH Zurich</span>
  <span itemprop="legalName" lang="en">Swiss Federal Institute of Technology Zurich</span>
  <span itemprop="alternateName" lang="de">Eidgenössische Technische Hochschule Zürich</span>
  <span itemprop="alternateName" lang="fr">École Polytechnique Fédérale de Zurich</span>
  <span itemprop="alternateName" lang="it">Scuola Politecnica Federale di Zurigo</span>
</div>
```

### JSON-LD with Language Objects

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "@language": "en"
  },
  "@type": "EducationalOrganization",
  "name": {
    "@value": "ETH Zurich",
    "@language": "en"
  },
  "legalName": [
    {
      "@value": "Swiss Federal Institute of Technology Zurich", 
      "@language": "en"
    },
    {
      "@value": "Eidgenössische Technische Hochschule Zürich",
      "@language": "de" 
    },
    {
      "@value": "École Polytechnique Fédérale de Zurich",
      "@language": "fr"
    }
  ]
}
```

---

## Validation Against Mapping Rules

This example demonstrates all key aspects of the refined content-primary mapping approach:

✅ **Content precedence**: `names.value` drives `schema:name` mapping  
✅ **Conditional display**: `ror_display` evaluated contextually  
✅ **Type semantics**: ROR name types map to appropriate Schema.org properties  
✅ **Multilingual support**: All language variants preserved appropriately  
✅ **Implementation flexibility**: Both simple and enhanced approaches provided  

The example validates that the content-primary approach provides superior semantic accuracy compared to the original display-primary or ItemList approaches.

---

*Example created: 2024-09-10*  
*Based on mapping rules: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:22-27*  
*Related analysis: documentation/names/ror-names-mapping-analysis.md*