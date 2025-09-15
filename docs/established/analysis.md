# ROR `established` to Schema.org Mapping Analysis

## Overview

Analysis of mapping ROR's `established` field to Schema.org properties, focusing on both semantic compatibility and type safety requirements.

## Current Mapping (Line [LINE_NUMBER])

```tsv
[NEEDS_TO_BE_IDENTIFIED_FROM_SSSOM_FILE]
```

## ROR Field Definition

**Field:** `established`
- **Type:** `null` or `number`
- **Default:** `null`
- **Semantic meaning:** Numerical year when the organization was established (e.g., `1985`)
- **Schema location:** Line 70-75 in `ror_schema_v2_1.json`
- **Examples:** `1985`, `2003`, `null`

### ROR Schema Structure

```json
"established": {
    "type": ["null", "number"],
    "default": null
}
```

## Schema.org Analysis

### Target Property: `foundingDate`

- **Property type:** Date
- **Domain:** Organization
- **Range:** Date
- **Comment:** "The date that this organization was founded"
- **Cardinality:** Single value

## Mapping Evaluation

### ✅ Current Mapping Strengths
- Perfect semantic match for founding/establishment concept
- Appropriate domain (Organization)

### ❌ Current Mapping Issues
- **Type mismatch**: ROR uses number (year), Schema.org expects Date
- **Format incompatibility**: `1985` vs `"1985-01-01"` or `"1985"`

### Alternative Mappings Evaluated

#### Option 1: schema:foundingDate with conversion
- **Mapping**: `ROR:established` → `schema:foundingDate`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.85
- **Pros**:
  - Perfect semantic alignment
  - Uses intended temporal property
- **Cons**:
  - Requires data transformation (number to date string)
  - Introduces assumptions about date precision

#### Option 2: schema:additionalProperty
- **Mapping**: `ROR:established` → `schema:additionalProperty`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.90
- **Pros**:
  - Type-safe (PropertyValue accepts numeric values)
  - Preserves exact semantic meaning
  - Maintains data fidelity
- **Cons**:
  - Doesn't use semantic equivalent property
  - Slightly more complex structure

## Recommendation

**Recommended mapping:** `ROR:established` → `schema:additionalProperty`

### Rationale

1. **Type Safety**: PropertyValue structure handles numeric values correctly
2. **Semantic Preservation**: Maintains exact ROR establishment semantics
3. **Data Fidelity**: No loss of precision or assumptions about date format
4. **Schema Compliance**: Uses PropertyValue structure as intended
5. **Extensibility**: Can accommodate additional metadata if needed

### Updated Mapping

```tsv
ROR:established	Organization Established Year	skos:exactMatch	schema:additionalProperty	additionalProperty	Exact mapping - establishment year as structured property with numeric value	https://orcid.org/0000-0002-0465-1009	0.90	Numeric year preserved in PropertyValue structure
```

## Confidence Rationale

**Confidence: 0.90** based on:
- Perfect semantic preservation (1.0)
- Type safety maintained (1.0)
- Uses intended Schema.org pattern for custom properties (0.9)
- Slight reduction for not using direct temporal property (-0.1)

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org foundingDate](https://schema.org/foundingDate)

---

*Analysis conducted: 2024-09-10*
*Updated mapping file: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv*
*Related analysis: established/implementation_example.md*