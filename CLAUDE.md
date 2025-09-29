# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Project Overview

This repository maps Research Organization Registry (ROR) schema fields to Schema.org properties using SSSOM format. It's a comprehensive research project for semantic web applications and data integration, covering all major ROR v2.1 schema fields.

## Key Commands

### Environment Setup
No dependency management tools found - this is a pure data mapping project.

### Analysis
```bash
# View mapping statistics
wc -l mappings/tsv/*.tsv
# Count unique mapped fields
cut -f1 mappings/tsv/ror_to_schema.tsv | sort -u | grep "ROR:" | wc -l
```

## File Structure

### Core Mappings
- `mappings/sssom/` - SSSOM mapping files with metadata headers (74-96 lines)
  - `ror_to_schema.sssom.tsv` - Complete field mappings (65 mappings)
  - `ror_to_schema_human_in_loop.sssom.tsv` - Human-reviewed mappings (53 mappings)
- `mappings/tsv/` - Clean TSV files without metadata headers
  - `ror_to_schema.tsv` - Complete field mappings (clean format)
  - `ror_to_schema_human_in_loop.tsv` - Human-reviewed mappings (clean format)

### Reference Materials
- `context/ror/` - ROR JSON schema definitions
  - `ror_schema_v2_0.json` - ROR Schema v2.0 specification
  - `ror_schema_v2_1.json` - ROR Schema v2.1 specification
- `context/ror_examples/` - Example ROR records
  - `example_record_v2_0.json` - Sample v2.0 ROR record
  - `example_record_v2_1.json` - Sample v2.1 ROR record

### Documentation (Comprehensive Analysis)
- `docs/admin/` - Administrative metadata mapping analysis
- `docs/domains/` - Domain field mapping analysis
- `docs/established/` - Establishment date mapping analysis
- `docs/external_ids/` - External identifier PropertyValue mapping with valueReference
- `docs/id/` - Primary ROR identifier PropertyValue mapping analysis
- `docs/links/` - External links mapping analysis
- `docs/names/` - Name field mapping analysis (multilingual approach)
- `docs/organization_types/` - Organization type classification analysis
- `docs/status/` - Organization status field analysis
- `docs/locations/` - Location field hierarchical Place mapping analysis
- `docs/relationships/` - Organizational relationships mapping analysis

Each documentation folder contains:
- `analysis.md` - Detailed field analysis and mapping rationale
- `implementation_example.md` - Complete Schema.org JSON-LD examples

## Mapping Coverage

### Current Status
- **54 unique ROR fields** mapped to Schema.org properties
- **64 total mappings** including nested field mappings
- **Complete coverage** of all major ROR v2.1 schema fields

### SSSOM Format
- **Metadata headers**: Curie mappings (ROR:, schema:, skos:)
- **Standard columns**: subject_id, object_id, predicate_id, confidence, comment
- **SKOS predicates**: exactMatch (primary), narrowMatch, broadMatch, relatedMatch
- **Confidence scoring**: 0.85-0.95 range for high-quality mappings
- **ORCID attribution**: https://orcid.org/0000-0002-0465-1009

### Key Mapping Insights
- **Names approach**: Content-primary mapping with `ROR:names.value` → `schema:name`
- **Multilingual support**: StructuredValue with Language objects for Schema.org compliance
- **Administrative data**: Complete lifecycle tracking with `admin` field mapping
- **Type mappings**: ROR organizational types to specific Schema.org Organization subclasses
- **Hierarchical location mapping**: GeoNames data to Schema.org Place hierarchy with containedInPlace relationships
- **Unified identifier system**: ROR.id and external_ids both mapped to `schema:identifier` using PropertyValue structure
- **Bidirectional integrity**: Perfect reverse mapping via PropertyValue with valueReference for preferred status
- **Structural consistency**: Uniform PropertyValue arrays eliminate mixed-type identifier structures

## Development Notes

- **Pure data mapping project** - no executable code or dependencies
- **Schema compliance focus** - all mappings validated against Schema.org specifications
- **Human-in-loop refinement** - manually reviewed versions with improved semantic accuracy
- **Documentation-driven** - extensive analysis supporting each mapping decision
- **Real-world examples** - implementation examples show complete JSON-LD output
- **Semantic correctness priority** - prioritizes data integrity and bidirectional mapping over implementation simplicity

## Advanced Mapping Techniques

### PropertyValue with valueReference
For complex identifier structures (external_ids), the project uses:
- **PropertyValue base structure** for all identifier types
- **valueReference nested objects** to preserve preferred status metadata
- **propertyID differentiation** to distinguish primary from external identifiers
- **Zero information loss** during bidirectional conversion

### Hierarchical Place Mapping (Updated 2024-09-29)
For geographic location data (locations), the project implements:
- **containedInPlace hierarchy**: Place → State → Country → Continent structure
- **Complete geographic preservation**: All 10+ ROR location fields mapped without conflicts
- **Bidirectional integrity**: Perfect round-trip conversion capability
- **Schema.org compliance**: Uses proper Place, State, Country, Continent types

### Hybrid Organizational Relationships (Updated 2024-09-29)
For organizational relationships (relationships), the project uses:
- **Type-specific routing**: Direct Schema.org properties for clear equivalents (parent/child)
- **Structured preservation**: PropertyValue approach for ambiguous relationships (related, successor, predecessor)
- **Semantic accuracy**: Eliminates incorrect action-based mappings (replacer/replacee)
- **Implementation efficiency**: Clear routing logic based on relationship.type enum values
