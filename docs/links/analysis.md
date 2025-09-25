# ROR `links` to Schema.org Mapping Analysis

## Overview

The ROR `links` field contains an array of external web references for organizations, specifically websites and Wikipedia pages, that provide additional identity information and official presence indicators.

## ROR Field Definition

**Field:** `links`
- **Type:** Array of objects with `type` and `value` properties
- **Default:** `[]` (empty array)
- **Semantic meaning:** External web links that identify or represent the organization
- **Schema location:** `/properties/links` in ROR Schema v2.1
- **Examples:** Website URLs, Wikipedia page URLs

### ROR Schema Structure

```json
"links": {
    "type": "array",
    "uniqueItems": true,
    "default": [],
    "items": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
            "value": {
                "type": "string"
            },
            "type": {
                "type": "string",
                "enum": [
                    "website",
                    "wikipedia"
                ]
            }
        },
        "required": [
            "type",
            "value"
        ]
    }
}
```

## Schema.org Analysis

### Target Properties: `url` and `sameAs`

**`schema:url`**
- **Property type:** URL
- **Domain:** Thing (applies to all entities)
- **Range:** URL
- **Comment:** URL of the item
- **Cardinality:** Multiple values supported in JSON-LD arrays

**`schema:sameAs`**
- **Property type:** URL
- **Domain:** Thing (applies to all entities)
- **Range:** URL
- **Comment:** URL of a reference Web page that unambiguously indicates the item's identity
- **Cardinality:** Multiple values supported in JSON-LD arrays

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: All links to url
- **Mapping**: `ROR:links.value` → `schema:url`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.80
- **Pros**: Simple, direct mapping of all URLs
- **Cons**: Loses semantic distinction between website types, doesn't align with Schema.org intent

#### Option 2: All links to sameAs
- **Mapping**: `ROR:links.value` → `schema:sameAs`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.85
- **Pros**: Treats all links as identity references
- **Cons**: Official websites are not external identity references

#### Option 3: Structured URL objects with additionalType
- **Mapping**: `ROR:links` → `schema:sameAs` with URL objects containing `additionalType`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.70
- **Pros**: Preserves complete ROR structure
- **Cons**: Not supported by Schema.org consumers, non-standard practice

#### Option 4: Semantic Separation (Chosen)
- **Mapping**:
  - `ROR:links[type=website].value` → `schema:url`
  - `ROR:links[type=wikipedia].value` → `schema:sameAs`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.94-0.95
- **Pros**: Preserves semantic intent, follows Schema.org best practices, handles multiplicity
- **Cons**: Requires type-based filtering during mapping

## Recommendation

**Recommended mapping:** Semantic Separation Approach
- `ROR:links[type=website].value` → `schema:url`
- `ROR:links[type=wikipedia].value` → `schema:sameAs`

### Rationale

1. **Semantic Accuracy**: Website URLs represent the organization's own web presence (`schema:url`), while Wikipedia pages are external identity references (`schema:sameAs`)

2. **Standard Compliance**: Follows documented Schema.org patterns and Google's recommended practices for Organization markup

3. **Multiplicity Support**: Both properties support JSON-LD arrays, handling multiple websites or Wikipedia pages naturally

4. **Consumer Compatibility**: Aligns with how major consumers (Google, search engines) process structured data

5. **Consistency**: Matches the existing pattern where `external_ids` maps to `sameAs` for identity references

### Updated Mapping

```tsv
ROR:links[type=website].value	Organization Website URLs	skos:exactMatch	schema:url	url	Semantic equivalence - official organization websites	https://orcid.org/0000-0002-0465-1009	0.94	Website URLs as primary organization URL array - semantic separation preserves intent
ROR:links[type=wikipedia].value	Wikipedia Reference URLs	skos:exactMatch	schema:sameAs	sameAs	Semantic equivalence - Wikipedia identity references	https://orcid.org/0000-0002-0465-1009	0.95	Wikipedia pages as external identity references - handles multiple pages in array
```

## Confidence Rationale

**Website URLs → schema:url: 0.94** based on:
- Direct semantic alignment (0.95)
- Standard practice alignment (0.95)
- Minor complexity for type filtering (-0.01)

**Wikipedia URLs → schema:sameAs: 0.95** based on:
- Perfect semantic match for identity references (0.98)
- Established pattern consistency (0.95)
- Standard consumer support (0.95)

## Key Decisions Made

### Problem Resolution
- **Initial Issue**: Original mapping created redundancy by mapping both `ROR:links` array and `ROR:links.value` to `schema:url`
- **Solution**: Adopted semantic separation based on link type rather than structural preservation

### Type Preservation Strategy
- **Considered**: Complex URL objects with `additionalType` properties
- **Rejected**: Not supported by Schema.org consumers in practice
- **Adopted**: Semantic property selection where the chosen property inherently encodes the URL type

### Array Handling
- **Confirmed**: Both `schema:url` and `schema:sameAs` support JSON-LD arrays
- **Implementation**: Multiple websites go to `url` array, multiple Wikipedia pages go to `sameAs` array

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org url Property](https://schema.org/url)
- [Schema.org sameAs Property](https://schema.org/sameAs)
- [Google Organization Markup Guidelines](https://developers.google.com/search/docs/appearance/structured-data/organization)

---

*Analysis conducted: 2025-09-24*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: External IDs mapping (similar sameAs pattern)*