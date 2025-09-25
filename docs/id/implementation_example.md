# ROR `id` to Schema.org Implementation Example

## Overview

This implementation demonstrates the PropertyValue-based approach for mapping ROR primary identifiers to Schema.org, ensuring structural consistency with external identifier mappings while maintaining clear semantic distinction.

**Mapping Approach**: Primary identifier as PropertyValue with propertyID="ror"
**Target Institution**: University of California System
**Focus**: Uniform identifier array structure and bidirectional mapping capability

---

## Source ROR Record

Based on ROR schema v2.1 structure:

```json
{
  "id": "https://ror.org/00dmfq477",
  "name": "University of California System",
  "external_ids": [
    {
      "type": "wikidata",
      "all": ["Q861"]
    }
  ]
}
```

---

## Schema.org Mapping Output

### Unified PropertyValue Implementation

Using PropertyValue structure for all identifiers:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00dmfq477",
  "identifier": [
    {
      "@type": "PropertyValue",
      "propertyID": "ror",
      "value": "https://ror.org/00dmfq477"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "wikidata",
      "value": ["Q861"]
    }
  ],
  "name": "University of California System"
}
```

---

## Mapping Logic Breakdown

### 1. Primary Identifier Mapping

**Applied Rule**: `ROR:id` → `schema:identifier` (PropertyValue)

```
Source: "https://ror.org/00dmfq477"
Target: PropertyValue with propertyID="ror"
Confidence: 0.95
```

**Rationale**: PropertyValue structure enables semantic distinction from external identifiers while maintaining uniform array format. The `propertyID="ror"` clearly marks this as the primary organizational identifier.

### 2. Structural Consistency

**Applied Rule**: Uniform PropertyValue format across identifier array

```
Before: Mixed array [string, object, object]
After: Uniform array [PropertyValue, PropertyValue, PropertyValue]
Benefits: Clean structure, consistent processing
```

---

## Reverse Mapping: Schema.org to ROR

### Example Schema.org Input

```json
{
  "identifier": [
    {
      "@type": "PropertyValue",
      "propertyID": "ror",
      "value": "https://ror.org/00dmfq477"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "wikidata",
      "value": ["Q861"]
    }
  ]
}
```

### ROR Identifier Reconstruction

**Step 1**: Filter PropertyValue objects by `propertyID="ror"`
**Step 2**: Extract `value` property
**Step 3**: Validate ROR ID format pattern

### Resulting ROR ID Structure

```json
{
  "id": "https://ror.org/00dmfq477"
}
```

## Implementation Benefits

### Structural Integrity
- Eliminates mixed-type identifier arrays
- Provides consistent processing patterns
- Maintains Schema.org PropertyValue best practices

### Semantic Clarity
- Primary identifier clearly distinguished by `propertyID="ror"`
- External identifiers marked by their respective system types
- No ambiguity in identifier roles or sources

---

## Implementation Notes

### Technical Considerations
- PropertyValue requires `@type` declaration for proper JSON-LD processing
- `propertyID` values should be standardized strings for consistency
- Value property can contain arrays for external identifiers but single values for ROR IDs

### Best Practices
- Always use `propertyID="ror"` for ROR primary identifiers
- Maintain consistent PropertyValue structure across all identifier types
- Include `@type: "PropertyValue"` for JSON-LD compliance

### Edge Cases
- Handle null or empty ROR IDs by excluding from identifier array
- Validate ROR ID pattern before mapping to ensure data integrity
- Consider versioning implications for schema updates

---

## Validation Against Mapping Rules

This example demonstrates key aspects of the Unified PropertyValue Approach:

✅ **Structural Consistency**: All identifiers use PropertyValue format eliminating mixed arrays
✅ **Semantic Preservation**: Primary identifier clearly distinguished from external references
✅ **Bidirectional Integrity**: Perfect reverse mapping via propertyID filtering mechanism

## Bidirectional Mapping Process

### Forward Mapping (ROR → Schema.org)
1. Extract ROR `id` value
2. Create PropertyValue with `propertyID="ror"`
3. Set `value` to ROR ID string
4. Include in identifier array

### Reverse Mapping (Schema.org → ROR)
1. Filter identifier array for `propertyID="ror"`
2. Extract `value` from matching PropertyValue
3. Validate against ROR pattern `^https://ror.org/0[a-z|0-9]{8}$`
4. Assign to ROR `id` field

### Distinction from External Identifiers

**Key Differentiator**: The `propertyID` value
- `propertyID="ror"` → Primary ROR identifier
- `propertyID="wikidata|grid|isni|fundref"` → External cross-reference identifiers

This approach ensures that when reverse mapping from Schema.org to ROR, the primary identifier is unambiguously separated from external reference identifiers, maintaining the semantic distinction present in the original ROR data structure.

---

*Example created: 2025-09-25*
*Based on mapping rules: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:30*
*Related analysis: docs/id/analysis.md*
