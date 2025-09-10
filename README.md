# ROR to Schema.org Mappings

This repository maps Research Organization Registry (ROR) schema fields to Schema.org properties using SSSOM format.

## What's Here

### Core Mappings
- **SSSOM files** (`mappings/sssom/`) - Full mapping files with metadata
- **TSV files** (`mappings/tsv/`) - Clean mapping tables without headers
- **Human-reviewed** versions include manual corrections and refinements

### Documentation
- **Name mapping analysis** (`documentation/names/`) - Detailed analysis of multilingual name handling
- **Complete examples** - Real-world mapping examples with Schema.org output
- **Status mapping analysis** (`documentation/status/`) - Organization status field analysis

### Reference Materials
- **ROR schemas** (`json/ror/`) - v2.0 and v2.1 JSON schema definitions
- **Example records** (`json/ror_examples/`) - Sample ROR organization records

## Key Mapping Insights

### Names Approach
- **Content-primary**: `ROR:names.value` maps to `schema:name`
- **Type-based**: Different name types (label, alias, acronym) map to appropriate Schema.org properties
- **Multilingual**: Schema.org compliant approach using StructuredValue with Language objects

### Organization Types
- **Direct mappings**: Education → EducationalOrganization, Funder → FundingAgency
- **Narrow mappings**: Healthcare → MedicalOrganization, Company → Corporation
- **High confidence**: Most mappings have 0.85+ confidence scores

### Location Data
- **GeoNames integration**: ROR location details map to Schema.org address properties
- **Coordinate mapping**: Latitude/longitude preserved directly
- **Regional data**: Country, subdivision, and continent information mapped appropriately

## File Formats

### SSSOM Format
- Standard ontological mapping format
- Includes confidence scores (0.0-1.0)
- Uses SKOS predicates (exactMatch, narrowMatch, etc.)
- ORCID-based attribution

### TSV Format
- Clean tabular data
- Easy to import into other systems
- Same mapping data without SSSOM metadata

## Usage

Use these mappings to convert ROR organizational data to Schema.org markup for:
- Search engine optimization
- Semantic web applications
- Linked data publishing
- Research data integration

## License

MIT
