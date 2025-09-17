# ROR `admin` to Schema.org Mapping Analysis

## Overview

ROR's `admin` field contains comprehensive administrative metadata about the lifecycle of ROR records, including creation and modification timestamps with schema versioning for data provenance tracking.

## Current Mappings (Lines 35-41)

```tsv
ROR:admin	Administrative Information Object	skos:closeMatch	schema:CreativeWork	CreativeWork	Close mapping - ROR records are curated intellectual works with structured metadata	https://orcid.org/0000-0002-0465-1009	0.85	Administrative metadata for ROR record management as structured data products
ROR:admin.created	Creation Information Object	skos:exactMatch	schema:additionalProperty	additionalProperty	Structured creation metadata preserving date and schema version relationship	https://orcid.org/0000-0002-0465-1009	0.92	Creation metadata as PropertyValue with date and schema version context
ROR:admin.created.date	Creation Date	skos:exactMatch	schema:dateCreated	dateCreated	Semantic equivalence - specific creation date	https://orcid.org/0000-0002-0465-1009	0.95	ISO date format for record creation
ROR:admin.created.schema_version	Creation Schema Version	skos:relatedMatch	schema:version	version	Related mapping - schema version at creation	https://orcid.org/0000-0002-0465-1009	0.85	Schema version when record was created (1.0, 2.0, 2.1)
ROR:admin.last_modified	Last Modified Information Object	skos:exactMatch	schema:additionalProperty	additionalProperty	Structured modification metadata preserving date and schema version relationship	https://orcid.org/0000-0002-0465-1009	0.92	Modification metadata as PropertyValue with date and schema version context
ROR:admin.last_modified.date	Last Modified Date	skos:exactMatch	schema:dateModified	dateModified	Semantic equivalence - specific modification date	https://orcid.org/0000-0002-0465-1009	0.95	ISO date format for record modification
ROR:admin.last_modified.schema_version	Modification Schema Version	skos:relatedMatch	schema:version	version	Related mapping - schema version at modification	https://orcid.org/0000-0002-0465-1009	0.85	Schema version when record was last modified
```

## ROR Field Definition

**Field:** `admin`
- **Type:** Object (required at root level)
- **Default:** None (required field)
- **Semantic meaning:** Administrative metadata container for ROR record lifecycle management
- **Schema location:** `ror_schema_v2_1.json:7-59`
- **Examples:** Complete admin object with creation and modification metadata

### ROR Schema Structure

```json
"admin": {
    "type": "object",
    "additionalProperties": false,
    "properties": {
        "created": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "date": {
                    "type": "string",
                    "format": "date"
                },
                "schema_version": {
                    "type": "string",
                    "enum": ["1.0", "2.0", "2.1"]
                }
            },
            "required": ["date", "schema_version"]
        },
        "last_modified": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "date": {
                    "type": "string",
                    "format": "date"
                },
                "schema_version": {
                    "type": "string",
                    "enum": ["1.0", "2.0", "2.1"]
                }
            },
            "required": ["date", "schema_version"]
        }
    },
    "required": ["created", "last_modified"]
}
```

## Schema.org Analysis

### Container Mapping: `admin` → `CreativeWork`

- **Property type:** Class
- **Domain:** Thing
- **Range:** CreativeWork subclass
- **Comment:** The most generic kind of creative work
- **Cardinality:** Single class assignment

### Structured Mappings: `admin.created/last_modified` → `additionalProperty`

- **Property type:** ObjectProperty
- **Domain:** Thing (covers CreativeWork)
- **Range:** PropertyValue
- **Comment:** Additional characteristics represented as property-value pairs
- **Cardinality:** Multiple values supported

### Direct Mappings: Date Properties

- **`dateCreated`**: Date/DateTime range, single value
- **`dateModified`**: Date/DateTime range, single value
- **`version`**: Text/Number range, multiple values possible

## Mapping Evaluation

### ✅ Current Mapping Strengths

- **Hybrid approach**: Combines direct access (dates) with structured metadata (containers)
- **Type compatibility**: All mappings handle ROR data types correctly
- **Information preservation**: No data loss in Schema.org transformation
- **Bidirectional mapping**: Can reconstruct exact ROR structure
- **Schema versioning**: Preserves temporal context for different schema versions

### ❌ Previous Mapping Issues (Resolved)

- **Type mismatches**: Original container → atomic property mappings
- **Property conflicts**: Multiple schema versions mapping to single `version` property
- **Information loss**: Flattening structured objects lost contextual relationships
- **Redundancy concerns**: Initially seen as problematic, now justified by semantic requirements

### Critical Decision Points

#### Schema Version Collision Problem
**Issue**: `admin.created.schema_version` and `admin.last_modified.schema_version` can differ:
```json
{
  "admin": {
    "created": {"date": "2018-03-15", "schema_version": "1.0"},
    "last_modified": {"date": "2023-08-15", "schema_version": "2.1"}
  }
}
```

**Solution**: Parallel mapping strategy preserves both values with context.

#### Redundancy vs Accuracy Trade-off
**Decision**: Accept controlled redundancy to preserve semantic relationships rather than optimize for minimal representation.

## Alternative Mappings Evaluated

#### Option 1: Pure Leaf Mapping (Rejected)
- **Approach**: Map only leaf properties, ignore containers
- **Issues**: Schema version conflicts, information loss, reconstruction problems

#### Option 2: Pure Structural Mapping (Considered)
- **Approach**: Map everything to `additionalProperty` structures
- **Issues**: Verbose output, loss of direct access patterns

#### Option 3: Action-Based Mapping (Alternative)
- **Approach**: Use `CreateAction` and `UpdateAction`
- **Assessment**: Semantically valid but more complex than metadata approach

## Recommendation

**Recommended mapping strategy:** Hybrid approach with structured containers and direct leaf access

### Rationale

1. **Semantic Accuracy**: Each mapping targets the most appropriate Schema.org property for its content
2. **Type Safety**: No type conversion issues or data loss
3. **Query Flexibility**: Supports both simple date queries and complex metadata analysis
4. **Provenance Preservation**: Maintains essential temporal and versioning relationships
5. **Schema.org Compliance**: Follows established patterns for complex metadata

## Output Example

**ROR Input**:
```json
{
  "admin": {
    "created": {"date": "2018-03-15", "schema_version": "1.0"},
    "last_modified": {"date": "2023-08-15", "schema_version": "2.1"}
  }
}
```

**Schema.org Output**:
```json
{
  "@type": "CreativeWork",
  "dateCreated": "2018-03-15",
  "dateModified": "2023-08-15",
  "version": ["1.0", "2.1"],
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "propertyID": "admin.created",
      "value": {"date": "2018-03-15", "schema_version": "1.0"}
    },
    {
      "@type": "PropertyValue",
      "propertyID": "admin.last_modified",
      "value": {"date": "2023-08-15", "schema_version": "2.1"}
    }
  ]
}
```

## Confidence Rationale

**Overall confidence: 0.90** based on:

- **Container framework** (0.85): CreativeWork provides solid foundation
- **Structured metadata** (0.92): PropertyValue pattern perfectly handles complex objects
- **Direct date access** (0.95): Exact semantic matches for temporal properties
- **Schema versioning** (0.85): Related but not identical to content versioning
- **Bidirectional mapping** (0.95): Complete information preservation

## Key Design Principles

1. **Preserve Provenance**: Administrative metadata contains essential data stewardship information
2. **Handle Temporal Complexity**: Different schema versions for creation vs modification events
3. **Enable Multiple Access Patterns**: Both simple and complex queries supported
4. **Maintain Type Safety**: No lossy conversions or type mismatches
5. **Follow Standards**: Schema.org best practices for structured metadata

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org CreativeWork](https://schema.org/CreativeWork)
- [Schema.org additionalProperty](https://schema.org/additionalProperty)
- [Schema.org PropertyValue](https://schema.org/PropertyValue)
- [Schema.org dateCreated](https://schema.org/dateCreated)
- [Schema.org dateModified](https://schema.org/dateModified)

---

*Analysis conducted: 2025-09-17*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: Applies pattern for other ROR complex objects*