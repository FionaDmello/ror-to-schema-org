# URI vs Text Distinction in ROR v2 Schema Mappings

## Overview
This document provides a clear distinction between fields that expect URIs versus text values in the ROR v2 schema to Schema.org mappings.

## URI Fields (3 fields)

### Fields Requiring Valid URIs
| ROR Field | Type | Example | Validation Pattern | Notes |
|-----------|------|---------|-------------------|-------|
| `id` | URI | `https://ror.org/05gq02987` | `https://ror.org/0[^ilo]{6}\d{2}` | ROR persistent identifier |
| `links.value` | URI | `https://example.org` | Valid URL format | Actual URL values |
| `relationships.id` | URI | `https://ror.org/04kdfz702` | Valid ROR URI | Related organization ID |

### URI Validation Rules
- **ROR IDs**: Must match pattern `https://ror.org/0[^ilo]{6}\d{2}`
- **Link Values**: Must be valid URLs (http/https)
- **Relationship IDs**: Must be valid ROR URIs

## Text Fields (20+ fields)

### Fields Requiring Text Values
| ROR Field | Type | Example | Validation | Notes |
|-----------|------|---------|------------|-------|
| `names.value` | text | `"Harvard University"` | UTF-8 text | Organization names |
| `names.lang` | text (ISO 639-3) | `"en"` | ISO 639-3 codes | Language codes |
| `types.funder` | text | `"funder"` | Controlled vocabulary | Organization type |
| `types.government` | text | `"government"` | Controlled vocabulary | Organization type |
| `types.education` | text | `"education"` | Controlled vocabulary | Organization type |
| `types.company` | text | `"company"` | Controlled vocabulary | Organization type |
| `types.archive` | text | `"archive"` | Controlled vocabulary | Organization type |
| `types.healthcare` | text | `"healthcare"` | Controlled vocabulary | Organization type |
| `types.nonprofit` | text | `"nonprofit"` | Controlled vocabulary | Organization type |
| `types.facility` | text | `"facility"` | Controlled vocabulary | Organization type |
| `types.other` | text | `"other"` | Controlled vocabulary | Organization type |
| `status` | text | `"active"` | Controlled vocabulary | Organization status |
| `locations.geonames_details.country_code` | text (ISO 3166-1) | `"US"` | ISO 3166-1 codes | Country codes |
| `locations.geonames_details.country_name` | text | `"United States"` | UTF-8 text | Country names |
| `locations.geonames_details.name` | text | `"Cambridge"` | UTF-8 text | Location names |
| `links.type` | text | `"website"` | Controlled vocabulary | Link types |
| `external_ids.type` | text | `"grid"` | Controlled vocabulary | ID type context |
| `relationships.label` | text | `"Harvard Medical School"` | UTF-8 text | Related org names |
| `relationships.type` | text | `"parent"` | Controlled vocabulary | Relationship types |
| `admin.created.schema_version` | text | `"2.0"` | Version format | Schema versions |
| `admin.last_modified.schema_version` | text | `"2.1"` | Version format | Schema versions |

### Text Validation Rules
- **General Text**: Must be valid UTF-8 text
- **Language Codes**: Must be valid ISO 639-3 codes
- **Country Codes**: Must be valid ISO 3166-1 codes
- **Dates**: Must be valid ISO 8601 format
- **Organization Types**: Must be from controlled vocabulary
- **Relationship Types**: Must be from controlled vocabulary

## Array Fields

### Arrays of Text
| ROR Field | Type | Example | Notes |
|-----------|------|---------|-------|
| `types` | array of text | `["education", "funder"]` | Multiple organization types |
| `domains` | array of text | `["harvard.edu", "hms.harvard.edu"]` | Organization domains |
| `external_ids.all` | array of text | `["grid.38142.3c", "0000 0004 1936 754X"]` | All external identifiers |
| `names.types` | array of text | `["ror_display", "label"]` | Name type classifications |

### Arrays of Objects
| ROR Field | Type | Example | Notes |
|-----------|------|---------|-------|
| `names` | array of objects | `[{"value": "Harvard", "lang": "en"}]` | Name objects with metadata |
| `locations` | array of objects | `[{"geonames_details": {...}}]` | Location objects with details |
| `relationships` | array of objects | `[{"id": "...", "type": "parent"}]` | Relationship objects |
| `external_ids` | array of objects | `[{"type": "grid", "all": [...]}]` | External ID objects |

## Numeric Fields

### Integer Fields
| ROR Field | Type | Example | Notes |
|-----------|------|---------|-------|
| `established` | integer (year) | `1636` | Founding year |
| `locations.geonames_id` | integer | `4930956` | GeoNames identifier |

### Float Fields
| ROR Field | Type | Example | Notes |
|-----------|------|---------|-------|
| `locations.geonames_details.lat` | number (float) | `42.3736` | Geographic latitude |
| `locations.geonames_details.lng` | number (float) | `-71.1097` | Geographic longitude |

## Object Fields

### Complex Objects
| ROR Field | Type | Example | Notes |
|-----------|------|---------|-------|
| `admin` | object | `{"created": {...}, "last_modified": {...}}` | Administrative metadata |
| `admin.created` | object | `{"date": "2018-11-14", "schema_version": "1.0"}` | Creation information |
| `admin.last_modified` | object | `{"date": "2024-12-11", "schema_version": "2.1"}` | Modification information |

## Implementation Guidelines

### URI Processing
1. **Validate URI format** before processing
2. **Check ROR ID pattern** for ROR identifiers
3. **Verify URL accessibility** for link values
4. **Resolve relationship IDs** to ensure they exist

### Text Processing
1. **Validate UTF-8 encoding** for all text fields
2. **Check controlled vocabularies** for type fields
3. **Validate format constraints** (ISO codes, dates)
4. **Normalize text values** for consistency

### Type Safety
1. **Use strict type checking** in data pipelines
2. **Implement validation schemas** for each field type
3. **Handle type conversion** carefully between ROR and Schema.org
4. **Log type mismatches** for debugging

## Summary

- **URI Fields**: 3 fields requiring strict URI validation
- **Text Fields**: 20+ fields with various text formats and constraints
- **Array Fields**: 8 fields containing arrays of text or objects
- **Numeric Fields**: 4 fields with integer or float values
- **Object Fields**: 3 complex nested object structures

This distinction enables precise type validation and ensures data integrity in ROR v2 to Schema.org transformations.
