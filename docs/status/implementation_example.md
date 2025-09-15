# ROR `status` to Schema.org Implementation Example

## Overview

Implementation example demonstrating the additionalProperty mapping approach for ROR registry status, showing how to preserve enum values in Schema.org compliant format.

**Mapping Approach**: Registry status as structured PropertyValue
**Target Institution**: Example organizations with different status values
**Focus**: Demonstrates enum value preservation and PropertyValue structure

---

## Source ROR Record

Based on ROR schema v2.1 structure:

```json
{
  "id": "https://ror.org/05a28rw58",
  "status": "active",
  "names": [
    {
      "value": "ETH Zurich",
      "types": ["ror_display"],
      "lang": "en"
    }
  ],
  "types": ["education"],
  "established": 1855
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Using additionalProperty with PropertyValue:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registryStatus",
    "value": "active"
  }
}
```

### Enhanced Implementation

With optional context:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/05a28rw58",
  "identifier": "https://ror.org/05a28rw58",
  "name": "ETH Zurich",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registryStatus",
    "value": "active"
  }
}
```

---

## Mapping Logic Breakdown

### 1. Status Value Preservation

**Applied Rule**: `ROR:status` → `schema:additionalProperty`

```
Source: "status": "active"
Target: "additionalProperty": { "name": "registryStatus", "value": "active" }
Confidence: 0.80
```

**Rationale**: PropertyValue structure handles enum values correctly while maintaining exact ROR semantics.

### 2. Enum Value Handling

**All Status Values**:
- `"active"` → PropertyValue with value: "active"
- `"inactive"` → PropertyValue with value: "inactive"
- `"withdrawn"` → PropertyValue with value: "withdrawn"

### 3. Structured Property Benefits

**Type Safety**: PropertyValue accepts string enum values
**Extensibility**: Can accommodate future ROR status values
**Semantic Preservation**: Maintains exact meaning without transformation

---

## Alternative Implementation Approaches

### Approach 1: Simple String Property

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "status",
    "value": "active"
  }
}
```

**Benefits**: More generic property name
**Trade-offs**: Less specific about registry context

### Approach 2: With Additional Context

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registryStatus",
    "value": "active"
  }
}
```

**Benefits**: Full context and traceability
**Trade-offs**: More complex structure

---

## Implementation Benefits

### Type Safety
- PropertyValue handles enum string values correctly
- No type transformation required
- Preserves original ROR data types

### Semantic Preservation
- Maintains exact ROR status semantics
- Registry-specific context preserved
- Future enum values automatically supported

### Schema.org Compliance
- Uses established PropertyValue pattern
- Follows custom metadata conventions
- Compatible with all Schema.org processors

---

## Implementation Notes

### Technical Considerations
- Use consistent property name "registryStatus" across all organizations
- PropertyValue structure is required (not simple string)
- Enum values should be preserved exactly as in ROR

### Best Practices
- Maintain consistency with other ROR metadata mappings

### Edge Cases
- Handle null status values (use ROR default: "active")
- Future enum values automatically supported
- No special handling needed for status transitions

---

## Validation Against Mapping Rules

This example demonstrates key aspects of the additionalProperty approach:

✅ **Type Safety**: PropertyValue correctly handles enum string values
✅ **Semantic Preservation**: Exact ROR status meaning maintained
✅ **Standard Pattern**: Uses established Schema.org convention for custom metadata
✅ **Extensibility**: Future ROR status values automatically accommodated
✅ **Schema Compliance**: Fully compatible with Schema.org specifications

---

*Example created: 2024-09-15*
*Based on mapping rules: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:26*
*Related analysis: status/analysis.md*