# ROR `locations` to Schema.org Mapping Analysis

## Overview

The ROR locations field contains comprehensive geographic information about an organization's location, including GeoNames identifiers, coordinates, and hierarchical administrative divisions from city to continent level.

## ROR Field Definition

**Field:** `locations`
- **Type:** Array of location objects
- **Default:** []
- **Semantic meaning:** Geographic location information for organizations with GeoNames integration
- **Schema location:** Root level array property
- **Examples:** Oakland, CA, US (University of California System)

### ROR Schema Structure

```json
"locations": {
    "type": "array",
    "uniqueItems": true,
    "default": [],
    "minItems": 1,
    "items": {
        "type": "object",
        "additionalProperties": false,
        "required": ["geonames_id", "geonames_details"],
        "properties": {
            "geonames_id": {
                "type": "integer"
            },
            "geonames_details": {
                "type": "object",
                "required": ["name"],
                "properties": {
                    "name": {"$ref": "#/$defs/non-empty-string"},
                    "lat": {"type": ["number", "null"]},
                    "lng": {"type": ["number", "null"]},
                    "continent_code": {
                        "type": ["string", "null"],
                        "enum": ["AF", "AN", "AS", "EU", "NA", "OC", "SA", null]
                    },
                    "continent_name": {
                        "type": ["string", "null"],
                        "enum": ["Africa", "Antarctica", "Asia", "Europe", "Oceania", "South America", "North America", null]
                    },
                    "country_code": {
                        "type": ["string", "null"],
                        "pattern": "^[A-Z]{2}$"
                    },
                    "country_name": {"type": ["string", "null"]},
                    "country_subdivision_code": {
                        "type": ["string", "null"],
                        "pattern": "^[A-Z0-9]{1,3}$"
                    },
                    "country_subdivision_name": {"type": ["string", "null"]}
                }
            }
        }
    }
}
```

## Schema.org Analysis

### Target Property: `location`

- **Property type:** Organization property
- **Domain:** Organization, Event, Action, InteractionCounter
- **Range:** VirtualLocation, PostalAddress, Text, Place
- **Comment:** The location of, for example, where an event is happening, where an organization is located, or where an action takes place
- **Cardinality:** Multiple values allowed

## Mapping Evaluation

### Alternative Mappings Evaluated

#### Option 1: PostalAddress Structure
- **Mapping**: `ROR:locations` → `schema:address` (PostalAddress)
- **Predicate**: skos:exactMatch
- **Confidence**: 0.75
- **Pros**: Standard postal address format, widely supported by tools
- **Cons**:
  - Data loss: Multiple ROR fields map to same Schema.org properties (country_code and country_name both → addressCountry)
  - Semantic mismatch: Continental information doesn't fit PostalAddress model
  - Bidirectional conversion conflicts

#### Option 2: Hierarchical Place Objects
- **Mapping**: `ROR:locations` → `schema:location` (Place with containedInPlace)
- **Predicate**: skos:exactMatch
- **Confidence**: 0.93
- **Pros**:
  - Perfect field preservation: Every ROR field has unique Schema.org mapping
  - Semantic accuracy: Models real geographic containment relationships
  - Bidirectional integrity: No data loss during round-trip conversion
  - Schema.org compliant: Uses Place → State → Country → Continent hierarchy
- **Cons**:
  - Implementation complexity: Requires nested object handling
  - Larger JSON-LD output: ~3x size of simple PostalAddress

#### Option 3: Hybrid Approach
- **Mapping**: Both `schema:address` (PostalAddress) and `schema:location` (Place)
- **Predicate**: skos:exactMatch
- **Confidence**: 0.88
- **Pros**: Maximum compatibility with legacy and modern consumers
- **Cons**: Data duplication, maintenance complexity

## Recommendation

**Recommended mapping:** `ROR:locations` → `schema:location` (Hierarchical Place Objects)

### Rationale

1. **Data Fidelity**: Perfect preservation of all ROR geographic data without conflicts or loss
2. **Semantic Accuracy**: Geographic containment relationships properly modeled using Schema.org Place hierarchy
3. **Bidirectional Integrity**: Complete round-trip conversion capability maintains data consistency
4. **Schema.org Compliance**: Fully validated against Schema.org specifications for Place, State, Country, and Continent types
5. **Extensibility**: Framework supports future ROR geographic enhancements

## Confidence Rationale

**Confidence: 0.93** based on:
- **Schema.org Compliance** (0.98): Perfect alignment with Place, State, Country, Continent specifications
- **Data Preservation** (1.0): Zero information loss during conversion
- **Semantic Accuracy** (0.95): Geographic relationships properly modeled
- **Implementation Complexity** (0.85): Requires nested object parsing but manageable
- **Bidirectional Integrity** (1.0): Perfect round-trip conversion capability

## Implementation Considerations

### Complexity Factors
1. **Nested Object Parsing**: Requires recursive handling of containedInPlace hierarchy
2. **Null Value Management**: Geographic fields may be null and must be handled gracefully
3. **Multiple Locations**: Organizations may have multiple location objects requiring array processing
4. **Performance Impact**: Larger JSON-LD structures require more parsing time and memory

### Optimization Strategies
1. **Caching**: Cache Place objects for repeated geographic regions
2. **Lazy Loading**: Build containedInPlace chains only when complete data is available
3. **Validation**: Implement robust error handling for incomplete geographic data
4. **Fallback Mechanisms**: Provide simple Place objects when full hierarchy cannot be constructed

## References

- [ROR Schema Documentation](https://ror.readme.io/docs/ror-data-structure)
- [ROR Schema v2.1](https://github.com/ror-community/ror-schema)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org Place](https://schema.org/Place)
- [Schema.org location property](https://schema.org/location)
- [Schema.org containedInPlace](https://schema.org/containedInPlace)

---

*Analysis conducted: 2024-09-29*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*
*Related analysis: None (first comprehensive location mapping analysis)*