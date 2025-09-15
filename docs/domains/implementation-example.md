# ROR `domains` to Schema.org Implementation Example

## Overview

Practical implementation examples showing how to map ROR domain arrays to Schema.org additionalProperty where each domain becomes its own PropertyValue.

**Mapping Approach**: Each domain → separate PropertyValue in additionalProperty array
**Target Institution**: University of Pisa (multiple domains example)
**Focus**: Real JSON-LD output with individual PropertyValues

---

## Source ROR Record

Real ROR data structure with domains:

```json
{
  "id": "https://ror.org/02pq29p90",
  "names": [
    {
      "value": "University of Pisa",
      "types": ["ror_display"],
      "lang": "en"
    }
  ],
  "domains": [
    "unipi.it",
    "di.unipi.it",
    "iet.unipi.it"
  ],
  "types": ["education"]
}
```

---

## Schema.org Mapping Output

### Basic Implementation

Each domain as separate PropertyValue:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/02pq29p90",
  "identifier": "https://ror.org/02pq29p90",
  "name": "University of Pisa",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "unipi.it"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "di.unipi.it"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "iet.unipi.it"
    }
  ]
}
```

### Enhanced Implementation

With optional property naming:

```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/02pq29p90",
  "identifier": "https://ror.org/02pq29p90",
  "name": "University of Pisa",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "unipi.it"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "di.unipi.it"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "iet.unipi.it"
    }
  ]
}
```

---

## Step-by-Step Implementation

### Step 1: Map Each Domain to PropertyValue

```javascript
// Input: ROR domains array
const rorDomains = ["unipi.it", "di.unipi.it", "iet.unipi.it"];

// Create PropertyValue for each domain
const domainProperties = rorDomains.map(domain => ({
  "@type": "PropertyValue",
  "name": "registeredDomain",
  "value": domain
}));

// Result: Array of 3 PropertyValue objects
```

### Step 2: Add to Schema.org Object

```javascript
const schemaOrg = {
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": rorRecord.id,
  "identifier": rorRecord.id,
  "name": rorRecord.names[0].value,
  "additionalProperty": domainProperties
};
```


---

## Real-World Examples

### Example 1: Single Domain Organization

**ROR Input:**
```json
{
  "domains": ["stanford.edu"]
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "Stanford University",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "stanford.edu"
    }
  ]
}
```

### Example 2: Multiple Domains Organization

**ROR Input:**
```json
{
  "domains": [
    "universityofcalifornia.edu",
    "berkeley.edu",
    "ucla.edu",
    "ucsf.edu"
  ]
}
```

**Schema.org Output:**
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "name": "University of California",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "universityofcalifornia.edu"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "berkeley.edu"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "ucla.edu"
    },
    {
      "@type": "PropertyValue",
      "name": "registeredDomain",
      "value": "ucsf.edu"
    }
  ]
}
```

### Example 3: Empty Domains Array

**ROR Input:**
```json
{
  "domains": []
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
*Note: No additionalProperty is added for empty domains array*

---

## Implementation Code Examples

### Python Implementation

```python
def map_ror_domains_to_schema(ror_domains):
    """
    Map ROR domains array to additionalProperty with individual PropertyValues
    """
    if not ror_domains:
        return []

    domain_properties = []

    for domain in ror_domains:
        property_value = {
            "@type": "PropertyValue",
            "name": "registeredDomain",
            "value": domain
        }
        domain_properties.append(property_value)

    return domain_properties
```

### JavaScript Implementation

```javascript
function mapRorDomainsToSchema(rorDomains = []) {
  if (!rorDomains || rorDomains.length === 0) {
    return [];
  }

  return rorDomains.map(domain => ({
    "@type": "PropertyValue",
    "name": "registeredDomain",
    "value": domain
  }));
}
```

### Enhanced Implementation with Validation

```javascript
function validateAndMapDomains(rorDomains) {
  const domainPattern = /^[a-z0-9]+([\-\.]{1}[a-z0-9]+)*\.[a-z]{2,}$/;

  const validDomains = rorDomains.filter(domain => {
    if (!domainPattern.test(domain)) {
      console.warn(`Invalid domain format: ${domain}`);
      return false;
    }
    return true;
  });

  return validDomains.map(domain => ({
    "@type": "PropertyValue",
    "name": "registeredDomain",
    "value": domain
  }));
}
```

---

## Validation Examples

### Test Case 1: Array Structure

```javascript
const input = ["mit.edu", "csail.mit.edu"];
const result = mapRorDomainsToSchema(input);

console.log(Array.isArray(result)); // true
console.log(result.length); // 2
console.log(result[0]["@type"]); // "PropertyValue"
console.log(result[0].name); // "registeredDomain"
console.log(result[0].value); // "mit.edu"
```

### Test Case 2: Empty Array

```javascript
const input = [];
const result = mapRorDomainsToSchema(input);

console.log(Array.isArray(result)); // true
console.log(result.length); // 0
```

### Test Case 3: Multiple Domains

```javascript
const input = ["harvard.edu", "hms.harvard.edu", "hsph.harvard.edu"];
const result = mapRorDomainsToSchema(input);

console.log(result.length); // 3
console.log(result[0].value); // "harvard.edu"
console.log(result[1].value); // "hms.harvard.edu"
console.log(result[2].value); // "hsph.harvard.edu"
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

  // Add domains as individual PropertyValues
  if (rorRecord.domains && rorRecord.domains.length > 0) {
    const domainProperties = validateAndMapDomains(rorRecord.domains);
    if (domainProperties.length > 0) {
      schemaOrg.additionalProperty = domainProperties;
    }
  }

  return schemaOrg;
}

// Usage with real data
const rorRecord = {
  "id": "https://ror.org/02pq29p90",
  "names": [{"value": "University of Pisa", "types": ["ror_display"]}],
  "domains": ["unipi.it", "di.unipi.it", "iet.unipi.it"]
};

const schemaOutput = processRorRecord(rorRecord);
console.log(JSON.stringify(schemaOutput, null, 2));

// Verify each domain is its own PropertyValue
console.log(schemaOutput.additionalProperty.length); // 3
console.log(schemaOutput.additionalProperty[0].value); // "unipi.it"
console.log(schemaOutput.additionalProperty[1].value); // "di.unipi.it"
```

### Combining with Other Properties

```javascript
// When combining domains with other additionalProperties
function combineAdditionalProperties(rorRecord) {
  let additionalProperties = [];

  // Add domains
  if (rorRecord.domains) {
    additionalProperties = additionalProperties.concat(
      mapRorDomainsToSchema(rorRecord.domains)
    );
  }

  // Add other properties like status
  if (rorRecord.status) {
    additionalProperties.push({
      "@type": "PropertyValue",
      "name": "registryStatus",
      "value": rorRecord.status
    });
  }

  return additionalProperties;
}
```

---

*Example created: 2024-09-15*
*Based on mapping rules: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:30*
*Related analysis: domains/analysis.md*