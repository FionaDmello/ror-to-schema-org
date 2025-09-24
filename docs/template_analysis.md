# ROR `[FIELD_NAME]` to Schema.org Mapping Analysis

## Overview

[Brief 1-2 sentence description of the field and its role in the ROR schema]

## ROR Field Definition

**Field:** `[FIELD_NAME]`
- **Type:** [ROR_TYPE]
- **Default:** [DEFAULT_VALUE]
- **Semantic meaning:** [DESCRIPTION_OF_FIELD_PURPOSE]
- **Schema location:** [JSON_SCHEMA_REFERENCE]
- **Examples:** [EXAMPLE_VALUES]

### ROR Schema Structure

```json
"[FIELD_NAME]": {
    [JSON_SCHEMA_DEFINITION]
}
```

## Schema.org Analysis

### Target Property: `[SCHEMA_PROPERTY]`

- **Property type:** [SCHEMA_TYPE]
- **Domain:** [APPLICABLE_CLASSES]
- **Range:** [EXPECTED_TYPES]
- **Comment:** [SCHEMA_DESCRIPTION]
- **Cardinality:** [SINGLE_OR_MULTIPLE]

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: [ALTERNATIVE_PROPERTY_1]
- **Mapping**: `ROR:[FIELD_NAME]` → `schema:[PROPERTY_1]`
- **Predicate**: [SKOS_PREDICATE]
- **Confidence**: [0.0-1.0]
- **Pros**: [ADVANTAGES]
- **Cons**: [DISADVANTAGES]

#### Option 2: [ALTERNATIVE_PROPERTY_2]
- **Mapping**: `ROR:[FIELD_NAME]` → `schema:[PROPERTY_2]`
- **Predicate**: [SKOS_PREDICATE]
- **Confidence**: [0.0-1.0]
- **Pros**: [ADVANTAGES]
- **Cons**: [DISADVANTAGES]

[Additional options as needed]

## Recommendation

**Recommended mapping:** `ROR:[FIELD_NAME]` → `schema:[CHOSEN_PROPERTY]`

### Rationale

1. **[CRITERION_1]**: [EXPLANATION]
2. **[CRITERION_2]**: [EXPLANATION]
3. **[CRITERION_3]**: [EXPLANATION]

### Updated Mapping

```tsv
[UPDATED_MAPPING_LINE_FOR_SSSOM]
```

## Confidence Rationale

**Confidence: [0.0-1.0]** based on:
- [CONFIDENCE_FACTOR_1] ([SCORE])
- [CONFIDENCE_FACTOR_2] ([SCORE])
- [CONFIDENCE_FACTOR_3] ([SCORE])

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org [TARGET_CLASS]](https://schema.org/[TARGET_CLASS])
- [Schema.org [TARGET_PROPERTY]](https://schema.org/[TARGET_PROPERTY])

---

*Analysis conducted: [DATE]*
*Updated mapping file: [MAPPING_FILE_REFERENCE]*
*Related analysis: [RELATED_DOCS]*
