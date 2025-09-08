# ROR Schema Type Mapping - Depth-Based Sorting Summary

## Overview
The mapping files have been sorted by grouping ROR keys based on their hierarchical depth while maintaining the original sort order from the SSSOM file within each depth level.

## Sorting Strategy

### 1. Depth Calculation
- **Depth 0**: Top-level keys (e.g., `id`, `names`, `types`, `locations`)
- **Depth 1**: First-level nested keys (e.g., `names.value`, `types.funder`, `locations.geonames_id`)
- **Depth 2**: Second-level nested keys (e.g., `locations.geonames_details.lat`, `admin.created.date`)

### 2. Sort Order Within Each Depth
Within each depth level, keys are sorted according to their appearance order in the original sorted SSSOM file (`ror_v2_schema_org.sssom_sorted.tsv`), which prioritizes:
1. `skos:exactMatch` relationships (highest confidence)
2. `skos:narrowMatch` relationships
3. `skos:broadMatch` relationships  
4. `skos:relatedMatch` relationships (lowest confidence)

### 3. Depth Distribution
- **Depth 0**: 13 mappings (top-level ROR schema fields)
- **Depth 1**: 26 mappings (first-level nested properties)
- **Depth 2**: 9 mappings (second-level nested properties)

## Files Created

### Sorted by Depth
- `ror_schema_type_mapping_simple_sorted_by_depth.md` - Markdown table format
- `ror_schema_type_mapping_simple_sorted_by_depth.csv` - CSV format

### Benefits of Depth-Based Sorting
1. **Hierarchical Organization**: Groups related fields together by their structural depth
2. **Maintains SSSOM Order**: Preserves the confidence-based ordering within each depth level
3. **Improved Readability**: Easier to understand the schema structure at a glance
4. **Logical Grouping**: Related nested properties appear together

## Example Depth Groupings

### Depth 0 (Top-level)
- `id` → `schema:identifier`
- `names` → `schema:name`
- `types` → `schema:additionalType`
- `locations` → `schema:address`

### Depth 1 (First-level nested)
- `names.value` → `schema:name`
- `names.lang` → `schema:inLanguage`
- `types.funder` → `schema:FundingAgency`
- `locations.geonames_id` → `schema:geoMidpoint`

### Depth 2 (Second-level nested)
- `locations.geonames_details.lat` → `schema:latitude`
- `locations.geonames_details.lng` → `schema:longitude`
- `admin.created.date` → `schema:dateCreated`
- `admin.last_modified.date` → `schema:dateModified`

This organization makes it easier to understand the ROR schema structure while maintaining the semantic relationships established in the original SSSOM mapping.
