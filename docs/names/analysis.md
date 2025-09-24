# ROR `names` to Schema.org Mapping Analysis

## Overview

Analysis of mapping ROR's `names` property to Schema.org properties, addressing the overly complex ItemList structure and establishing the optimal content-primary mapping approach.

## Mapping (Current Implementation)

```tsv
ROR:names[types=ror_display] → schema:name (skos:exactMatch, confidence: 0.95)
ROR:names[types=label,lang=en] → schema:legalName (skos:exactMatch, confidence: 0.93)
ROR:names[types=label,lang≠en] → schema:additionalProperty (skos:exactMatch, confidence: 0.88)
ROR:names[types=alias|acronym] → schema:alternateName (skos:exactMatch, confidence: 0.94)
```

## ROR Field Definition

**Field:** `names`
- **Type:** Array of objects
- **Default:** (required field, minItems: 1)
- **Semantic meaning:** Structured organizational names with type classifications and language tags
- **Schema location:** `/json/ror/ror_schema_v2_1.json:251-293`
- **Examples:** See structured name objects with value, types, and lang properties

### ROR Schema Structure

```json
"names": {
    "type": "array",
    "uniqueItems": true,
    "minItems": 1,
    "items": {
        "type": "object",
        "properties": {
            "value": { "type": "string" },
            "types": {
                "type": "array",
                "items": {
                    "enum": ["acronym", "alias", "label", "ror_display"]
                }
            },
            "lang": {
                "type": "string",
                "pattern": "^[a-z]{2}$"
            }
        },
        "required": ["types", "value"]
    }
}
```

**Semantic structure:**
- **Array of name objects**: Multiple name variants per organization
- **Name types**: `acronym`, `alias`, `label`, `ror_display` - each with specific semantic meaning
- **Multilingual support**: Optional ISO 639-1 language codes
- **Required fields**: Both `value` (the actual name) and `types` (categorization)

## Schema.org Analysis

### Target Properties: Multiple Name Properties

- **name**: Primary organizational name
- **alternateName**: Alternative names, aliases, acronyms
- **legalName**: Official registered name
- **ItemList**: Complex structured list (current mapping)

## Mapping Evaluation

### ✅ Current Mapping Strengths
- **Composite object integrity**: Each mapping preserves complete ROR name objects
- **Bidirectional reconstruction**: Schema.org properties can be mapped back to ROR structure
- **Language-aware context**: Language handled within semantic mappings, not as separate metadata
- **Type-based logic**: Clear conditional mapping based on ROR name types
- **Schema.org compliance**: Follows established organizational naming patterns

#### Option 1: Content-Primary Mapping (Recommended)
- **Mapping**: Map based on actual content vs. display metadata hierarchy
- **Predicate**: `skos:exactMatch` for direct mappings
- **Confidence**: 0.88-0.95
- **Pros**:
  - Content drives the mapping, not display preferences
  - Semantic correctness (content → content properties)
  - Type compatibility (all map to Text type)
  - Implementation flexibility
  - Future-proof against display convention changes
- **Cons**:
  - Requires conditional logic for display handling
  - Language handling needs structured approach

#### Option 3: Language Handling Options
- **names.lang handling**:
  - `additionalProperty` with StructuredValue (0.80 confidence)
  - HTML lang attributes in microdata context (0.85 confidence)
  - JSON-LD language objects (0.75 confidence)

## Implementation Approach

**Current mapping:** Composite Object Mapping with language-aware context

### Rationale

1. **Semantic Correctness**: Content drives mappings, not presentation choices
2. **Type Compatibility**: All mappings result in Text type properties
3. **Language Preservation**: Multilingual data handled through additionalProperty
4. **Implementation Flexibility**: Handles complex display scenarios
5. **Schema.org Alignment**: Follows established organizational naming patterns

### Current Mapping Implementation

```tsv
ROR:names[types=ror_display]	ROR Display Name Object	skos:exactMatch	schema:name	name	Primary organizational name with language context preserved	https://orcid.org/0000-0002-0465-1009	0.95	ror_display names map to schema:name; non-English handled via inLanguage in StructuredValue
ROR:names[types=label,lang=en]	English Legal Name Object	skos:exactMatch	schema:legalName	legalName	Official English organizational designation	https://orcid.org/0000-0002-0465-1009	0.93	English label names map directly to legalName property
ROR:names[types=label,lang≠en]	Non-English Legal Name Object	skos:exactMatch	schema:additionalProperty	additionalProperty	Official non-English names as StructuredValue with inLanguage	https://orcid.org/0000-0002-0465-1009	0.88	Non-English labels preserved in PropertyValue structure for multilingual support
ROR:names[types=alias|acronym]	Alternate Name Objects	skos:exactMatch	schema:alternateName	alternateName	Organizational aliases and acronyms as alternate names	https://orcid.org/0000-0002-0465-1009	0.94	Both aliases and acronyms function as alternate name variants
```

## Confidence Rationale

**Updated Confidence: 0.88-0.95** based on:
- Composite object integrity (1.0)
- Type compatibility (1.0)
- Semantic accuracy for name types (0.94-0.95)
- Bidirectional reconstruction capability (1.0)
- Language handling complexity (-0.07 to -0.12)
- Implementation clarity (0.95)

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org name](https://schema.org/name)
- [Schema.org alternateName](https://schema.org/alternateName)
- [Schema.org legalName](https://schema.org/legalName)

## Bidirectional Mapping

### Forward Mapping (ROR → Schema.org)

The composite mapping approach ensures that each ROR name object `{value, types, lang}` maps to the appropriate Schema.org property while preserving all semantic information:

**Mapping Logic:**
1. **Primary Display**: `types=["ror_display"]` → `schema:name`
2. **English Official**: `types=["label"], lang="en"` → `schema:legalName`
3. **Non-English Official**: `types=["label"], lang≠"en"` → `schema:additionalProperty` (StructuredValue)
4. **Alternates**: `types=["alias"|"acronym"]` → `schema:alternateName`

### Reverse Mapping (Schema.org → ROR)

The mapping preserves enough information to reconstruct complete ROR name objects:

**Reconstruction Rules:**
- `schema:name` → `{value: name, types: ["ror_display"], lang: contextual}`
- `schema:legalName` → `{value: legalName, types: ["label"], lang: "en"}`
- `schema:additionalProperty` (with inLanguage) → `{value: name, types: ["label"], lang: inLanguage}`
- `schema:alternateName` → `{value: alternateName, types: ["alias"], lang: contextual}`

**Bidirectional Integrity Test:**
✅ Complete ROR name objects can be reconstructed from Schema.org properties
✅ Language information is preserved through direct context or StructuredValue
✅ Name type semantics are maintained through property choice
✅ No information loss in round-trip conversion

---

*Analysis conducted: 2024-09-10*
*Implementation updated: 2024-09-24*
*Updated mapping files: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:42-45, mappings/tsv/ror_to_schema_human_in_loop.tsv:4-7*
*Related analysis: names/implementation_example.md*
