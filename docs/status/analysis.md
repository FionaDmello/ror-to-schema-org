# ROR `status` to Schema.org Mapping Analysis

## Overview

Analysis of mapping ROR's `status` field to Schema.org properties, addressing type incompatibility and semantic misalignment with the original mapping.

## Current Mapping (Line 26)

```tsv
ROR:status → schema:status (skos:relatedMatch, confidence: 0.60)
```

## ROR Field Definition

**Field:** `status`
- **Type:** string (enum)
- **Default:** `"active"`
- **Semantic meaning:** Registry operational status of the organization
- **Schema location:** `/json/ror/ror_schema_v2_1.json:327-335`
- **Examples:** `"active"`, `"inactive"`, `"withdrawn"`

### ROR Schema Structure

```json
"status": {
    "type": "string",
    "default": "active",
    "enum": [
        "active",
        "inactive",
        "withdrawn"
    ]
}
```

**Semantic meaning:**
- `active`: Organization is currently operational in the ROR registry
- `inactive`: Organization is no longer active but record preserved
- `withdrawn`: Organization record has been withdrawn from registry

## Schema.org Analysis

### Target Property: `status`

- **Property type:** Text/Enumeration
- **Domain:** MedicalStudy, MedicalCondition, MedicalProcedure
- **Range:** EventStatusType, MedicalStudyStatus, Text
- **Comment:** Domain-specific status property
- **Cardinality:** Single value

## Mapping Evaluation

### ✅ Current Mapping Strengths
- Simple direct property mapping concept

### ❌ Current Mapping Issues
- **Non-existent property**: `schema:Thing.status` does not exist in Schema.org vocabulary
- **Domain mismatch**: Available status properties are domain-specific to medical contexts
- **Type constraint violations**: Would violate Schema.org type constraints if applied to Organization
- **Low confidence**: 0.60 confidence reflected the poor semantic alignment

### Alternative Mappings Evaluated

#### Option 1: dissolutionDate
- **Mapping**: `ROR:status` → `schema:dissolutionDate`
- **Predicate**: `skos:narrowMatch`
- **Confidence**: 0.75
- **Pros**:
  - Semantically relevant for inactive/withdrawn states
- **Cons**:
  - Fundamental type mismatch (enum to date)
  - Only applicable to inactive/withdrawn, not active
  - Active organizations have no dissolution concept

#### Option 2: additionalType
- **Mapping**: `ROR:status` → `schema:additionalType`
- **Predicate**: `skos:relatedMatch`
- **Confidence**: 0.70
- **Pros**:
  - Creates semantic types for status
  - Standard Schema.org pattern for external vocabularies
- **Cons**:
  - Requires creating custom ROR vocabulary URIs
  - Loses direct enum value representation
  - More complex implementation

#### Option 3: disambiguatingDescription
- **Mapping**: `ROR:status` → `schema:disambiguatingDescription`
- **Predicate**: `skos:relatedMatch`
- **Confidence**: 0.65
- **Pros**:
  - Simple text representation
  - Human-readable
- **Cons**:
  - Loses structured/enumerated nature
  - More descriptive than semantic
  - No standardization of values

#### Option 4: additionalProperty
- **Mapping**: `ROR:status` → `schema:additionalProperty`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.80
- **Pros**:
  - Type-safe (PropertyValue handles structured data)
  - Semantic preservation (maintains exact ROR enum values)
  - Standard pattern (established Schema.org convention)
  - Extensible (can accommodate future ROR status values)
- **Cons**:
  - Slightly more complex JSON-LD structure
  - Custom property name not standardized

## Recommendation

**Recommended mapping:** `ROR:status` → `schema:additionalProperty`

### Rationale

1. **Type Safety**: PropertyValue structure handles enum values correctly
2. **Semantic Preservation**: Maintains exact ROR status semantics
3. **Standard Pattern**: Follows Schema.org conventions for custom metadata
4. **Extensibility**: Future ROR status values can be accommodated
5. **Higher Confidence**: Improved from 0.60 to 0.80 due to better semantic fit

### Updated Mapping

```tsv
ROR:status	Organization Registry Status	skos:exactMatch	schema:additionalProperty	additionalProperty	Exact mapping - registry status as structured property with enum value	https://orcid.org/0000-0002-0465-1009	0.80	Registry operational status preserved in PropertyValue structure
```

## Confidence Rationale

**Confidence: 0.80** based on:
- Semantic preservation (1.0)
- Type safety maintained (1.0)
- Uses intended Schema.org pattern for custom properties (0.9)
- Slight reduction for not using direct status property (-0.1)

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org additionalProperty](https://schema.org/additionalProperty)

---

*Analysis conducted: 2024-09-10*
*Updated mapping file: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:26*
*Related analysis: status/implementation_example.md*
