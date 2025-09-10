# ROR Status to Schema.org Mapping Analysis

## Problem Statement

The ROR schema v2.1 `status` field (line 26 in human-in-loop mapping) required a better semantic mapping to Schema.org properties due to type incompatibility and semantic misalignment with the original mapping.

## Original Mapping Issues

**Original mapping:**
```tsv
ROR:status → schema:status (skos:relatedMatch, confidence: 0.60)
```

**Critical issues identified:**

1. **Non-existent property**: `schema:Thing.status` does not exist in Schema.org vocabulary
2. **Low confidence**: 0.60 confidence reflected the poor semantic alignment
3. **Type mismatch concern**: Even if it existed, status enum values would need appropriate Schema.org equivalents

## ROR Status Field Analysis

**Source:** `/json/ror/ror_schema_v2_1.json:327-335`

```json
"status": {
    "type": "string",
    "default": "active",
    "enum": [
        "active",
        "inactive",
        "withdrawn"
    ]
}
```

**Semantic meaning:**
- `active`: Organization is currently operational in the ROR registry
- `inactive`: Organization is no longer active but record preserved
- `withdrawn`: Organization record has been withdrawn from registry

## Schema.org Analysis

### Detailed Alternative Evaluation:

#### 1. **dissolutionDate** ❌
- **Property type**: Date
- **ROR type**: enum string (active, inactive, withdrawn)
- **Issues**:
  - Fundamental type mismatch - cannot map enum to date
  - Only semantically relevant for inactive/withdrawn states
  - Active organizations have no dissolution concept
- **Confidence if used**: 0.75 (for inactive/withdrawn only)
- **Original consideration**: Seemed logical for inactive organizations but fails on type compatibility


#### 2. **schema:status** ❌
- **Property type**: Text/Enumeration
- **Domain**: MedicalStudy, MedicalCondition, MedicalProcedure
- **Range**: EventStatusType, MedicalStudyStatus, Text
- **Issues**:
  - Domain-specific, not applicable to Organization type
  - Would violate Schema.org type constraints
- **Confidence if used**: 0.40
- **Why evaluated**: Found during Schema.org property search

#### 3. **additionalType** 🔄
- **Property type**: URL/Text
- **Range**: Class or URL from external vocabulary
- **Approach**: Map ROR status as additional type classifications
  - `ror:ActiveOrganization`
  - `ror:InactiveOrganization`
  - `ror:WithdrawnOrganization`
- **Pros**:
  - Creates semantic types for status
  - Standard Schema.org pattern for external vocabularies
- **Cons**:
  - Requires creating custom ROR vocabulary URIs
  - Loses direct enum value representation
  - More complex implementation
- **Confidence**: 0.70
- **Implementation example**:
  ```json
  {
    "@type": ["Organization"],
    "additionalType": "https://ror.org/vocab/ActiveOrganization"
  }
  ```

#### 4. **disambiguatingDescription** 🔄
- **Property type**: Text
- **Range**: Text
- **Approach**: Use status as descriptive disambiguation
- **Pros**:
  - Simple text representation
  - Human-readable
- **Cons**:
  - Loses structured/enumerated nature
  - More descriptive than semantic
  - No standardization of values
- **Confidence**: 0.65
- **Implementation example**:
  ```json
  {
    "@type": "Organization",
    "disambiguatingDescription": "Registry status: active"
  }
  ```

#### 5. **nonprofitStatus** ❌
- **Property type**: NonprofitType enumeration
- **Range**: Specific to nonprofit classification
- **Issues**:
  - Domain-specific to nonprofit organizations
  - Not applicable to all ROR organization types
  - Different semantic meaning than registry status
- **Confidence if used**: 0.30

#### 6. **additionalProperty** ✅ (Chosen solution)
- **Property type**: PropertyValue
- **Range**: PropertyValue (structured name-value pairs)
- **Approach**: Create structured property with name='registryStatus'
- **Pros**:
  - Type-safe: PropertyValue handles structured data
  - Semantic preservation: Maintains exact ROR enum values
  - Standard pattern: Established Schema.org convention for custom metadata
  - Extensible: Can accommodate future ROR status values
  - Flexible: Can include additional context if needed
- **Cons**:
  - Slightly more complex JSON-LD structure
  - Custom property name not standardized
- **Confidence**: 0.80
- **Implementation example**:
  ```json
  {
    "@type": "Organization",
    "additionalProperty": {
      "@type": "PropertyValue",
      "name": "registryStatus",
      "value": "active",
      "description": "ROR registry operational status"
    }
  }
  ```

## Recommended Solution

**Updated mapping:**
```tsv
ROR:status → schema:additionalProperty (skos:relatedMatch, confidence: 0.80)
```

**Justification:** "Related mapping - status as structured property with name-value pair"

**Implementation details:** "PropertyValue with name='registryStatus' and value from ROR enum (active, inactive, withdrawn)"

## Benefits of additionalProperty Approach

1. **Type safety**: PropertyValue structure handles enum values correctly
2. **Semantic preservation**: Maintains exact ROR status semantics
3. **Standard pattern**: Follows Schema.org conventions for custom metadata
4. **Higher confidence**: Improved from 0.60 to 0.80 due to better semantic fit
5. **Extensibility**: Future ROR status values can be accommodated

## JSON-LD Implementation Example

```json
{
  "@type": "Organization",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registryStatus",
    "value": "active"
  }
}
```

## Conclusion

The `additionalProperty` mapping provides a semantically appropriate, type-safe solution that preserves ROR's registry status information while conforming to Schema.org patterns. This approach significantly improves the mapping quality and confidence level.

---
*Analysis conducted: 2024-09-10*
*Updated mapping file: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:26*
