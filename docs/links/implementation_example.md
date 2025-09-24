# ROR `links` to Schema.org Implementation Example

## Overview

This implementation demonstrates the semantic separation approach for mapping ROR organizational links to Schema.org properties, distinguishing between official websites and external identity references.

**Mapping Approach**: Semantic Separation - website URLs to `schema:url`, Wikipedia URLs to `schema:sameAs`
**Target Institution**: University of California System
**Focus**: Type-based property selection, array handling, semantic preservation

---

## Source ROR Record

Based on ROR schema v2.1 structure:

```json
{
  "id": "https://ror.org/00pjdza24",
  "names": [
    {
      "value": "University of California System",
      "types": ["ror_display", "label"],
      "lang": "en"
    }
  ],
  "links": [
    {
      "type": "website",
      "value": "http://www.universityofcalifornia.edu/"
    },
    {
      "type": "wikipedia",
      "value": "http://en.wikipedia.org/wiki/University_of_California"
    }
  ],
  "types": ["education"]
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Using semantic separation approach:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "identifier": "https://ror.org/00pjdza24",
  "name": "University of California System",
  "url": [
    "http://www.universityofcalifornia.edu/"
  ],
  "sameAs": [
    "http://en.wikipedia.org/wiki/University_of_California"
  ]
}
```

### Enhanced Implementation

With multiple links of each type (hypothetical example):

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "identifier": "https://ror.org/00pjdza24",
  "name": "University of California System",
  "url": [
    "http://www.universityofcalifornia.edu/",
    "https://www.universityofcalifornia.edu/about-us"
  ],
  "sameAs": [
    "http://en.wikipedia.org/wiki/University_of_California",
    "https://en.wikipedia.org/wiki/University_of_California_System"
  ]
}
```

---

## Mapping Logic Breakdown

### 1. Website URL Processing

**Applied Rule**: `ROR:links[type=website].value` → `schema:url`

```
Source: {"type": "website", "value": "http://www.universityofcalifornia.edu/"}
Target: "url": ["http://www.universityofcalifornia.edu/"]
Confidence: 0.94
```

**Rationale**: Official organization website represents the entity's primary web presence, making `schema:url` the semantically appropriate property.

### 2. Wikipedia URL Processing

**Applied Rule**: `ROR:links[type=wikipedia].value` → `schema:sameAs`

```
Source: {"type": "wikipedia", "value": "http://en.wikipedia.org/wiki/University_of_California"}
Target: "sameAs": ["http://en.wikipedia.org/wiki/University_of_California"]
Confidence: 0.95
```

**Rationale**: Wikipedia pages are external references that unambiguously identify the same entity, perfectly matching `schema:sameAs` semantics.

### 3. Type-Based Filtering Logic

```javascript
// Pseudocode for mapping implementation
const websites = rorRecord.links
  .filter(link => link.type === "website")
  .map(link => link.value);

const wikipediaPages = rorRecord.links
  .filter(link => link.type === "wikipedia")
  .map(link => link.value);

const schemaOrg = {
  "@context": "https://schema.org",
  "@type": "Organization",
  "url": websites.length > 0 ? websites : undefined,
  "sameAs": wikipediaPages.length > 0 ? wikipediaPages : undefined
};
```

---

## Reverse Mapping: Schema.org to ROR

### Example Schema.org Input

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "identifier": "https://ror.org/00pjdza24",
  "name": "University of California System",
  "url": [
    "http://www.universityofcalifornia.edu/",
    "https://www.universityofcalifornia.edu/about-us"
  ],
  "sameAs": [
    "http://en.wikipedia.org/wiki/University_of_California",
    "https://wikidata.org/entity/Q132765",
    "https://en.wikipedia.org/wiki/University_of_California_System"
  ]
}
```

### ROR Links Reconstruction

```javascript
// Pseudocode for reverse mapping
const rorLinks = [];

// Map schema:url values to website type
if (schemaOrg.url) {
  const urlArray = Array.isArray(schemaOrg.url) ? schemaOrg.url : [schemaOrg.url];
  urlArray.forEach(urlValue => {
    rorLinks.push({
      "type": "website",
      "value": urlValue
    });
  });
}

// Map schema:sameAs values to appropriate types
if (schemaOrg.sameAs) {
  const sameAsArray = Array.isArray(schemaOrg.sameAs) ? schemaOrg.sameAs : [schemaOrg.sameAs];
  sameAsArray.forEach(sameAsValue => {
    if (sameAsValue.includes('wikipedia.org/wiki/')) {
      rorLinks.push({
        "type": "wikipedia",
        "value": sameAsValue
      });
    }
    // Note: Non-Wikipedia sameAs values (like Wikidata)
    // would map to external_ids, not links
  });
}
```

### Resulting ROR Links Structure

```json
{
  "links": [
    {
      "type": "website",
      "value": "http://www.universityofcalifornia.edu/"
    },
    {
      "type": "website",
      "value": "https://www.universityofcalifornia.edu/about-us"
    },
    {
      "type": "wikipedia",
      "value": "http://en.wikipedia.org/wiki/University_of_California"
    },
    {
      "type": "wikipedia",
      "value": "https://en.wikipedia.org/wiki/University_of_California_System"
    }
  ]
}
```

### Reverse Mapping Considerations

- **URL Pattern Recognition**: Wikipedia URLs can be identified by domain pattern matching
- **Type Assignment**: All `schema:url` values become `type: "website"` in ROR
- **Filtering**: Non-Wikipedia `sameAs` values should be mapped to `external_ids` instead of `links`
- **Validation**: Ensure reconstructed links conform to ROR schema constraints

---

## Implementation Benefits

### Semantic Clarity
- Official websites correctly identified as primary organization URLs
- External references properly categorized as identity assertions
- Clear distinction preserved for different consumers

### Standards Compliance
- Follows Schema.org property semantics
- Aligns with Google's Organization markup guidelines
- Uses documented JSON-LD array patterns

### Scalability
- Handles multiple websites naturally (e.g., main site, department sites)
- Supports multiple Wikipedia pages (e.g., different languages, disambiguation)
- Extensible to additional link types if ROR schema expands

---

## Implementation Notes

### Technical Considerations
- Array properties should omit empty arrays rather than including `[]`
- Single values can be provided as arrays for consistency
- URL validation should be performed during mapping process
- HTTP/HTTPS protocol differences should be preserved as-is from ROR

### Best Practices
- Filter links by type before mapping to appropriate Schema.org properties
- Preserve ROR URL values exactly without modification
- Use arrays consistently even for single values to simplify consumer processing
- Include both properties only when corresponding link types exist

### Edge Cases
- Organizations with no website links: omit `url` property entirely
- Organizations with no Wikipedia page: omit `sameAs` property entirely
- Invalid URLs in ROR data: should be handled by data validation layer
- Mixed case in link types: ROR schema enforces lowercase enum values

---

## Validation Against Mapping Rules

This example demonstrates key aspects of the Semantic Separation approach:

✅ **Type Preservation**: Link types encoded through Schema.org property selection rather than additional metadata
✅ **Semantic Accuracy**: Website URLs use `url`, Wikipedia pages use `sameAs` per Schema.org semantics
✅ **Array Support**: Both properties demonstrated with single values in arrays, extensible to multiple values
✅ **Consumer Compatibility**: Uses standard Schema.org patterns recognized by major search engines
✅ **Consistency**: Aligns with external_ids → sameAs mapping pattern used elsewhere in the mapping set
✅ **Bidirectional Mapping**: Demonstrates how to reconstruct ROR links from Schema.org properties

---

*Example created: 2024-09-24*
*Based on mapping rules: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv lines 45-46*
*Related analysis: docs/links/analysis.md*