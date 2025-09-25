# ROR to Schema.org Mappings

This repository provides comprehensive mappings from Research Organization Registry (ROR) schema fields to Schema.org properties using SSSOM format, covering all major ROR v2.1 schema elements with extensive documentation and real-world examples.

## What's Here

### Core Mappings (Complete Coverage)
- **SSSOM files** (`mappings/sssom/`) - Full mapping files with SSSOM metadata headers
  - `ror_to_schema.sssom.tsv` - Complete mappings (65 field mappings, 96 lines total)
  - `ror_to_schema_human_in_loop.sssom.tsv` - Human-reviewed refinements (53 mappings, 74 lines)
- **TSV files** (`mappings/tsv/`) - Clean mapping tables for easy import
  - `ror_to_schema.tsv` - Complete mappings without metadata headers
  - `ror_to_schema_human_in_loop.tsv` - Human-reviewed version without headers

### Comprehensive Documentation
- **Field-by-field analysis** (`docs/`) - Complete mapping rationale for all ROR fields:
  - `id` - Primary ROR identifier PropertyValue mapping for structural consistency
  - `external_ids` - External identifier PropertyValue mapping with valueReference approach
  - `admin/` - Administrative metadata lifecycle mapping
  - `domains/` - Organization domain name mapping
  - `established/` - Establishment date mapping
  - `external_ids/` - External identifier PropertyValue mapping with valueReference approach
  - `id/` - Primary ROR identifier PropertyValue mapping for structural consistency
  - `links/` - External website and Wikipedia link mapping
  - `names/` - Multilingual name handling with StructuredValue approach
  - `organization_types/` - ROR type to Schema.org Organization subclass mapping
  - `status/` - Organization status field mapping
- **Implementation examples** - Complete JSON-LD output examples for each field group
- **Mapping analysis** - Detailed semantic reasoning for each mapping decision

### Reference Materials
- **ROR schemas** (`context/ror/`) - Official JSON schema definitions
  - `ror_schema_v2_0.json` - ROR Schema v2.0 specification
  - `ror_schema_v2_1.json` - ROR Schema v2.1 specification
- **Example records** (`context/ror_examples/`) - Real ROR organization data
  - `example_record_v2_0.json` - Sample v2.0 ROR record
  - `example_record_v2_1.json` - Sample v2.1 ROR record
- **Schema.org documentation** (`context/schema_org/`) - Official Schema.org documentation
  - `schemaorg-current-https.jsonld` - JSON-LD context definitions for Schema.org

## Mapping Coverage & Quality

### Complete Field Coverage
- **54 unique ROR fields** mapped to Schema.org properties
- **64 total mappings** including nested field structures
- **All major ROR v2.1 fields** covered: id, names, types, links, domains, established, status, locations, external_ids, admin
- **High confidence mappings**: 85-95% confidence scores with semantic validation

### Key Mapping Insights

#### Names & Multilingual Support
- **Content-primary approach**: `ROR:names.value` → `schema:name` for semantic clarity
- **Language-aware mapping**: ISO language codes to Schema.org `inLanguage` property
- **StructuredValue implementation**: Schema.org compliant multilingual name representation
- **Type differentiation**: Different name types (ror_display, label, alias) mapped appropriately

#### Organization Classification
- **Precise type mappings**: ROR organizational types to specific Schema.org classes
  - Education → EducationalOrganization
  - Healthcare → MedicalOrganization
  - Company → Corporation
  - Facility → Organization (with additionalType)
- **Semantic accuracy**: Human-reviewed mappings ensure proper Schema.org compliance

#### Location & Geographic Data
- **GeoNames integration**: Complete address mapping from ROR locations
- **Coordinate preservation**: Direct latitude/longitude mapping
- **Administrative hierarchy**: Country, subdivision, and city data properly structured
- **PostalAddress compliance**: Full Schema.org address property coverage

#### Unified Identifier System
- **PropertyValue consistency**: Both ROR.id and external_ids use PropertyValue structure
- **Semantic separation**: Primary identifiers (propertyID="ror") vs external identifiers (propertyID=system)
- **Preferred status preservation**: valueReference nested PropertyValue for external_ids.preferred
- **Bidirectional integrity**: Perfect reverse mapping from Schema.org back to ROR structure
- **Zero information loss**: All ROR identifier metadata preserved during conversion

#### External Identity & Links
- **Advanced identifier mapping**: PropertyValue with valueReference for complex external identifier structures
- **Link classification**: Website vs Wikipedia links properly differentiated
- **URL validation**: Proper Schema.org URL and sameAs property usage

## File Formats & Standards

### SSSOM Format Compliance
- **W3ID SSSOM specification**: Full compliance with https://w3id.org/sssom/ standard
- **SKOS predicate vocabulary**: exactMatch, narrowMatch, broadMatch, relatedMatch
- **Confidence scoring**: Quantified 0.0-1.0 confidence with justification comments
- **ORCID attribution**: Proper authorship metadata with https://orcid.org/0000-0002-0465-1009
- **Curie mappings**: Standard namespace prefixes (ROR:, schema:, skos:)

### TSV Format Features
- **Clean tabular structure**: Easy import into databases and analysis tools
- **Standard delimiters**: Tab-separated values for maximum compatibility
- **Header consistency**: Standardized column names across all files
- **Unicode support**: Proper handling of international characters in organization names

## Usage & Applications

### Semantic Web Integration
- **JSON-LD generation**: Convert ROR data to Schema.org structured data
- **Search engine optimization**: Enhanced organization markup for better discoverability
- **Linked data publishing**: Connect ROR organizations to broader semantic web
- **Knowledge graph construction**: Build organization-centric knowledge representations

### Research Data Integration
- **Institutional affiliation markup**: Enhance research publications with organization data
- **Grant and funding metadata**: Connect funding organizations to Schema.org FundingAgency
- **Cross-reference resolution**: Link ROR identifiers to other organization databases
- **Multilingual research platforms**: Support international organization name variants

### Technical Implementation
```bash
# View mapping statistics
wc -l mappings/tsv/*.tsv

# Extract specific field mappings
grep "ROR:names" mappings/tsv/ror_to_schema.tsv
grep "ROR:external_ids" mappings/tsv/ror_to_schema_human_in_loop.tsv

# Count unique mapped fields
cut -f1 mappings/tsv/ror_to_schema.tsv | sort -u | grep "ROR:" | wc -l

# View advanced PropertyValue mappings
grep "propertyID\|valueReference" mappings/tsv/ror_to_schema_human_in_loop.tsv
```

## Quality Assurance

### Human-in-the-Loop Review
- **Manual validation**: Expert review of automated mappings
- **Semantic accuracy**: Verification of Schema.org property usage
- **Edge case handling**: Special consideration for complex field structures
- **Documentation validation**: Cross-reference with official ROR and Schema.org specifications

### Documentation Standards
- **Analysis-driven approach**: Every mapping backed by detailed semantic analysis
- **Real-world examples**: Implementation examples using actual ROR data
- **Template consistency**: Standardized documentation format across all field analyses

## License

MIT
