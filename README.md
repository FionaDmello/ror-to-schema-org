# ROR Schema Mappings

This repository contains mappings between Research Organization Registry (ROR) schema versions and Schema.org vocabulary using the Simple Standard for Sharing Ontological Mappings (SSSOM) format.

## Project Structure

- `v1/` - ROR v1.0 schema mappings
- `v2/` - ROR v2.0 schema mappings
- `json/` - JSON format mappings
- `pyproject.toml` - Python project configuration (Poetry)
- `poetry.lock` - Locked dependencies

## ROR v2.0 Mappings

The v2.0 mappings include:
- Sorted SSSOM mapping files
- Type mapping tables with URI vs Text distinctions
- Processing statistics and summaries
- Depth-based sorted mappings

## Key Features

- **SSSOM Format**: Standardized ontological mappings
- **Confidence Scoring**: Mappings include confidence levels
- **Predicate Types**: exactMatch, narrowMatch, broadMatch, relatedMatch
- **Type Analysis**: Detailed analysis of ROR and Schema.org value types
- **Depth-based Organization**: Hierarchical sorting of schema fields

## Usage

This repository is primarily for research and reference purposes, providing mappings between ROR schema fields and Schema.org properties for semantic web applications and data integration.

## License

MIT
