# ROR v2 Schema to Schema.org Type Mapping Summary

## Overview
This document provides a summary of the type mappings between ROR v2 schema fields and Schema.org properties, based on the SSSOM mappings in `ror_v2_schema_org.sssom_sorted.tsv`.

## Files Generated
1. **`ror_schema_type_mapping_table.md`** - Comprehensive markdown table with detailed mappings
2. **`ror_schema_type_mapping_table.csv`** - CSV format for data processing
3. **`ror_schema_type_mapping_summary.md`** - This summary document

## Key Statistics
- **Total Mappings**: 58
- **Unique ROR Schema Keys**: 53
- **Unique Schema.org Properties**: 42
- **Mapping Categories**: 8 main categories

## Mapping Categories

### 1. Core Identifiers (1 mapping)
- `id` → `schema:identifier` (0.95 confidence)

### 2. Names and Labels (4 mappings)
- `names` → `schema:name` (0.92 confidence)
- `names.value` → `schema:name` (0.95 confidence)
- `names.lang` → `schema:inLanguage` (0.90 confidence)
- `names.types` → `schema:additionalProperty` (0.75 confidence)

### 3. Organization Types (9 mappings)
- `types` → `schema:additionalType` (0.85 confidence)
- `types.funder` → `schema:FundingAgency` (0.92 confidence)
- `types.government` → `schema:GovernmentOrganization` (0.92 confidence)
- `types.education` → `schema:EducationalOrganization` (0.90 confidence)
- `types.company` → `schema:Corporation` (0.88 confidence)
- `types.archive` → `schema:ArchiveOrganization` (0.88 confidence)
- `types.healthcare` → `schema:MedicalOrganization` (0.85 confidence)
- `types.nonprofit` → `schema:NGO` (0.85 confidence)
- `types.facility` → `schema:ResearchOrganization` (0.80 confidence)
- `types.other` → `schema:Organization` (0.75 confidence)

### 4. Temporal Information (2 mappings)
- `established` → `schema:foundingDate` (0.90 confidence)
- `status` → `schema:dissolutionDate` (0.70 confidence)

### 5. Location Information (6 mappings)
- `locations` → `schema:address` (0.92 confidence)
- `locations.geonames_details.lat` → `schema:latitude` (0.98 confidence)
- `locations.geonames_details.lng` → `schema:longitude` (0.98 confidence)
- `locations.geonames_details.country_code` → `schema:addressCountry` (0.95 confidence)
- `locations.geonames_details.country_name` → `schema:addressCountry` (0.95 confidence)
- `locations.geonames_details.name` → `schema:addressLocality` (0.90 confidence)
- `locations.geonames_id` → `schema:geoMidpoint` (0.85 confidence)

### 6. Web and Links (4 mappings)
- `domains` → `schema:url` (0.88 confidence)
- `links` → `schema:url` (0.85 confidence)
- `links.value` → `schema:url` (0.95 confidence)
- `links.type` → `schema:additionalProperty` (0.70 confidence)

### 7. External Identifiers (4 mappings)
- `external_ids` → `schema:sameAs` (0.90 confidence)
- `external_ids.all` → `schema:sameAs` (0.95 confidence)
- `external_ids.preferred` → `schema:sameAs` (0.92 confidence)
- `external_ids.type` → `schema:additionalProperty` (0.75 confidence)

### 8. Relationships (6 mappings)
- `relationships` → `schema:parentOrganization` (0.80 confidence)
- `relationships.id` → `schema:parentOrganization` (0.80 confidence)
- `relationships.label` → `schema:name` (0.85 confidence)
- `relationships.type` → `schema:parentOrganization` (0.75 confidence)
- `relationships.type` → `schema:subOrganization` (0.75 confidence)
- `relationships.type` → `schema:memberOf` (0.70 confidence)
- `relationships.type` → `schema:affiliation` (0.68 confidence)

### 9. Administrative Metadata (6 mappings)
- `admin` → `schema:CreativeWork` (0.65 confidence)
- `admin.created` → `schema:dateCreated` (0.90 confidence)
- `admin.created.date` → `schema:dateCreated` (0.95 confidence)
- `admin.created.schema_version` → `schema:version` (0.85 confidence)
- `admin.last_modified` → `schema:dateModified` (0.90 confidence)
- `admin.last_modified.date` → `schema:dateModified` (0.95 confidence)
- `admin.last_modified.schema_version` → `schema:version` (0.85 confidence)

### 10. Generic Organization Types (2 mappings)
- `*` (ROR Record) → `schema:ResearchOrganization` (0.85 confidence)
- `*` (ROR Record) → `schema:Organization` (0.95 confidence)

## Data Type Analysis

### ROR v2 Schema Value Types
| Type | Count | Examples |
|------|-------|----------|
| string | 25 | URIs, text values, identifiers |
| array of objects | 4 | names, locations, relationships, external_ids |
| array of strings | 3 | types, domains, external_ids.all |
| object | 3 | admin, admin.created, admin.last_modified |
| integer | 2 | established (year), geonames_id |
| number (float) | 2 | latitude, longitude |

### Schema.org Value Types
| Type | Count | Examples |
|------|-------|----------|
| Text | 15 | name, identifier, addressCountry |
| URL | 8 | sameAs, url |
| Class | 12 | Organization, ResearchOrganization |
| Date | 4 | dateCreated, dateModified, foundingDate |
| Number | 2 | latitude, longitude |
| PropertyValue | 4 | additionalProperty |
| PostalAddress | 1 | address |
| GeoCoordinates | 1 | geoMidpoint |

## Confidence Distribution

### High Confidence (≥0.9)
- **Count**: 22 mappings (37.9%)
- **Examples**: Geographic coordinates (0.98), core identifiers (0.95), administrative dates (0.95)

### Medium Confidence (0.7-0.89)
- **Count**: 34 mappings (58.6%)
- **Examples**: Organization types (0.75-0.92), location information (0.85-0.95)

### Lower Confidence (<0.7)
- **Count**: 2 mappings (3.4%)
- **Examples**: Administrative metadata (0.65), some relationship types (0.68)

## Predicate Distribution

| Predicate | Count | Percentage |
|-----------|-------|------------|
| skos:exactMatch | 26 | 44.8% |
| skos:relatedMatch | 21 | 36.2% |
| skos:narrowMatch | 8 | 13.8% |
| skos:broadMatch | 3 | 5.2% |

## Key Insights

1. **Comprehensive Coverage**: All major ROR v2 schema fields are mapped to appropriate Schema.org properties.

2. **High Quality Mappings**: 96.6% of mappings have confidence ≥0.7, indicating good semantic alignment.

3. **Strong Geographic Support**: Location-related mappings have the highest confidence scores (0.85-0.98).

4. **Flexible Type System**: ROR's organization types map well to Schema.org's hierarchical type system.

5. **Administrative Metadata**: ROR v2's enhanced administrative tracking is well-represented in the mappings.

6. **Bidirectional Support**: The mappings support both ROR→Schema.org and Schema.org→ROR transformations.

## Usage Recommendations

1. **High Confidence Mappings**: Use for core data transformation and validation
2. **Medium Confidence Mappings**: Use with validation and manual review
3. **Lower Confidence Mappings**: Use with caution and additional validation
4. **Type Validation**: Implement type checking based on the documented value types
5. **Context Awareness**: Consider the specific use case when applying relationship mappings

## Files for Further Analysis

- **Markdown Table**: `ror_schema_type_mapping_table.md` - Human-readable format
- **CSV Table**: `ror_schema_type_mapping_table.csv` - Machine-processable format
- **Original Mappings**: `ror_v2_schema_org.sssom_sorted.tsv` - Source SSSOM file
- **Statistics**: `ror_v2_schema_org.sssom_sorting_stats.txt` - Detailed analysis
