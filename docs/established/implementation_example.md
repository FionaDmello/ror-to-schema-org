# ROR `established` to Schema.org Implementation Example

## Overview

Practical implementation examples showing how to map ROR establishment year to Schema.org additionalProperty using PropertyValue structure.

**Mapping Approach**: Numeric year → additionalProperty with PropertyValue
**Target Institution**: MIT (established 1861)
**Focus**: Real JSON-LD output preserving numeric data types

---

## Source ROR Record

Real ROR data structure with established field:

```json
{
  "id": "https://ror.org/042nb2s44",
  "names": [
    {
      "value": "Massachusetts Institute of Technology",
      "types": ["ror_display"],
      "lang": "en"
    }
  ],
  "established": 1861,
  "types": ["education"]
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Numeric year preserved in PropertyValue:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/042nb2s44",
  "identifier": "https://ror.org/042nb2s44",
  "name": "Massachusetts Institute of Technology",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established",
    "value": 1861
  }
}
```

### Enhanced Implementation

With optional metadata:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/042nb2s44",
  "identifier": "https://ror.org/042nb2s44",
  "name": "Massachusetts Institute of Technology",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established",
    "value": 1861
  }
}
```

---

## Step-by-Step Implementation

### Step 1: Extract Establishment Year

```javascript
// Input: ROR established field
const rorEstablished = 1861;

// Verify it's a valid year
const isValidYear = typeof rorEstablished === 'number' &&
                   rorEstablished > 0 &&
                   rorEstablished <= new Date().getFullYear();
// Result: true
```

### Step 2: Create PropertyValue Structure

```javascript
const establishedProperty = {
  "@type": "PropertyValue",
  "name": "established",
  "value": rorEstablished
};
```

### Step 3: Add to Schema.org Object

```javascript
const schemaOrg = {
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": rorRecord.id,
  "identifier": rorRecord.id,
  "name": rorRecord.names[0].value,
  "additionalProperty": establishedProperty
};
```

---

## Real-World Examples

### Example 1: Recent Institution

**ROR Input:**
```json
{
  "established": 2003
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "New University",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established",
    "value": 2003
  }
}
```

### Example 2: Historic Institution

**ROR Input:**
```json
{
  "established": 1348
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "Charles University",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "established",
    "value": 1348
  }
}
```

### Example 3: Null/Missing Establishment Year

**ROR Input:**
```json
{
  "established": null
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "Research Institute"
}
```
*Note: No additionalProperty is added for null values*

---

## Implementation Code Examples

### Python Implementation

```python
def map_ror_established_to_schema(ror_established):
    """
    Map ROR established year to Schema.org additionalProperty
    """
    if ror_established is None:
        return None

    # Validate year
    current_year = datetime.now().year
    if not isinstance(ror_established, int) or ror_established <= 0 or ror_established > current_year:
        print(f"Warning: Invalid establishment year: {ror_established}")
        return None

    return {
        "@type": "PropertyValue",
        "name": "established",
        "value": ror_established
    }
```

### JavaScript Implementation

```javascript
function mapRorEstablishedToSchema(rorEstablished) {
  if (rorEstablished === null || rorEstablished === undefined) {
    return null;
  }

  // Validate year
  const currentYear = new Date().getFullYear();
  if (typeof rorEstablished !== 'number' ||
      rorEstablished <= 0 ||
      rorEstablished > currentYear) {
    console.warn(`Invalid establishment year: ${rorEstablished}`);
    return null;
  }

  return {
    "@type": "PropertyValue",
    "name": "established",
    "value": rorEstablished
  };
}
```

### Enhanced Implementation with Validation

```javascript
function validateAndMapEstablished(rorEstablished) {
  const result = {
    valid: false,
    property: null,
    warnings: []
  };

  // Handle null/undefined
  if (rorEstablished === null || rorEstablished === undefined) {
    result.warnings.push("Establishment year is null");
    return result;
  }

  // Type validation
  if (typeof rorEstablished !== 'number') {
    result.warnings.push(`Expected number, got ${typeof rorEstablished}`);
    return result;
  }

  // Range validation
  const currentYear = new Date().getFullYear();
  if (rorEstablished <= 0) {
    result.warnings.push("Establishment year must be positive");
    return result;
  }

  if (rorEstablished > currentYear) {
    result.warnings.push("Establishment year cannot be in the future");
    return result;
  }

  // Create property
  result.valid = true;
  result.property = {
    "@type": "PropertyValue",
    "name": "established",
    "value": rorEstablished
  };

  return result;
}
```

---

## Validation Examples

### Test Case 1: Valid Year

```javascript
const input = 1985;
const result = mapRorEstablishedToSchema(input);

console.log(result["@type"]); // "PropertyValue"
console.log(result.name); // "established"
console.log(result.value); // 1985
console.log(typeof result.value); // "number"
```

### Test Case 2: Null Value

```javascript
const input = null;
const result = mapRorEstablishedToSchema(input);

console.log(result); // null
```

### Test Case 3: Invalid Year

```javascript
const input = 2050; // Future year
const result = validateAndMapEstablished(input);

console.log(result.valid); // false
console.log(result.warnings); // ["Establishment year cannot be in the future"]
console.log(result.property); // null
```

### Test Case 4: Historic Institution

```javascript
const input = 1348;
const result = mapRorEstablishedToSchema(input);

console.log(result.value); // 1348
console.log(typeof result.value); // "number"
```

---

## Production Usage Example

```javascript
// Complete implementation for a ROR record
function processRorRecord(rorRecord) {
  const schemaOrg = {
    "@context": "https://schema.org",
    "@type": "EducationalOrganization",
    "@id": rorRecord.id,
    "identifier": rorRecord.id,
    "name": rorRecord.names[0].value
  };

  // Add establishment year if present
  if (rorRecord.established !== null && rorRecord.established !== undefined) {
    const establishedProperty = mapRorEstablishedToSchema(rorRecord.established);
    if (establishedProperty) {
      schemaOrg.additionalProperty = establishedProperty;
    }
  }

  return schemaOrg;
}

// Usage with real data
const rorRecord = {
  "id": "https://ror.org/042nb2s44",
  "names": [{"value": "Massachusetts Institute of Technology", "types": ["ror_display"]}],
  "established": 1861
};

const schemaOutput = processRorRecord(rorRecord);
console.log(JSON.stringify(schemaOutput, null, 2));

// Verify numeric type is preserved
console.log(typeof schemaOutput.additionalProperty.value); // "number"
console.log(schemaOutput.additionalProperty.value === 1861); // true
```

### Combining with Other Properties

```javascript
// When combining established with other additionalProperties
function combineAdditionalProperties(rorRecord) {
  let additionalProperties = [];

  // Add established year
  if (rorRecord.established) {
    const establishedProperty = mapRorEstablishedToSchema(rorRecord.established);
    if (establishedProperty) {
      additionalProperties.push(establishedProperty);
    }
  }

  // Add status
  if (rorRecord.status) {
    additionalProperties.push({
      "@type": "PropertyValue",
      "name": "registryStatus",
      "value": rorRecord.status
    });
  }

  return additionalProperties.length === 1
    ? additionalProperties[0]
    : additionalProperties;
}
```

### Date Conversion Alternative (Not Recommended)

```javascript
// Alternative approach converting to foundingDate (loses precision)
function convertToFoundingDate(rorEstablished) {
  if (!rorEstablished) return null;

  // This loses the precise numeric year value
  return `${rorEstablished}-01-01`; // Assumes January 1st
}

// The PropertyValue approach is preferred because:
// 1. Preserves exact numeric value (1861 vs "1861-01-01")
// 2. No assumptions about specific dates
// 3. Type-safe for computational use
// 4. Maintains data fidelity
```

---

*Example created: 2024-09-15*
*Based on mapping rules: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv*
*Related analysis: established/analysis.md*