# ROR v2 Schema to Schema.org Type Mapping (Simplified)

## Table: ROR Schema Keys and Type Mappings

| ROR Key | ROR Value Type | Schema.org Key | Schema.org Value Type |
|---------|----------------|----------------|----------------------|
| **Core Identifiers** |
| `id` | URI | `schema:identifier` | Text |
| **Names and Labels** |
| `names` | array of objects | `schema:name` | Text |
| `names.value` | text | `schema:name` | Text |
| `names.lang` | text (ISO 639-3) | `schema:inLanguage` | Text |
| `names.types` | array of text | `schema:additionalProperty` | PropertyValue |
| **Organization Types** |
| `types` | array of text | `schema:additionalType` | Text |
| `types.funder` | text | `schema:FundingAgency` | Class |
| `types.government` | text | `schema:GovernmentOrganization` | Class |
| `types.education` | text | `schema:EducationalOrganization` | Class |
| `types.company` | text | `schema:Corporation` | Class |
| `types.archive` | text | `schema:ArchiveOrganization` | Class |
| `types.healthcare` | text | `schema:MedicalOrganization` | Class |
| `types.nonprofit` | text | `schema:NGO` | Class |
| `types.facility` | text | `schema:ResearchOrganization` | Class |
| `types.other` | text | `schema:Organization` | Class |
| **Temporal Information** |
| `established` | integer (year) | `schema:foundingDate` | Date |
| `status` | text | `schema:dissolutionDate` | Date |
| **Location Information** |
| `locations` | array of objects | `schema:address` | PostalAddress |
| `locations.geonames_details.lat` | number (float) | `schema:latitude` | Number |
| `locations.geonames_details.lng` | number (float) | `schema:longitude` | Number |
| `locations.geonames_details.country_code` | text (ISO 3166-1) | `schema:addressCountry` | Text |
| `locations.geonames_details.country_name` | text | `schema:addressCountry` | Text |
| `locations.geonames_details.name` | text | `schema:addressLocality` | Text |
| `locations.geonames_id` | integer | `schema:geoMidpoint` | GeoCoordinates |
| **Web and Links** |
| `domains` | array of text | `schema:url` | URL |
| `links` | array of objects | `schema:url` | URL |
| `links.value` | URI | `schema:url` | URL |
| `links.type` | text | `schema:additionalProperty` | PropertyValue |
| **External Identifiers** |
| `external_ids` | array of objects | `schema:sameAs` | URL |
| `external_ids.all` | array of text | `schema:sameAs` | URL |
| `external_ids.preferred` | text | `schema:sameAs` | URL |
| `external_ids.type` | text | `schema:additionalProperty` | PropertyValue |
| **Relationships** |
| `relationships` | array of objects | `schema:parentOrganization` | Organization |
| `relationships.id` | URI | `schema:parentOrganization` | Organization |
| `relationships.label` | text | `schema:name` | Text |
| `relationships.type` | text | `schema:parentOrganization` | Organization |
| `relationships.type` | text | `schema:subOrganization` | Organization |
| `relationships.type` | text | `schema:memberOf` | Organization |
| `relationships.type` | text | `schema:affiliation` | Organization |
| **Administrative Metadata** |
| `admin` | object | `schema:CreativeWork` | Class |
| `admin.created` | object | `schema:dateCreated` | Date |
| `admin.created.date` | text (ISO 8601) | `schema:dateCreated` | Date |
| `admin.created.schema_version` | text | `schema:version` | Text |
| `admin.last_modified` | object | `schema:dateModified` | Date |
| `admin.last_modified.date` | text (ISO 8601) | `schema:dateModified` | Date |
| `admin.last_modified.schema_version` | text | `schema:version` | Text |
| **Generic Organization Types** |
| `*` (ROR Record) | object | `schema:ResearchOrganization` | Class |
| `*` (ROR Record) | object | `schema:Organization` | Class |

## Summary

- **Total Mappings**: 58
- **Unique ROR Keys**: 53
- **Unique Schema.org Keys**: 42

## Data Type Distribution

### ROR Value Types
- **text**: 20 fields
- **URI**: 3 fields
- **array of text**: 3 fields
- **array of objects**: 4 fields
- **object**: 3 fields
- **integer**: 2 fields
- **number (float)**: 2 fields

### Schema.org Value Types
- **Text**: 15 fields
- **URL**: 8 fields
- **Class**: 12 fields
- **Date**: 4 fields
- **Number**: 2 fields
- **PropertyValue**: 4 fields
- **PostalAddress**: 1 field
- **GeoCoordinates**: 1 field
- **Organization**: 1 field
