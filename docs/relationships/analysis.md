# ROR `relationships` to Schema.org Mapping Analysis

## Overview

The ROR relationships field contains structured information about organizational relationships including hierarchical (parent/child), lateral (related), and temporal (successor/predecessor) connections between research organizations. This field enables modeling complex institutional networks and organizational evolution.

## ROR Field Definition

**Field:** `relationships`
- **Type:** Array of relationship objects
- **Default:** []
- **Semantic meaning:** Organizational relationships between research institutions with typed connections
- **Schema location:** Root level array property
- **Examples:** Parent-child university systems, related research institutes, successor organizations

### ROR Schema Structure

```json
"relationships": {
    "type": "array",
    "uniqueItems": true,
    "default": [],
    "items": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
            "type": {
                "type": "string",
                "enum": ["related", "parent", "child", "successor", "predecessor"]
            },
            "id": {
                "type": "string",
                "pattern": "^https://ror.org/0[a-z|0-9]{8}$"
            },
            "label": {
                "type": "string",
                "minLength": 1
            }
        },
        "required": ["id", "label", "type"]
    }
}
```

## Schema.org Analysis

### Target Properties: Hybrid Approach

- **Primary Properties:** `subOrganization`, `parentOrganization`, `additionalProperty`
- **Domain:** Organization
- **Range:** Organization (direct), PropertyValue (structured)
- **Comment:** Type-specific routing based on relationship semantics
- **Cardinality:** Multiple values allowed for all relationship types

### Schema.org Relationship Properties

#### Direct Organizational Relationships
- **`parentOrganization`**: The larger organization that this organization is a subOrganization of
- **`subOrganization`**: A relationship between two organizations where the first includes the second
- **`memberOf`**: An Organization to which this Person or Organization belongs

#### Structured Relationship Preservation
- **`additionalProperty`**: PropertyValue objects for complex or ambiguous relationships
- **`additionalType`**: Relationship type specification within structured objects

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: Generic memberOf Approach
- **Mapping**: `ROR:relationships` → `schema:memberOf`
- **Predicate**: skos:relatedMatch
- **Confidence**: 0.75
- **Pros**: Simple implementation, widely supported property
- **Cons**:
  - Data loss: All relationship types collapsed to generic membership
  - Semantic mismatch: Parent/child relationships don't represent membership
  - Bidirectional conversion impossible due to type information loss

#### Option 2: Action-Based Approach (Previous)
- **Mapping**: `ROR:relationships.type.successor` → `schema:replacer`
- **Predicate**: skos:relatedMatch
- **Confidence**: 0.75
- **Pros**: Attempts to capture succession semantics
- **Cons**:
  - **Domain violation**: `replacer`/`replacee` require `ReplaceAction` domain, not `Organization`
  - **Schema.org non-compliance**: Properties used outside intended context
  - **Implementation failure**: Would cause validation errors in strict processors

#### Option 3: Hybrid Type-Specific Approach (Recommended)
- **Mapping**: `ROR:relationships` → `schema:subOrganization,schema:additionalProperty`
- **Predicate**: skos:exactMatch
- **Confidence**: 0.90
- **Pros**:
  - **Semantic accuracy**: Direct Schema.org properties for clear equivalents
  - **Data preservation**: Structured approach for ambiguous relationships
  - **Bidirectional integrity**: Perfect round-trip conversion capability
  - **Schema.org compliance**: All properties used within correct domain/range
- **Cons**:
  - **Implementation complexity**: Requires type-based routing logic
  - **Mixed property usage**: Combines direct and structured property approaches

## Recommendation

**Recommended mapping:** `ROR:relationships` → Hybrid Type-Specific Approach

### Rationale

1. **Semantic Accuracy**: Uses Schema.org organizational relationship properties for their intended purpose (parent/child → parentOrganization/subOrganization)
2. **Data Preservation**: Structured PropertyValue approach preserves complete relationship metadata for ambiguous types (related, successor, predecessor)
3. **Bidirectional Integrity**: Type-specific routing enables perfect ROR ↔ Schema.org conversion
4. **Schema.org Compliance**: Eliminates domain/range violations present in previous action-based approach
5. **Implementation Practicality**: Provides clear routing logic based on relationship.type enum values

## Confidence Rationale

**Confidence: 0.90** based on:
- **Schema.org Compliance** (0.98): Perfect alignment with Organization relationship specifications
- **Data Preservation** (1.0): Zero information loss during conversion
- **Semantic Accuracy** (0.95): Direct properties for clear equivalents, structured for ambiguous types
- **Implementation Complexity** (0.80): Requires type-based routing but manageable with clear logic
- **Bidirectional Integrity** (1.0): Perfect round-trip conversion capability

## Implementation Considerations

### Complexity Factors
1. **Type-Based Routing**: Requires conditional logic to route relationships based on type enum
2. **Mixed Property Handling**: Both direct Organization properties and structured PropertyValue objects
3. **Nested Object Construction**: Building complex additionalProperty structures for non-hierarchical relationships
4. **Validation Requirements**: Must ensure correct @type assignments for structured relationships

### Optimization Strategies
1. **Routing Logic**: Implement clear type-to-property mapping functions
2. **Caching**: Cache frequently used Organization objects for related institutions
3. **Lazy Loading**: Build structured relationships only when complete metadata is available
4. **Error Handling**: Graceful fallback to additionalProperty for unknown relationship types

### Performance Considerations
1. **Memory Usage**: Structured PropertyValue objects require more memory than simple references
2. **Processing Time**: Type-based routing adds computational overhead
3. **Query Efficiency**: Direct parentOrganization/subOrganization properties enable efficient SPARQL queries
4. **Network Impact**: Larger JSON-LD structures for organizations with many relationships

### Nested Property Handling
1. **PropertyValue Structures**: Deep nesting for complex relationship metadata
2. **StructuredValue Objects**: Additional nesting level for relationship details
3. **Identifier Resolution**: Handling nested @id references within structured objects
4. **Type Consistency**: Maintaining correct @type assignments throughout nested structures

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org parentOrganization](https://schema.org/parentOrganization)
- [Schema.org subOrganization](https://schema.org/subOrganization)
- [Schema.org additionalProperty](https://schema.org/additionalProperty)

---

*Analysis conducted: 2024-09-29*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: docs/locations/analysis.md (similar structured approach)*