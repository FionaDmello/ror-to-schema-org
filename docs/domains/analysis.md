# ROR `domains` to Schema.org Mapping Analysis

## Overview

Analysis of mapping ROR's `domains` field to Schema.org properties, examining the current mapping and evaluating alternatives based on official ROR documentation and Schema.org specifications.

## Current Mapping (Line 30)

```tsv
ROR:domains	Organization Domains Array	skos:exactMatch	schema:url	url	Semantic equivalence - web domains represent organization URLs	https://orcid.org/0000-0002-0465-1009	0.85	Array of domain names owned by organization
```

## ROR Field Definition

**Field:** `domains`
- **Type:** Array of strings
- **Default:** `[]`
- **Semantic meaning:** Domain names registered to a particular institution for cross-system mapping
- **Schema location:** ROR schema v2.1
- **Examples:** `["unipi.it"]`, `["universityofcalifornia.edu"]`

According to official ROR documentation (https://ror.readme.io/docs/ror-data-structure):

> **domains**: "The domains registered to a particular institution, not including the protocol, path portions, or query parameters that may exist in the URL for the organization's website. An organization's website is stored in the links field."

### ROR Schema Structure

```json
"domains": {
    "type": "array",
    "uniqueItems": true,
    "default": [],
    "items": {
        "type": "string",
        "pattern": "^((?=[a-z0-9-]{1,63}\\.)[a-z0-9]+(-[a-z0-9]+)*\\.)+[a-z]{2,63}$"
    }
}
```

## Schema.org Analysis

### Target Property: `url`

- **Property type:** URL
- **Domain:** Thing (all classes)
- **Range:** URL
- **Comment:** "URL of the item"
- **Cardinality:** Single value expected

## Mapping Evaluation

### ✅ Current Mapping Strengths
- Domains do represent web resources associated with the organization
- Simple conceptual connection between domains and URLs

### ❌ Current Mapping Issues
- **Semantic mismatch**: ROR domains are for institutional identification, Schema.org url is for web presence
- **Type incompatibility**: ROR array vs. Schema.org single URL
- **Format mismatch**: Bare domains vs. full URLs with protocol
- **Cardinality issues**: Multiple domains vs. typically one primary URL
- **ROR explicitly separates**: Domains vs. website URLs (stored in `links` field)

### Alternative Mappings Evaluated

#### Option 1: schema:additionalProperty
- **Mapping**: `ROR:domains` → `schema:additionalProperty`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.92
- **Pros**:
  - Preserves array structure using PropertyValue
  - Maintains semantic meaning as institutional metadata
  - Type-safe without forced conversions
  - Handles multiple domains cleanly
- **Cons**:
  - Slightly more complex JSON-LD structure
  - Custom property name not standardized

#### Option 2: Multiple schema:identifier instances
- **Mapping**: Transform array to multiple `schema:identifier` properties
- **Predicate**: `skos:narrowMatch`
- **Confidence**: 0.78
- **Pros**:
  - Domains serve as institutional identifiers
  - Standard Schema.org property
- **Cons**:
  - Loses grouped semantic meaning
  - Requires array transformation
  - Each domain becomes separate identifier

#### Option 3: schema:url with transformation
- **Mapping**: Convert domains to full URLs
- **Predicate**: `skos:narrowMatch`
- **Confidence**: 0.70
- **Pros**:
  - Uses intended Schema.org property for web resources
- **Cons**:
  - Requires protocol assumption (https://)
  - Semantic mismatch (identity vs. web presence)
  - Loses distinction between domains and primary website
  - May create invalid URLs for inactive domains

#### Option 4: schema:sameAs
- **Mapping**: Convert domains to URLs for identity purposes
- **Predicate**: `skos:relatedMatch`
- **Confidence**: 0.75
- **Pros**:
  - Better for identity than `url`
  - Standard identity property
- **Cons**:
  - Still requires URL conversion
  - Semantic stretch (domains ≠ "same as" references)

## Recommendation

**Recommended mapping:** `ROR:domains` → `schema:additionalProperty`

### Rationale

1. **Semantic Accuracy**: Preserves the meaning of domains as institutional metadata
2. **Type Safety**: Handles arrays without forced conversions
3. **Data Integrity**: Maintains all domain information without loss
4. **Schema Compliance**: Uses PropertyValue structure as intended
5. **Flexibility**: Can accommodate varying numbers of domains per organization

### Updated Mapping

```tsv
ROR:domains	Organization Domains Array	skos:exactMatch	schema:additionalProperty	additionalProperty	Exact mapping - registered domains as structured institutional metadata	https://orcid.org/0000-0002-0465-1009	0.92	Array of domain names preserved in PropertyValue structure for institutional identification
```

## Confidence Rationale

**Confidence: 0.92** based on:
- Semantic preservation (1.0)
- Type safety maintained (1.0)
- Uses intended Schema.org pattern for custom properties (0.9)
- Slight reduction for not using direct property mapping (-0.08)

## References

- [ROR Data Structure Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org additionalProperty](https://schema.org/additionalProperty)

---

*Analysis conducted: 2024-09-10*
*Updated mapping file: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:30*
*Related analysis: domains/implementation-example.md*