# ROR `id` to Schema.org Mapping Analysis

## Overview

The ROR identifier field serves as the primary, persistent identifier for organizations in the Research Organization Registry, following a standardized URL format that uniquely identifies each organization.

## ROR Field Definition

**Field:** `id`
- **Type:** string
- **Default:** "https://ror.org/012xzy7a9"
- **Semantic meaning:** Primary persistent identifier for the research organization
- **Schema location:** Root level property, required field
- **Examples:** "https://ror.org/012abc123", "https://ror.org/05dxps055"

### ROR Schema Structure

```json
"id": {
    "type": "string",
    "default": "https://ror.org/012xzy7a9",
    "pattern": "^https://ror.org/0[a-z|0-9]{8}$",
    "readOnly": true
}
```

## Schema.org Analysis

### Target Property: `identifier`

- **Property type:** rdf:Property
- **Domain:** Thing (available to all Schema.org types)
- **Range:** Text, URL, PropertyValue
- **Comment:** "The identifier property represents any kind of identifier for any kind of Thing, such as ISBNs, GTIN codes, UUIDs etc."
- **Cardinality:** Multiple (array supported)

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: Simple String Identifier
- **Mapping**: `ROR:id` → `schema:identifier` (as string)
- **Predicate**: skos:exactMatch
- **Confidence**: 0.95
- **Pros**: Simple, direct mapping; semantically correct
- **Cons**: Creates mixed-type array when combined with external identifiers

#### Option 2: PropertyValue Structure
- **Mapping**: `ROR:id` → `schema:identifier` (as PropertyValue with propertyID="ror")
- **Predicate**: skos:exactMatch
- **Confidence**: 0.95
- **Pros**: Uniform identifier array structure; clear distinction from external IDs; bidirectionally sound
- **Cons**: Slightly more complex structure

#### Option 3: Separate Property
- **Mapping**: `ROR:id` → `schema:url` or dedicated property
- **Predicate**: skos:relatedMatch
- **Confidence**: 0.70
- **Pros**: Maintains separation from other identifiers
- **Cons**: Semantic mismatch; loses primary identifier status

## Recommendation

**Recommended mapping:** `ROR:id` → `schema:identifier` (PropertyValue structure)

### Rationale

1. **Structural Consistency**: Maintains uniform PropertyValue structure across all identifiers, eliminating mixed-type arrays
2. **Semantic Clarity**: Primary identifier is clearly distinguished by `propertyID="ror"` from external identifiers
3. **Bidirectional Integrity**: Enables perfect reverse mapping by identifying ROR ID through propertyID value

### Updated Mapping

```tsv
ROR:id	ROR Identifier	skos:exactMatch	schema:identifier	identifier	Primary identifier as PropertyValue with propertyID='ror' for structural consistency	https://orcid.org/0000-0002-0465-1009	0.95	ROR ID wrapped in PropertyValue structure to maintain uniform identifier array format
```

## Confidence Rationale

**Confidence: 0.95** based on:
- Semantic correctness (0.95) - Perfect match between ROR ID and Schema.org identifier concept
- Structural integrity (0.95) - PropertyValue structure maintains consistency and bidirectionality
- Implementation clarity (0.95) - Clear distinction mechanism via propertyID for reverse mapping

## Relationship to External Identifiers

The ROR.id and external_ids mappings are intrinsically linked as they both map to `schema:identifier` but serve different semantic roles:

- **ROR.id**: Primary organizational identifier (propertyID="ror")
- **External_ids**: Cross-reference identifiers from other systems (propertyID=system type)

This unified approach ensures:
- Consistent identifier array structure
- Clear semantic separation through propertyID values
- Perfect bidirectional mapping capability
- Zero information loss during conversion

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Thing](https://schema.org/Thing)
- [Schema.org identifier](https://schema.org/identifier)

---

*Analysis conducted: 2025-09-25*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: docs/external_ids/analysis.md*