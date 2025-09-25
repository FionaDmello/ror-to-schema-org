# ROR `external_ids` to Schema.org Implementation Example

## Overview

**Mapping Approach**: External identifiers as PropertyValue objects with nested valueReference for preferred status
**Target Institution**: University of California, Berkeley
**Focus**: Preserving all external identifier metadata with bidirectional mapping

---

## Source ROR Record

```json
{
  "id": "https://ror.org/01an7q238",
  "external_ids": [
    {
      "type": "wikidata",
      "all": ["Q168756"],
      "preferred": "Q168756"
    },
    {
      "type": "grid",
      "all": ["grid.47840.3f", "grid.legacy.47840"],
      "preferred": "grid.47840.3f"
    }
  ]
}
```

---

## Schema.org Output

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/01an7q238",
  "identifier": [
    {
      "@type": "PropertyValue",
      "propertyID": "ror",
      "value": "https://ror.org/01an7q238"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "wikidata",
      "value": ["Q168756"],
      "valueReference": {
        "@type": "PropertyValue",
        "additionalType": "preferred",
        "value": "Q168756"
      }
    },
    {
      "@type": "PropertyValue",
      "propertyID": "grid",
      "value": ["grid.47840.3f", "grid.legacy.47840"],
      "valueReference": {
        "@type": "PropertyValue",
        "additionalType": "preferred",
        "value": "grid.47840.3f"
      }
    }
  ]
}
```

---

## Mapping Logic

| ROR Field | Schema.org Target | Example |
|-----------|------------------|---------|
| `external_ids.type` | `propertyID` | "wikidata" → `propertyID: "wikidata"` |
| `external_ids.all` | `value` | `["Q168756"]` → `value: ["Q168756"]` |
| `external_ids.preferred` | `valueReference.value` | "Q168756" → nested PropertyValue |

---

## Bidirectional Mapping

### ROR → Schema.org
1. Each external_ids object → PropertyValue in identifier array
2. `type` → `propertyID`
3. `all` → `value` array
4. `preferred` → `valueReference` with `additionalType="preferred"`

### Schema.org → ROR
1. Filter identifier array for non-ROR PropertyValues
2. `propertyID` → `type`
3. `value` → `all`
4. `valueReference.value` → `preferred`

### Distinguishing from ROR.id

**Key**: The `propertyID` value distinguishes identifier types:
- `propertyID="ror"` → Primary ROR identifier
- `propertyID="wikidata|grid|isni|fundref"` → External identifiers

---

## Implementation Notes

**Required Structure:**
- All identifiers use PropertyValue format for consistency
- valueReference preserves preferred status
- propertyID identifies the identifier system

**Edge Cases:**
- Missing `preferred` field → omit valueReference
- Empty `all` array → exclude from identifier array
- Multiple values in `all` → preserve as array in `value`

---

*Example created: 2025-09-25*
*Based on mapping rules: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:31-34*
*Related analysis: docs/external_ids/analysis.md*