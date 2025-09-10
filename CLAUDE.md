# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Project Overview

This repository maps Research Organization Registry (ROR) schema fields to Schema.org properties using SSSOM format. It's a research project for semantic web applications and data integration.

## Key Commands

### Environment Setup
```bash
poetry install    # Install dependencies
poetry shell     # Activate virtual environment
```

### Testing
```bash
poetry run pytest    # Run tests
```

## File Structure

### Core Mappings
- `mappings/sssom/` - SSSOM mapping files with metadata headers
  - `ror_to_schema.sssom.tsv` - Machine-generated mappings
  - `ror_to_schema_human_in_loop.sssom.tsv` - Human-reviewed mappings
- `mappings/tsv/` - Clean TSV files without metadata
  - `ror_to_schema.tsv` - Machine-generated mappings (clean)
  - `ror_to_schema_human_in_loop.tsv` - Human-reviewed mappings (clean)

### Reference Materials
- `json/ror/` - ROR JSON schema definitions (v2.0, v2.1)
- `json/ror_examples/` - Example ROR records
- `documentation/` - Analysis and examples
  - `names/` - Name mapping analysis and examples
  - `status/` - Status field mapping analysis

### Configuration
- `pyproject.toml` - Poetry dependencies (pandas/numpy)

## Mapping Approach

### SSSOM Format
- **Metadata headers**: Curie mappings (ROR:, schema:, skos:)
- **Columns**: subject_id, object_id, predicate_id, confidence, comment
- **Predicates**: exactMatch, narrowMatch, broadMatch, relatedMatch
- **Confidence**: 0.0-1.0 scoring
- **Attribution**: ORCID-based authorship

### Key Insights
- **Names mapping**: `ROR:names.value` → `schema:name` (content-primary approach)
- **Multilingual support**: StructuredValue with Language objects for Schema.org compliance
- **Type mappings**: ROR organizational types to specific Schema.org classes
- **Location data**: GeoNames details to Schema.org address properties

## Development Notes

- This is a data mapping project, not executable code
- Human-in-loop files contain manually reviewed and corrected mappings
- Documentation provides analysis of complex mapping decisions
- Focus on semantic accuracy and Schema.org compliance