# ROR Domains Implementation Example

## Overview

This document provides concrete examples of implementing the `ROR:domains` → `schema:additionalProperty` mapping using StructuredValue to properly handle arrays while respecting Schema.org type constraints.

## Schema.org Type Constraints

**Important**: `PropertyValue.value` only accepts:
- `Text` (string)
- `Number`
- `Boolean` 
- `StructuredValue`

**Arrays are NOT supported** in `PropertyValue.value`. We use `StructuredValue` to handle ROR's array of domains.

## ROR Source Data Examples

### Single Domain
```json
{
  "id": "https://ror.org/02jbv0t02",
  "domains": ["unipi.it"],
  "names": [{"value": "University of Pisa", "types": ["ror_display"]}]
}
```

### Multiple Domains
```json
{
  "id": "https://ror.org/00pjdza24",
  "domains": ["universityofcalifornia.edu", "ucop.edu"],
  "names": [{"value": "University of California System", "types": ["ror_display"]}]
}
```

### Empty Domains
```json
{
  "id": "https://ror.org/example123",
  "domains": [],
  "names": [{"value": "Example Research Institute", "types": ["ror_display"]}]
}
```

## Recommended Implementation: StructuredValue

### Single Domain Example
```json
{
  "@context": "https://schema.org",
  "@type": "Organization", 
  "@id": "https://ror.org/02jbv0t02",
  "name": "University of Pisa",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": {
      "@type": "StructuredValue",
      "additionalProperty": {
        "@type": "PropertyValue",
        "name": "domain",
        "value": "unipi.it"
      }
    },
    "description": "Domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

### Multiple Domains Example
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "@id": "https://ror.org/00pjdza24", 
  "name": "University of California System",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": {
      "@type": "StructuredValue",
      "additionalProperty": [
        {
          "@type": "PropertyValue",
          "name": "domain",
          "value": "universityofcalifornia.edu"
        },
        {
          "@type": "PropertyValue",
          "name": "domain",
          "value": "ucop.edu"
        }
      ]
    },
    "description": "Domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

## Handling Empty Domains Arrays

When ROR `domains` is an empty array `[]`, there are several implementation approaches. Choose based on your application's requirements:

### Option 1: Omit Property Entirely (Recommended for Most Cases)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://ror.org/example123",
  "name": "Example Research Institute"
  // No additionalProperty for domains when array is empty
}
```

**Pros:**
- Clean and minimal
- Follows Schema.org principle of only including meaningful data
- No structural noise
- Standard semantic web practice

**Cons:**
- Cannot distinguish between "no domains" vs "data not available"
- May complicate queries that expect consistent property presence

**Use When:** You want clean, minimal markup and don't need to distinguish between absence and emptiness.

### Option 2: Explicit "No Domains" Statement
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://ror.org/example123",
  "name": "Example Research Institute",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": "none",
    "description": "No domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

**Pros:**
- Explicitly states that domains were checked and none exist
- Distinguishes between "no domains" vs "unknown/missing data"
- Maintains consistent property structure across records
- Good for data validation and completeness checking

**Cons:**
- Creates additional property for negative information
- Requires special handling of "none" value

**Use When:** You need to explicitly indicate that domain information was available but empty, or maintain consistent property structure.

### Option 3: Empty StructuredValue (Not Recommended)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://ror.org/example123",
  "name": "Example Research Institute",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": {
      "@type": "StructuredValue",
      "additionalProperty": []
    },
    "description": "Domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

**Pros:**
- Maintains consistent structural pattern
- Preserves the StructuredValue container

**Cons:**
- Creates empty structural containers that add no semantic value
- More verbose than necessary
- Potential confusion about meaning of empty structure

**Use When:** Your application architecture requires consistent property structure regardless of content.

### Option 4: Status Indicator
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://ror.org/example123",
  "name": "Example Research Institute",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomainsStatus",
    "value": "no-registered-domains",
    "description": "Institution has no registered domain names",
    "propertyID": "ROR:domains"
  }
}
```

**Pros:**
- Clear semantic indication of status
- Distinguishes different states (none, unknown, multiple, etc.)
- Good for data quality tracking

**Cons:**
- Different property name/structure than non-empty cases
- Requires status value vocabulary

**Use When:** You need rich metadata about the state of domain information.

## Implementation Decision Guide

| Use Case | Recommended Option | Rationale |
|----------|-------------------|-----------|
| **Web markup/SEO** | Option 1 (Omit) | Clean, minimal markup |
| **Data integration** | Option 2 (Explicit "none") | Clear data completeness |
| **API responses** | Option 1 (Omit) or Option 2 | Depends on API design philosophy |
| **Database export** | Option 2 (Explicit "none") | Maintain data integrity |
| **Research datasets** | Option 2 (Explicit "none") | Important to distinguish missing vs. empty |

## Advanced Examples

### With Domain Metadata
```json
{
  "@context": "https://schema.org",
  "@type": "EducationalOrganization",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": {
      "@type": "StructuredValue",
      "additionalProperty": [
        {
          "@type": "PropertyValue",
          "name": "domain",
          "value": "universityofcalifornia.edu",
          "description": "Primary institutional domain"
        },
        {
          "@type": "PropertyValue", 
          "name": "domain",
          "value": "ucop.edu",
          "description": "Office of the President domain"
        }
      ],
      "description": "Complete set of registered institutional domains"
    },
    "description": "Domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

### Handling Multi-Domain Strings (Future-Proofing)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "additionalProperty": {
    "@type": "PropertyValue",
    "name": "registeredDomains",
    "value": {
      "@type": "StructuredValue",
      "additionalProperty": {
        "@type": "PropertyValue",
        "name": "domainGroup",
        "value": "example.edu,research.example.edu",
        "description": "Multiple domains in single string"
      }
    },
    "description": "Domain names registered to this institution",
    "propertyID": "ROR:domains"
  }
}
```

## Implementation Algorithm

### Python Conversion Function
```python
def convert_ror_domains_to_schema(ror_domains, empty_handling="omit"):
    """
    Convert ROR domains array to Schema.org StructuredValue
    
    Args:
        ror_domains: List of domain strings from ROR
        empty_handling: "omit", "explicit_none", "empty_structure", or "status"
    """
    
    if not ror_domains:  # Empty array
        if empty_handling == "omit":
            return None  # Omit property entirely
        elif empty_handling == "explicit_none":
            return {
                "@type": "PropertyValue",
                "name": "registeredDomains",
                "value": "none",
                "description": "No domain names registered to this institution",
                "propertyID": "ROR:domains"
            }
        elif empty_handling == "empty_structure":
            return {
                "@type": "PropertyValue",
                "name": "registeredDomains",
                "value": {
                    "@type": "StructuredValue",
                    "additionalProperty": []
                },
                "description": "Domain names registered to this institution",
                "propertyID": "ROR:domains"
            }
        elif empty_handling == "status":
            return {
                "@type": "PropertyValue",
                "name": "registeredDomainsStatus",
                "value": "no-registered-domains",
                "description": "Institution has no registered domain names",
                "propertyID": "ROR:domains"
            }
    
    # Non-empty domains
    domain_properties = []
    for domain in ror_domains:
        domain_properties.append({
            "@type": "PropertyValue",
            "name": "domain",
            "value": domain
        })
    
    return {
        "@type": "PropertyValue",
        "name": "registeredDomains",
        "value": {
            "@type": "StructuredValue", 
            "additionalProperty": domain_properties if len(domain_properties) > 1 else domain_properties[0]
        },
        "description": "Domain names registered to this institution",
        "propertyID": "ROR:domains"
    }
```

## Validation Guidelines

### Required Structure (Non-Empty)
```json
{
  "additionalProperty": {
    "@type": "PropertyValue",           // Required
    "name": "registeredDomains",        // Consistent naming
    "value": {
      "@type": "StructuredValue",       // Required for complex data
      "additionalProperty": [...]       // Domain PropertyValues
    },
    "propertyID": "ROR:domains",        // Source reference
    "description": "..."                // Human-readable explanation
  }
}
```

### Validation Checklist
- [ ] Choose consistent empty array handling approach
- [ ] PropertyValue contains StructuredValue in `value` field (when present)
- [ ] Each domain wrapped in PropertyValue with `name: "domain"`
- [ ] Consistent `propertyID: "ROR:domains"` for traceability
- [ ] Include descriptive `description` field
- [ ] Validate domain strings match ROR pattern
- [ ] Document chosen empty handling approach

## References

- [Schema.org StructuredValue](https://schema.org/StructuredValue)
- [Schema.org PropertyValue](https://schema.org/PropertyValue) 
- [Schema.org additionalProperty](https://schema.org/additionalProperty)
- [ROR Domains Documentation](https://ror.readme.io/docs/ror-data-structure)
- [JSON-LD Best Practices](https://www.w3.org/TR/json-ld11-api/)