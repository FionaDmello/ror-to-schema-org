# ROR `admin` to Schema.org Implementation Example

## Overview

This example demonstrates mapping ROR administrative metadata to Schema.org using PropertyValue containing array of StructuredValues for perfect structural preservation.

**Mapping Approach**: Array-based StructuredValue approach
**Target Institution**: University of California System (ROR ID: https://ror.org/00pjdza24)
**Focus**: Administrative metadata with creation/modification tracking and schema versioning

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
  "types": ["education"],
  "admin": {
    "created": {
      "date": "2020-04-25",
      "schema_version": "1.0"
    },
    "last_modified": {
      "date": "2022-10-18",
      "schema_version": "2.0"
    }
  }
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Using core Schema.org properties with StructuredValue containers:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "identifier": "https://ror.org/00pjdza24",
  "name": "University of California System",
  "dateCreated": "2020-04-25",
  "dateModified": "2022-10-18",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "recordAdministration",
      "value": [
        {
          "@type": "StructuredValue",
          "name": "created",
          "dateCreated": "2020-04-25",
          "version": "1.0"
        },
        {
          "@type": "StructuredValue",
          "name": "last_modified",
          "dateModified": "2022-10-18",
          "version": "2.0"
        }
      ]
    }
  ]
}
```

---

## Mapping Logic Breakdown

### 1. Admin Container Mapping

**Applied Rule**: `ROR:admin` → `schema:additionalProperty`

```
Source: {"created": {...}, "last_modified": {...}}
Target: PropertyValue with array of StructuredValues
Confidence: 0.90
```

**Rationale**: Administrative metadata fits naturally as additional organizational properties using PropertyValue container pattern.

### 2. Created/Modified Object Mapping

**Applied Rule**: `ROR:admin.created` → `schema:StructuredValue`

```
Source: {"date": "2020-04-25", "schema_version": "1.0"}
Target: StructuredValue with dateCreated and version properties
Confidence: 0.92
```

**Rationale**: Each admin sub-object becomes its own StructuredValue preserving the hierarchical relationship.

### 3. Date Properties Mapping

**Applied Rule**: `ROR:admin.created.date` → `schema:dateCreated`

```
Source: "2020-04-25"
Target: "2020-04-25" (within StructuredValue)
Confidence: 0.95
```

**Rationale**: Direct semantic equivalence for ISO date format within structured containers.

### 4. Schema Version Mapping

**Applied Rule**: `ROR:admin.created.schema_version` → `schema:version`

```
Source: "1.0"
Target: "1.0" (within StructuredValue)
Confidence: 0.95
```

**Rationale**: Direct version mapping within StructuredValue avoids conflicts between creation and modification versions.

---

## Implementation Benefits

### Structural Preservation
- Perfect round-trip conversion capability
- Maintains ROR admin object hierarchy exactly
- No information loss during transformation

### Semantic Accuracy
- Direct mapping of dates to appropriate Schema.org properties
- Proper version representation within structured contexts
- Clean separation of creation vs modification metadata

### Schema.org Compliance
- Uses PropertyValue.value with StructuredValue array as specified
- Follows established patterns for complex metadata
- Compatible with Schema.org validation tools

---

## Implementation Notes

### Technical Considerations
- PropertyValue.value explicitly supports StructuredValue arrays
- Each StructuredValue can contain semantic Schema.org properties
- Array structure prevents version conflicts between created/modified

### Best Practices
- Always preserve both top-level dates and structured admin metadata
- Use descriptive "name" properties for StructuredValue identification
- Maintain consistent property naming across implementations

### Edge Cases
- Different schema versions for creation vs modification are properly handled
- Missing admin data can be handled by omitting the PropertyValue
- Future ROR schema versions can be added without breaking the structure

---

## Validation Against Mapping Rules

This example demonstrates key aspects of the Array-based StructuredValue Approach:

✅ **Structural Integrity**: ROR admin hierarchy → PropertyValue with StructuredValue array preserves exact relationships
✅ **Semantic Mapping**: dateCreated/dateModified/version map directly to appropriate Schema.org properties within structured containers
✅ **Round-trip Fidelity**: Easy reconstruction of original ROR admin object from Schema.org representation

---

*Example created: 2025-01-24*
*Based on mapping rules: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv lines 35-41*
*Related analysis: docs/admin/analysis.md*