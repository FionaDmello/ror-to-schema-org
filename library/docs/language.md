# ROR to Schema.org Conversion Library - Planning Document

**Date**: 2025-10-01
**Status**: Planning Phase
**Target**: Production-quality bidirectional conversion library

---

## Language Selection Analysis

### Recommendation: TypeScript (Primary Choice)

#### Core Rationale

TypeScript is the optimal choice for this conversion library based on three critical factors:

1. **Complex Nested Structure Requirements**
2. **Bidirectional Conversion Integrity**
3. **Enterprise-Grade Type Safety**

---

## Detailed Language Analysis

### TypeScript Strengths

#### 1. Type-Safe Complex Structures

**Problem Context**: The ROR to Schema.org mappings involve highly nested structures:
- PropertyValue objects with nested valueReference
- Hierarchical Place structures (Place -> State -> Country -> Continent)
- Multilingual name handling with StructuredValue
- Type-specific routing for relationships (parent/child vs related/successor)


**Benefit**: Compile-time validation prevents structural errors before runtime, critical for 64+ mapping rules.

#### 2. Native JSON-LD Ecosystem

**Requirements**:
- Schema.org outputs JSON-LD format
- ROR inputs are JSON
- Need semantic web compatibility

**TypeScript Ecosystem**:
- `jsonld.js` - Official JSON-LD processor
- Native JSON support (no parsing overhead)
- TypeScript definitions for Schema.org types available
- Seamless integration with JSON Schema validation

#### 3. Bidirectional Validation Architecture

**Critical Requirement**: Perfect round-trip conversion (ROR -> Schema.org -> ROR)

**TypeScript Solution**: Runtime validation with type inference with `zod`

**Alternative Libraries**:
- `io-ts` - Functional approach with better error reporting
- `ajv` - JSON Schema validator (can generate TypeScript types)

#### 4. Development Experience

**IDE Support**:
- IntelliSense autocomplete for 64+ mapping rules
- Inline documentation for each field mapping
- Refactoring safety when updating mappings
- Error detection before running tests

#### 5. Deployment Flexibility

**Multi-Platform Support**:
- **Node.js**: CLI tool, server-side processing
- **Browser**: Web-based converter interface
- **npm Package**: Distributable library
- **Deno/Bun**: Modern runtime compatibility

**Build Options**:
- ESM modules for modern environments
- CommonJS for legacy Node.js
- UMD for browser direct usage
- TypeScript declarations for library consumers

#### 6. Testing Ecosystem

**Requirements**:
- Test all 64 mapping rules
- Validate bidirectional conversions
- Test edge cases (missing fields, multilingual data)

**Testing Libraries**:
- Vitest (fast, TypeScript-first)
- Jest (popular, mature ecosystem)
- Both support snapshot testing for JSON-LD output validation

---

### Alternative: Python

#### When Python Makes Sense

**Target Audience Consideration**:
- ROR community includes many researchers/academics
- Python is dominant in academic data processing
- Many institutional data pipelines use Python

#### Python Strengths

1. **Data Processing Libraries**:
   - `pandas` for bulk conversions
   - `rdflib` for RDF/semantic web operations
   - `pyld` for JSON-LD processing

2. **Type Safety** (Modern Python):

3. **Validation with Pydantic**:
   - Runtime validation with type hints
   - Automatic JSON Schema generation
   - Excellent error messages

#### Python Weaknesses for This Project

1. **JSON-LD Ecosystem**: Less mature than JavaScript/TypeScript
2. **Type System**: Structural typing less powerful than TypeScript
3. **Deployment**: Requires Python runtime (heavier than JS in browsers)
4. **Performance**: Slower for large-scale batch conversions


---


## Conclusion

**TypeScript is the recommended language** for this ROR -> Schema.org conversion library due to:

1. Superior type safety for complex nested structures
2. Excellent JSON-LD ecosystem
3. Robust bidirectional validation capabilities
4. Outstanding developer experience and tooling
5. Flexible deployment options (Node.js, browser, npm)

The library will follow enterprise-grade quality standards with comprehensive testing, type coverage, and documentation. Implementation will proceed in 5 phases over approximately 10 weeks, with clear success criteria at each stage.
