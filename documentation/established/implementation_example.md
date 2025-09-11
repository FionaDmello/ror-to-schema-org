# ROR Established Field to Schema.org Mapping Example

## Overview

This document provides a comprehensive example of mapping ROR's `established` field to Schema.org using the type-safe PropertyValue approach documented in `analysis.md`.

**Mapping Approach**: Type-safe semantic preservation via additionalProperty
**Target Institution**: MIT (established 1861)
**Focus**: Demonstrates handling numeric year data with type compatibility requirements

---

## Source ROR Record (Simplified)

Based on ROR schema v2.1 structure:

```json
{
  "id": "https://ror.org/042nb2s44",
  "names": [
    {
      "value": "Massachusetts Institute of Technology",
      "types": ["ror_display"],
      "lang": "en"
    },
    {
      "value": "MIT",
      "types": ["acronym"],
      "lang": "en"
    }
  ],
  "established": 1861,
  "types": ["education"],
  "locations": [
    {
      "geonames_details": {
        "name": "Cambridge",
        "country_name": "United States",
        "country_code": "US",
        "country_subdivision_name": "Massachusetts",
        "lat": 42.3601,
        "lng": -71.0942
      }
    }
  ]
}
```

**Key Field Analysis:**
- **`established`**: `1861` (number type)
- **Semantic meaning**: Year the organization was founded
- **Type constraint**: Must preserve numeric format for computational use

---

## Schema.org Mapping Output

### Basic Implementation (Recommended)

Using additionalProperty with PropertyValue structure:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/042nb2s44",
  "identifier": "https://ror.org/042nb2s44",
  "name": "Massachusetts Institute of Technology",
  "alternateName": "MIT",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Cambridge",
    "addressRegion": "Massachusetts",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 42.3601,
    "longitude": -71.0942
  },
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established",
    "value": 1861,
    "description": "Year the organization was established",
    "unitText": "year"
  }
}
```

### Enhanced Implementation with Multiple Properties

Demonstrating type-safe approach alongside other organizational metadata:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/042nb2s44",
  "identifier": "https://ror.org/042nb2s44",
  "name": "Massachusetts Institute of Technology",
  "alternateName": "MIT",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Cambridge",
    "addressRegion": "Massachusetts", 
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 42.3601,
    "longitude": -71.0942
  },
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "established",
      "value": 1861,
      "description": "Year the organization was established",
      "unitText": "year",
      "propertyID": "ROR:established"
    },
    {
      "@type": "PropertyValue",
      "name": "ror-institution-type",
      "value": "education",
      "description": "ROR organizational type classification"
    }
  ]
}
```

### Alternative Date-Compatible Implementation

For systems requiring Schema.org Date compliance (with data transformation):

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/042nb2s44",
  "identifier": "https://ror.org/042nb2s44", 
  "name": "Massachusetts Institute of Technology",
  "alternateName": "MIT",
  "foundingDate": "1861",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Cambridge",
    "addressRegion": "Massachusetts",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 42.3601,
    "longitude": -71.0942
  },
  "additionalProperty": {
    "@type": "PropertyValue", 
    "name": "established-original",
    "value": 1861,
    "description": "Original numeric year from ROR",
    "unitText": "year"
  }
}
```

---

## Mapping Logic Breakdown

### 1. Type Safety Analysis

**ROR Schema Definition**:
```json
"established": {
  "type": ["null", "number"],
  "default": null
}
```

**Schema.org foundingDate Requirements**:
```json
{
  "rangeIncludes": {"@id": "schema:Date"}
}
```

**Incompatibility**: `number` ≠ `Date` type

### 2. PropertyValue Solution

**Applied Rule**: `ROR:established` → `schema:additionalProperty`

```
Source: "established": 1861 (number)
Target: PropertyValue structure preserving type and semantics
Confidence: 0.90 (high semantic + type preservation)
```

**Benefits**:
- ✅ Preserves original numeric format
- ✅ Maintains computational usability
- ✅ Provides semantic context via description
- ✅ Follows Schema.org extension patterns

### 3. PropertyValue Structure Components

```json
{
  "@type": "PropertyValue",
  "name": "established",           // Field identifier
  "value": 1861,                   // Original numeric value
  "description": "Year the organization was established",  // Semantic context
  "unitText": "year",              // Unit specification
  "propertyID": "ROR:established"  // Source reference (optional)
}
```

---

## Implementation Benefits

### Type Safety
- **No data transformation**: Original ROR number format preserved
- **Schema.org compliance**: Uses approved PropertyValue pattern for custom data
- **Computational integrity**: Numeric operations remain possible

### Semantic Preservation
- **Clear meaning**: Description field explains semantic intent
- **Source attribution**: PropertyID links back to ROR schema
- **Unit specification**: UnitText clarifies temporal context

### Implementation Flexibility
- **Core approach**: Basic PropertyValue for simple implementations
- **Enhanced metadata**: Additional fields for rich applications
- **Hybrid option**: Dual mapping for systems requiring both formats

---

## Comparison with Rejected Approaches

### foundingDate Mapping (Rejected)

**Original mapping**: `ROR:established` → `schema:foundingDate`

```json
// Would require data transformation:
{
  "foundingDate": "1861-01-01"  // or "1861"
}
```

**Issues**:
- ❌ Type conversion required (number → Date)
- ❌ Precision assumptions (year → full date)
- ❌ Data transformation complexity
- ❌ Potential information loss

### Text Conversion (Rejected)

```json
{
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established", 
    "value": "1861"  // String instead of number
  }
}
```

**Issues**:
- ❌ Loses numeric typing
- ❌ Reduces computational utility
- ❌ Unnecessary data conversion

---

## Query and Usage Examples

### SPARQL Query for Established Years

```sparql
PREFIX schema: <https://schema.org/>

SELECT ?org ?name ?established WHERE {
  ?org a schema:Organization ;
       schema:name ?name ;
       schema:additionalProperty ?prop .
  
  ?prop a schema:PropertyValue ;
        schema:name "established" ;
        schema:value ?established .
  
  FILTER(?established > 1800)
}
ORDER BY ?established
```

### JavaScript Processing

```javascript
// Extract established year for calculations
function getEstablishedYear(organization) {
  const props = organization.additionalProperty || [];
  const establishedProp = props.find(p => 
    p['@type'] === 'PropertyValue' && 
    p.name === 'established'
  );
  
  return establishedProp ? establishedProp.value : null;
}

// Usage
const mit = /* organization JSON */;
const established = getEstablishedYear(mit);
const age = new Date().getFullYear() - established;
console.log(`MIT is ${age} years old`);
```

---

## Validation Against Mapping Requirements

This example demonstrates adherence to strict type+semantic matching:

✅ **Type compatibility**: PropertyValue accepts numeric values  
✅ **Semantic preservation**: Description maintains establishment meaning  
✅ **Data fidelity**: Original ROR data unchanged  
✅ **Schema.org compliance**: Uses standard extension pattern  
✅ **Implementation clarity**: Clear structure for developers  
✅ **Query support**: Enables structured data queries  

The PropertyValue approach provides the optimal solution when strict type safety is required while preserving full semantic meaning.

---

## Alternative Implementation Contexts

### HTML Microdata

```html
<div itemscope itemtype="https://schema.org/EducationalOrganization">
  <span itemprop="name">Massachusetts Institute of Technology</span>
  <div itemprop="additionalProperty" itemscope itemtype="https://schema.org/PropertyValue">
    <meta itemprop="name" content="established">
    <span itemprop="value">1861</span>
    <meta itemprop="description" content="Year the organization was established">
  </div>
</div>
```

### RDFa

```html
<div vocab="https://schema.org/" typeof="EducationalOrganization">
  <span property="name">Massachusetts Institute of Technology</span>
  <div property="additionalProperty" typeof="PropertyValue">
    <meta property="name" content="established">
    <span property="value" datatype="xsd:integer">1861</span>
    <meta property="description" content="Year the organization was established">
  </div>
</div>
```

---

*Example created: 2024-09-11*  
*Based on mapping rules: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:30*  
*Related analysis: documentation/established/analysis.md*