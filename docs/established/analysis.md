# ROR `established` Field Mapping Analysis

## Overview

Analysis of mapping ROR's `established` field to Schema.org properties, focusing on both semantic compatibility and type safety requirements.

## ROR Schema Definition

**Field:** `established`
- **Type:** `null` or `number` 
- **Default:** `null`
- **Semantic meaning:** Numerical year when the organization was established (e.g., `1985`)
- **Schema location:** Line 70-75 in `ror_schema_v2_1.json`

## Schema.org Candidate Properties

### Initially Considered: `foundingDate`

- **Property:** `schema:foundingDate`
- **Domain:** `schema:Organization` ✅
- **Range:** `schema:Date` ❌ 
- **Comment:** "The date that this organization was founded"
- **Semantic match:** Perfect ✅
- **Type match:** Failed ❌ (expects Date, not number)

### Alternative Properties Evaluated

**`schema:dateCreated`:**
- **Domain:** `CreativeWork`, `DataFeedItem` ❌
- **Issue:** Wrong domain - not for organizations

**`schema:dissolutionDate`:**
- **Domain:** `schema:Organization` ✅
- **Range:** `schema:Date` ❌
- **Issue:** Opposite semantic meaning (end vs start)

## Type Compatibility Analysis

### The Core Problem

ROR stores establishment as:
```json
{
  "established": 1985
}
```

Schema.org expects Date format:
```json
{
  "foundingDate": "1985-01-01"  // or "1985" 
}
```

**Key Finding:** No Schema.org property exists that:
1. Has semantic meaning of "establishment/founding year" ✅
2. Accepts `Number`/`Integer` type ✅  
3. Has `schema:Organization` in domain ✅

## Recommended Solution

### Use `additionalProperty` with PropertyValue

**Mapping:**
- **Subject:** `ROR:established`
- **Object:** `schema:additionalProperty`  
- **Predicate:** `skos:exactMatch`
- **Confidence:** 0.90

**Implementation Structure:**
```json
{
  "@type": "Organization",
  "additionalProperty": {
    "@type": "PropertyValue", 
    "name": "established",
    "value": 1985,
    "description": "Year the organization was established"
  }
}
```

**Advantages:**
- ✅ Preserves exact semantic meaning
- ✅ Type-safe (PropertyValue accepts numeric values)
- ✅ Extensible and well-documented approach
- ✅ Maintains data fidelity

## Alternative Approaches Considered

### Option 1: Type Conversion to foundingDate
- Convert number to Date string (e.g., `1985` → `"1985-01-01"`)
- **Pros:** Uses semantic equivalent property
- **Cons:** Data transformation required, introduces assumptions about date precision

### Option 2: Text Representation  
- Store as text in additionalProperty
- **Pros:** Simple implementation
- **Cons:** Loses numeric typing, reduces queryability

### Option 3: No Mapping
- Document as unmappable due to type constraints
- **Cons:** Loses valuable temporal information

## Confidence Rationale

**Confidence: 0.90** based on:
- Perfect semantic preservation (1.0)
- Type safety maintained (1.0) 
- Uses intended Schema.org pattern for custom properties (0.9)
- Slight reduction for not using direct temporal property (-0.1)

## Implementation Notes

When implementing this mapping:
1. Use PropertyValue structure to preserve semantics
2. Include descriptive name field for clarity
3. Consider adding unitText: "year" for additional context
4. Maintain numeric data type for computational use

## Conclusion

The `additionalProperty` approach provides the optimal balance of semantic accuracy and type safety when strict type+semantic matching is required. While not as elegant as a direct temporal property mapping, it preserves all original information while remaining fully compliant with Schema.org specifications.