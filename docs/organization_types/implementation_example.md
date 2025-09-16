# ROR `types` to Schema.org Implementation Example

## Overview

Multi-type array mapping preserves all ROR organizational classifications in clean Schema.org JSON-LD.

**Mapping Approach**: Direct type array mapping
**Target Institution**: University of California System
**Focus**: Clean JSON-LD @type arrays

---

## Source ROR Record

Actual UC System record:

```json
{
  "id": "https://ror.org/00pjdza24",
  "types": ["education"],
  "names": [
    {
      "value": "University of California System",
      "types": ["ror_display", "label"],
      "lang": "en"
    }
  ]
}
```

---

## Schema.org Mapping Output

### Single Type

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24",
  "identifier": "https://ror.org/00pjdza24",
  "name": "University of California System"
}
```

### Multi-Type Example

For `types: ["education", "healthcare"]`:

```json
{
  "@context": "https://schema.org",
  "@type": ["EducationalOrganization", "MedicalOrganization"],
  "@id": "https://ror.org/example",
  "identifier": "https://ror.org/example",
  "name": "University Medical Center"
}
```

---

## Mapping Logic Breakdown

### 1. Type Mapping

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
```

### 2. Array Handling

```javascript
function mapRorTypes(rorTypes) {
  const schemaTypes = rorTypes.map(t => TYPE_MAP[t] || "Organization");

  // Remove duplicates
  const unique = [...new Set(schemaTypes)];

  // Return string for single type, array for multiple
  return unique.length === 1 ? unique[0] : unique;
}
```

---

## Implementation Examples

### JavaScript Implementation

```javascript
function buildSchemaOrg(rorRecord) {
  return {
    "@context": "https://schema.org",
    "@type": mapRorTypes(rorRecord.types || []),
    "@id": rorRecord.id,
    "identifier": rorRecord.id,
    "name": rorRecord.names.find(n =>
      n.types.includes("ror_display"))?.value
  };
}
```

### Python Implementation

```python
def build_schema_org(ror_record):
    return {
        "@context": "https://schema.org",
        "@type": map_ror_types(ror_record.get("types", [])),
        "@id": ror_record["id"],
        "identifier": ror_record["id"],
        "name": next(n["value"] for n in ror_record["names"]
                    if "ror_display" in n["types"])
    }
```

---

## Test Cases

### Single Type
```javascript
mapRorTypes(["education"])
// → "EducationalOrganization"
```

### Multi-Type
```javascript
mapRorTypes(["education", "healthcare"])
// → ["EducationalOrganization", "MedicalOrganization"]
```

### Fallback
```javascript
mapRorTypes(["facility"])
// → "Organization"
```

---

*Example created: 2024-09-16*
*Based on: UC System record + ROR schema v2.1*
*Related: organization_types/ror_organization_types_analysis.md*