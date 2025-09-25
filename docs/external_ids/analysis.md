# ROR `external_ids` to Schema.org Mapping Analysis

## Overview

The ROR external_ids field contains cross-reference identifiers from other authoritative systems, enabling interoperability and linkage between ROR and established identifier frameworks like Wikidata, GRID, ISNI, and FundRef.

## ROR Field Definition

**Field:** `external_ids`
- **Type:** array of objects
- **Default:** []
- **Semantic meaning:** Cross-reference identifiers from other authoritative organizational identifier systems
- **Schema location:** Root level property, optional field
- **Examples:** Wikidata IDs, GRID IDs, ISNI codes, FundRef IDs

### ROR Schema Structure

```json
"external_ids": {
    "type": "array",
    "uniqueItems": true,
    "default": [],
    "items": {
        "type": "object",
        "properties": {
            "type": {
                "type": "string",
                "enum": ["fundref", "grid", "isni", "wikidata"]
            },
            "all": {
                "type": "array",
                "items": {"type": "string"}
            },
            "preferred": {
                "type": ["string", "null"]
            }
        },
        "required": ["all", "type"]
    }
}
```

## Schema.org Analysis

### Target Properties

#### Primary: `identifier` (PropertyValue structure)
- **Property type:** rdf:Property
- **Domain:** Thing
- **Range:** Text, URL, PropertyValue
- **Comment:** "The identifier property represents any kind of identifier for any kind of Thing"
- **Cardinality:** Multiple

#### Component Properties:
- **propertyID**: "A commonly used identifier for the characteristic represented by the property"
- **value**: "The value of a property value node"
- **valueReference**: "A secondary value that provides additional information on the original value"

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: sameAs Approach
- **Mapping**: `ROR:external_ids` → `schema:sameAs`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.70
- **Pros**: Simple, direct semantic equivalence
- **Cons**: Requires URL conversion; loses structural metadata; information loss on preferred status

#### Option 2: additionalProperty Approach
- **Mapping**: `ROR:external_ids` → `schema:additionalProperty`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.85
- **Pros**: Preserves all metadata; no semantic violations
- **Cons**: Generic property usage; loses identifier semantics

#### Option 3: PropertyValue Identifier Approach
- **Mapping**: `ROR:external_ids` → `schema:identifier` (PropertyValue)
- **Predicate**: skos:exactMatch
- **Confidence**: 0.93
- **Pros**: Semantically correct; preserves metadata; bidirectionally sound; uniform with ROR.id
- **Cons**: Complex nested structure

## Recommendation

**Recommended mapping:** `ROR:external_ids` → `schema:identifier` (PropertyValue with valueReference)

### Component Mappings

1. **external_ids.type** → `schema:propertyID` (identifies the identifier system)
2. **external_ids.all** → `schema:value` (array of all identifiers of this type)
3. **external_ids.preferred** → `schema:valueReference` (nested PropertyValue with preferred identifier)

### Rationale

1. **Semantic Correctness**: External identifiers are legitimate identifiers from authoritative systems, not just references
2. **Structural Integrity**: PropertyValue structure preserves all ROR metadata without loss
3. **Bidirectional Soundness**: Complete reconstruction of ROR structure possible from Schema.org output
4. **Unified Approach**: Consistent with ROR.id mapping using identical PropertyValue patterns

### Updated Mappings

## Confidence Rationale

**Overall Confidence: 0.93** based on:
- Semantic alignment (0.95) - External IDs are legitimate identifiers from authoritative systems
- Structural preservation (0.95) - All ROR metadata preserved through PropertyValue structure
- Bidirectional integrity (0.90) - Complete reverse mapping possible via nested valueReference
- Schema.org compliance (0.95) - Proper use of PropertyValue for complex identifiers

## Relationship to ROR.id

The external_ids and id mappings are designed as a unified identifier system:

### Unified Design Principles
- **Both map to `schema:identifier`** for semantic correctness
- **Both use PropertyValue structure** for consistency
- **Distinguished by `propertyID` values** for reverse mapping clarity

### Semantic Separation
- **ROR.id**: `propertyID="ror"` - Primary organizational identifier
- **External_ids**: `propertyID=system_type` - Cross-reference identifiers

### Bidirectional Recovery Process
1. **Primary ID Recovery**: Filter for `propertyID="ror"` → ROR.id
2. **External ID Recovery**: Filter for other propertyID values → external_ids array
3. **Preferred Status**: Extract from valueReference.value → preferred field
4. **System Type**: Extract from propertyID → type field
5. **All Values**: Extract from value array → all field

This unified approach ensures zero information loss and maintains clear semantic boundaries between primary and external identifiers.

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org identifier](https://schema.org/identifier)
- [Schema.org valueReference](https://schema.org/valueReference)

---

*Analysis conducted: 2025-09-25*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: docs/id/analysis.md*
