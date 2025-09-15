# ROR `names` to Schema.org Mapping Analysis

## Overview

Analysis of mapping ROR's `names` property to Schema.org properties, addressing the overly complex ItemList structure and establishing the optimal content-primary mapping approach.

## Current Mapping (Lines 22-25)

```tsv
ROR:names → schema:ItemList (skos:exactMatch, confidence: 0.85)
ROR:names.value → schema:ItemList.itemListElement.name (skos:exactMatch, confidence: 0.95)
ROR:names.lang → schema:ItemList.itemListElement.inLanguage (skos:relatedMatch, confidence: 0.75)
ROR:names.types → schema:ItemList.itemListElement.additionalProperty (skos:relatedMatch, confidence: 0.80)
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
- Preserves all ROR name structure and metadata
- Maintains type and language information

### ❌ Current Mapping Issues
- **Type mismatch**: ROR names are structured name objects, not simple list items
- **Semantic misalignment**: Organizations don't have "lists of names" - they have primary names with variants
- **Overengineering**: ItemList introduces unnecessary complexity for name variants
- **Deep nesting**: Creates complex JSON-LD structure: `ItemList → itemListElement → name`
- **Pattern violation**: Not how Schema.org typically handles organizational naming

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

#### Option 2: Current ItemList Approach
- **Mapping**: `ROR:names` → `schema:ItemList`
- **Predicate**: `skos:exactMatch`
- **Confidence**: 0.85
- **Pros**:
  - Preserves all structure and metadata
  - Maintains type and language information
- **Cons**:
  - Overengineered for simple name variants
  - Complex nested JSON-LD structure
  - Not typical Schema.org organizational pattern

#### Option 3: Language Handling Options
- **names.lang handling**:
  - `additionalProperty` with StructuredValue (0.80 confidence)
  - HTML lang attributes in microdata context (0.85 confidence)
  - JSON-LD language objects (0.75 confidence)

## Recommendation

**Recommended mapping:** Content-Primary Mapping with structured language handling

### Rationale

1. **Semantic Correctness**: Content drives mappings, not presentation choices
2. **Type Compatibility**: All mappings result in Text type properties
3. **Language Preservation**: Multilingual data handled through additionalProperty
4. **Implementation Flexibility**: Handles complex display scenarios
5. **Schema.org Alignment**: Follows established organizational naming patterns

### Updated Mapping

```tsv
ROR:names.value	Name Value String	skos:exactMatch	schema:name	name	Semantic equivalence - contains the actual name text content	https://orcid.org/0000-0002-0465-1009	0.95	All name values are the primary text strings for organizational names
ROR:names.lang	Name Language	skos:relatedMatch	schema:additionalProperty	additionalProperty	Related mapping - language metadata as structured property	https://orcid.org/0000-0002-0465-1009	0.80	Language tags preserved in StructuredValue with inLanguage
ROR:names[types=label]	Label Name	skos:exactMatch	schema:legalName	legalName	Semantic equivalence for official organizational label	https://orcid.org/0000-0002-0465-1009	0.90	label typically represents official/legal name designation
ROR:names[types=alias]	Alias Name	skos:exactMatch	schema:alternateName	alternateName	Direct correspondence for organizational aliases	https://orcid.org/0000-0002-0465-1009	0.95	alias directly maps to alternate name concept
ROR:names[types=acronym]	Acronym Name	skos:exactMatch	schema:alternateName	alternateName	Acronyms are alternate name forms	https://orcid.org/0000-0002-0465-1009	0.92	acronyms function as alternate names in practice
ROR:names.ror_display	Primary Display Name	skos:relatedMatch	schema:name	name	Related mapping - canonical display name when different from primary value	https://orcid.org/0000-0002-0465-1009	0.88	ror_display is schema:name when preferred display form; schema:alternateName otherwise
```

## Confidence Rationale

**Confidence: 0.80-0.95** based on:
- Content precedence approach (1.0)
- Type compatibility (1.0)
- Semantic accuracy for name types (0.9-0.95)
- Language handling complexity (-0.15 to -0.2)
- Conditional logic complexity (-0.05 to -0.12)

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org name](https://schema.org/name)
- [Schema.org alternateName](https://schema.org/alternateName)
- [Schema.org legalName](https://schema.org/legalName)

---

*Analysis conducted: 2024-09-10*
*Updated mapping file: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:22-27*
*Related analysis: names/implementation_example.md*
