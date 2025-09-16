# ROR Organization Types to Schema.org Mapping Analysis

## Overview

ROR `types` field uses 9 enum values to classify organizations. The current mapping incorrectly assigns all ROR records to `schema:ResearchOrganization`, ignoring this explicit taxonomy.

## Current Mapping (Line 31)

```tsv
ROR:*	ROR Record	skos:narrowMatch	schema:ResearchOrganization	ResearchOrganization	Narrow mapping - ROR focuses on research organizations	https://orcid.org/0000-0002-0465-1009	0.88	All ROR records represent research-related organizations
```

## ROR Field Definition

**Field:** `types`
- **Type:** Array of enum strings (9 values)
- **Schema location:** `ror_schema_v2_1.json:336-355`
- **Actual example:** `["education"]` (University of California System)

### ROR Types (from schema)

```
"education", "funder", "healthcare", "company",
"archive", "nonprofit", "government", "facility", "other"
```

### Multi-Type Support

- Array allows multiple types: `["education", "healthcare"]`
- `uniqueItems: true` prevents duplicates
- No mutual exclusivity constraints

## Schema.org Analysis

### Direct Type Mappings

- `education` → `EducationalOrganization`
- `healthcare` → `MedicalOrganization`
- `government` → `GovernmentOrganization`
- `funder` → `FundingAgency`
- `nonprofit` → `NGO`
- `archive` → `ArchiveOrganization`
- `company` → `Corporation`
- `facility` → `Organization` (conservative)
- `other` → `Organization` (conservative)

### Multi-Type Handling

Schema.org `@type` accepts arrays: `["EducationalOrganization", "MedicalOrganization"]`

## Mapping Evaluation

### ❌ Current Mapping Issues

- UC System (`types: ["education"]`) incorrectly mapped as ResearchOrganization
- Ignores ROR's 9-type taxonomy
- No multi-type handling
- 0.88 confidence inappropriate

### ✅ Recommended Approach

**Multi-type array mapping**: `ROR:types` → `@type` array

- `["education"]` → `"EducationalOrganization"`
- `["education","healthcare"]` → `["EducationalOrganization","MedicalOrganization"]`
- Confidence: 0.95 (direct mappings), 0.70 (facility/other)

## Implementation

### Core Mapping Logic

```javascript
const TYPE_MAP = {
  "education": "EducationalOrganization",
  "healthcare": "MedicalOrganization",
  "government": "GovernmentOrganization",
  "funder": "FundingAgency",
  "nonprofit": "NGO",
  "archive": "ArchiveOrganization",
  "company": "Corporation",
  "facility": "Organization",
  "other": "Organization"
};

// Map array to Schema.org types
function mapTypes(rorTypes) {
  const schemaTypes = rorTypes.map(t => TYPE_MAP[t] || "Organization");
  return schemaTypes.length === 1 ? schemaTypes[0] : schemaTypes;
}
```

### Example Output

UC System (`types: ["education"]`):
```json
{
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "name": "University of California System"
}
```

## References

- [ROR Schema v2.1 Line 336-355](../context/ror/ror_schema_v2_1.json)
- [ROR Example Record v2.1](../context/ror_examples/example_record_v2_1.json)
- [Schema.org Organization](https://schema.org/Organization)
- [Schema.org EducationalOrganization](https://schema.org/EducationalOrganization)

---

*Analysis conducted: 2024-09-16*
*Based on: ROR Schema v2.1, UC System example record*
*Updated mapping file: mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv*