# ROR Domains Field Mapping Analysis

## Overview

This document analyzes the mapping of the ROR `domains` field to Schema.org properties, examining the current mapping and evaluating alternatives based on official ROR documentation and Schema.org specifications.

## Current Mapping (Line 30)

```tsv
ROR:domains	Organization Domains Array	skos:exactMatch	schema:url	url	Semantic equivalence - web domains represent organization URLs	https://orcid.org/0000-0002-0465-1009	0.85	Array of domain names owned by organization
```

## ROR Domains Field Definition

According to official ROR documentation (https://ror.readme.io/docs/ror-data-structure):

> **domains**: "The domains registered to a particular institution, not including the protocol, path portions, or query parameters that may exist in the URL for the organization's website. An organization's website is stored in the links field."

### Key Properties

- **Type**: Array of strings
- **Pattern**: Domain format (e.g., `"unipi.it"`, `"universityofcalifornia.edu"`)
- **Purpose**: Persistent institutional identifiers for cross-system mapping
- **Separate from websites**: Organization websites stored in `links` field
- **Examples**: 
  - University of Pisa: `["unipi.it"]`
  - University of California: `["universityofcalifornia.edu"]`

## Schema Analysis

### ROR Schema (v2.1)
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

### Schema.org url Property
- **Expected types**: URL
- **Definition**: "URL of the item"
- **Cardinality**: Single value expected
- **Format**: Complete URL (e.g., `"https://example.org"`)

## Problems with Current Mapping

### 1. Semantic Mismatch
- **ROR domains**: Registered domains for institutional identification
- **Schema.org url**: Primary URL representing organization's web presence
- **ROR explicitly separates**: Domains vs. website URLs (in `links` field)

### 2. Type Incompatibility
- **ROR**: Array of domain strings `["domain.edu", "alt-domain.org"]`
- **Schema.org url**: Single URL string `"https://example.org"`

### 3. Format Mismatch
- **ROR**: Bare domains `"unipi.it"`
- **Schema.org url**: Full URLs `"https://unipi.it"`

### 4. Cardinality Issues
- **ROR**: Multiple domains per organization
- **Schema.org url**: Typically one primary URL

## Alternative Mappings Evaluated

### Option 1: schema:additionalProperty (Recommended)

**Mapping**: `ROR:domains` → `schema:additionalProperty`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.92
- **Reasoning**: 
  - Preserves array structure using PropertyValue
  - Maintains semantic meaning as institutional metadata
  - Type-safe without forced conversions
  - Handles multiple domains cleanly

**Implementation Example**:
```json
{
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": ["unipi.it", "unipisa.it"]
  }
}
```

### Option 2: Multiple schema:identifier instances

**Mapping**: Transform array to multiple `schema:identifier` properties
- **Predicate**: `skos:narrowMatch`
- **Confidence**: 0.78
- **Reasoning**: 
  - Domains serve as institutional identifiers
  - Loses grouped semantic meaning
  - Requires array transformation
  - Each domain becomes separate identifier

### Option 3: schema:url with transformation

**Mapping**: Convert domains to full URLs
- **Predicate**: `skos:narrowMatch`
- **Confidence**: 0.70
- **Reasoning**: 
  - Requires protocol assumption (https://)
  - Semantic mismatch (identity vs. web presence)
  - Loses distinction between domains and primary website
  - May create invalid URLs for inactive domains

### Option 4: schema:sameAs

**Mapping**: Convert domains to URLs for identity purposes
- **Predicate**: `skos:relatedMatch`
- **Confidence**: 0.75
- **Reasoning**: 
  - Better for identity than `url`
  - Still requires URL conversion
  - Semantic stretch (domains ≠ "same as" references)

## Recommendation

**Use `schema:additionalProperty`** as the mapping for ROR domains field.

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

## Implementation Notes

- Use PropertyValue with `name="registeredDomains"` for consistency
- Preserve array structure in `value` property
- Consider adding `identifier` property to PropertyValue for domain validation
- Document the semantic distinction from organization's primary website (in `links` field)

## References

- [ROR Data Structure Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org additionalProperty](https://schema.org/additionalProperty)