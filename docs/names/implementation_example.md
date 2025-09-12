# ROR Names to Schema.org Mapping Analysis

## Problem Statement

The ROR schema v2.1 `names` property (lines 22-25 in human-in-loop mapping) uses an overly complex ItemList structure that doesn't align well with Schema.org naming patterns or semantic intent for organizational names.

## ROR Names Property Structure

**Source:** `/json/ror/ror_schema_v2_1.json:251-293`

```json
"names": {
    "type": "array",
    "uniqueItems": true,
    "minItems": 1,
    "items": {
        "type": "object",
        "properties": {
            "value": { "type": "string" },
            "types": {
                "type": "array",
                "items": {
                    "enum": ["acronym", "alias", "label", "ror_display"]
                }
            },
            "lang": { 
                "type": "string",
                "pattern": "^[a-z]{2}$" 
            }
        },
        "required": ["types", "value"]
    }
}
```

**Semantic structure:**
- **Array of name objects**: Multiple name variants per organization
- **Name types**: `acronym`, `alias`, `label`, `ror_display` - each with specific semantic meaning
- **Multilingual support**: Optional ISO 639-1 language codes
- **Required fields**: Both `value` (the actual name) and `types` (categorization)

## Current Mapping Issues

### **Line 22**: `ROR:names → schema:ItemList` ❌
- **Type mismatch**: ROR names are structured name objects, not simple list items
- **Semantic misalignment**: Organizations don't have "lists of names" - they have primary names with variants
- **Overengineering**: ItemList introduces unnecessary complexity for name variants
- **Confidence**: 0.85 (artificially high for poor semantic fit)

### **Line 23**: `ROR:names.value → schema:ItemList.itemListElement.name` ❌  
- **Deep nesting**: Creates complex JSON-LD structure: `ItemList → itemListElement → name`
- **Pattern violation**: Not how Schema.org typically handles organizational naming
- **Implementation burden**: Requires complex nested structures for simple name variants

### **Line 24**: `ROR:names.lang → schema:ItemList.itemListElement.inLanguage` 🔄
- **Context mismatch**: `inLanguage` typically describes content language, not name language tags
- **Limited utility**: Most consumers expect simple name strings, not language-tagged names
- **Semantic drift**: Language of name vs. language of content are different concepts

### **Line 25**: `ROR:names.types → schema:ItemList.itemListElement.additionalProperty` ❌
- **Loss of semantics**: Name types have specific meaning (acronym vs. alias vs. legal name) that gets buried
- **Generic fallback**: `additionalProperty` used as catch-all instead of proper semantic mapping
- **Reduced discoverability**: Type information becomes harder to query and process

## Schema.org Name Properties Analysis

### Available Properties:

1. **`name`** (Thing)
   - **Range**: Text
   - **Purpose**: Primary name of the organization
   - **Usage**: Single canonical name

2. **`alternateName`** (Thing)  
   - **Range**: Text (can be array)
   - **Purpose**: Aliases, alternative names, common variations
   - **Usage**: Multiple names supported, well-recognized pattern

3. **`legalName`** (Organization)
   - **Range**: Text
   - **Purpose**: Official registered company name
   - **Usage**: Single official legal name

4. **`slogan`** (Organization)
   - **Range**: Text  
   - **Purpose**: Marketing slogan or motto
   - **Usage**: Not applicable to ROR naming

## Recommended Mapping Solutions

### **Option 1: Content-Primary Mapping (Recommended - Updated)**

Map based on actual content vs. display metadata hierarchy:

```tsv
ROR:names.value	Name Value String	skos:exactMatch	schema:name	name	Semantic equivalence - contains actual name text content	https://orcid.org/0000-0002-0465-1009	0.95	All name values are primary text strings for organizational names
ROR:names.ror_display	Display Name	skos:relatedMatch	schema:name	name	Related mapping - canonical when different from primary value	https://orcid.org/0000-0002-0465-1009	0.88	Conditional: schema:name when preferred display, schema:alternateName otherwise  
ROR:names[types=label]	Label Name	skos:exactMatch	schema:legalName	legalName	Semantic equivalence for official organizational label	https://orcid.org/0000-0002-0465-1009	0.90	label typically represents official/legal name designation
ROR:names[types=alias]	Alias Name	skos:exactMatch	schema:alternateName	alternateName	Direct correspondence for organizational aliases	https://orcid.org/0000-0002-0465-1009	0.95	alias directly maps to alternate name concept
ROR:names[types=acronym]	Acronym Name	skos:exactMatch	schema:alternateName	alternateName	Acronyms are alternate name forms	https://orcid.org/0000-0002-0465-1009	0.92	acronyms function as alternate names in practice
```

**Advantages:**
- **Semantic correctness**: Content drives the mapping, not display preferences
- **Type compatibility**: All map to Text type  
- **Conditional logic**: Handles cases where display differs from content
- **Implementation flexibility**: Can adapt to different ROR record structures
- **Future-proof**: Robust against changes in display conventions

**Implementation example:**
```json
{
  "@type": "Organization",
  "name": "Massachusetts Institute of Technology",
  "alternateName": ["MIT", "Mass. Institute of Technology"], 
  "legalName": "Massachusetts Institute of Technology"
}
```

### **Option 2: Consolidated alternateName Approach**

Map all ROR names to alternateName array, with primary name extraction:

```tsv
ROR:names	Organization Names Array	skos:closeMatch	schema:alternateName	alternateName	All name variants as alternate names	https://orcid.org/0000-0002-0465-1009	0.90	Consolidated approach treating all names as variants
ROR:names.value	Name Value	skos:exactMatch	schema:alternateName	alternateName	Direct string value mapping	https://orcid.org/0000-0002-0465-1009	0.95	Name values map directly to alternate name strings
ROR:names.lang	Name Language	skos:relatedMatch	schema:additionalProperty	additionalProperty	Language metadata as structured property	https://orcid.org/0000-0002-0465-1009	0.75	Language tags preserved as metadata
ROR:names.types	Name Types	skos:relatedMatch	schema:additionalProperty	additionalProperty	Name type categorization as metadata	https://orcid.org/0000-0002-0465-1009	0.80	Type information preserved for processing systems
```

**Advantages:**
- **Simplicity**: Single mapping strategy for all names
- **Google support**: Well-supported alternateName pattern
- **Metadata preservation**: Type and language info available via additionalProperty

### **Option 3: Hybrid Primary + Alternate Approach**

Extract primary name to `name`, others to `alternateName`:

```tsv
ROR:names[primary]	Primary Organization Name	skos:exactMatch	schema:name	name	Primary name extraction from names array	https://orcid.org/0000-0002-0465-1009	0.92	Extract ror_display or first name as primary
ROR:names[others]	Other Organization Names	skos:exactMatch	schema:alternateName	alternateName	All non-primary names as alternates	https://orcid.org/0000-0002-0465-1009	0.88	Remaining names as alternate forms
```

## Language Handling

For multilingual names, the recommended approaches:

1. **HTML lang attributes** (when in HTML context):
   ```html
   <div itemProp="name" lang="en">Massachusetts Institute of Technology</div>
   <div itemProp="alternateName" lang="fr">Institut de Technologie du Massachusetts</div>
   ```

2. **StructuredValue approach** (Schema.org compliant):
   ```json
   {
     "additionalProperty": [
       {
         "@type": "PropertyValue",
         "name": "ror-multilingual-name",
         "value": {
           "@type": "StructuredValue",
           "name": "マサチューセッツ工科大学",
           "inLanguage": {
             "@type": "Language",
             "alternateName": "ja"
           },
           "additionalType": "ror:label"
         }
       }
     ]
   }
   ```

## Impact Assessment

### Benefits of Recommended Changes:

1. **Improved semantic alignment**: Name types map to appropriate properties
2. **Reduced complexity**: Eliminate unnecessary ItemList structure  
3. **Better search optimization**: Follow established Schema.org patterns
4. **Enhanced usability**: Simpler JSON-LD structures for consumers
5. **Maintained semantics**: Preserve ROR type distinctions where valuable

### Migration Considerations:

1. **Breaking change**: Existing processors expecting ItemList structure need updates
2. **Type information**: May need additionalProperty for systems requiring ROR type metadata
3. **Language tags**: Consider if multilingual support is critical for target applications

## Conclusion

**Recommendation: Option 1 (Content-Primary Mapping) - Updated**

This approach provides the best balance of semantic accuracy, implementation simplicity, and Schema.org compliance. By making `names.value` the primary mapping to `schema:name`, it correctly prioritizes actual content over display metadata.

Key benefits of the final approach:
- **Semantic correctness**: Content drives mappings, not presentation choices
- **Conditional flexibility**: Handles complex display logic appropriately 
- **Future-proof**: Robust against changes in ROR display conventions
- **Schema.org alignment**: Follows established organizational naming patterns

The refined understanding eliminates the original ItemList complexity while correctly establishing the hierarchy between content (`names.value`) and display metadata (`ror_display`).

---

## Refined Analysis: Value vs. Display Hierarchy (2024-09-10 Update)

### **Critical Insight: Semantic Precedence Problem**

During human-in-loop review, a fundamental issue was identified with the original recommended mappings:

**Original Problem:**
- `ROR:names.ror_display` → `schema:name` (treating display choice as primary)
- `ROR:names.value` → `schema:Text` (treating actual content as just a data type)

**Semantic Logic Error:**
This mapping inverted the proper hierarchy. The `value` field contains the actual name content, while `ror_display` is metadata indicating which name should be displayed prominently.

### **Corrected Understanding**

#### **Primary Content vs. Display Metadata**
- **`names.value`**: Contains the actual string content of names - this is the semantic core
- **`ror_display`**: Indicates curatorial choice about which name to display - this is presentation metadata
- **Relationship**: Content should map to `schema:name`, display choice should be conditional

#### **Updated Mapping Logic (Implemented)**

```tsv
ROR:names.value	Name Value String	skos:exactMatch	schema:name	name	Semantic equivalence - contains the actual name text content	https://orcid.org/0000-0002-0465-1009	0.95	All name values are the primary text strings for organizational names

ROR:names.ror_display	Primary Display Name	skos:relatedMatch	schema:name	name	Related mapping - canonical display name when different from primary value	https://orcid.org/0000-0002-0465-1009	0.88	ror_display is schema:name when it represents the preferred display form; schema:alternateName otherwise
```

### **Why This Mapping is Semantically Superior**

1. **Content Precedence**: `names.value` contains the actual text, making it the natural choice for `schema:name`

2. **Conditional Display Logic**: `ror_display` can map to either:
   - `schema:name` when it represents the preferred canonical name
   - `schema:alternateName` when it differs from the primary content

3. **Reduced Confidence for Complexity**: `ror_display` confidence reduced to 0.88 and uses `skos:relatedMatch` to acknowledge its conditional nature

4. **Maintains Semantic Integrity**: The actual content drives the mapping, with display preferences as secondary considerations

### **Implementation Impact**

This approach provides:
- **Logical consistency**: Content maps to content properties
- **Flexibility**: Handles cases where display choice differs from canonical content
- **Semantic accuracy**: Preserves the distinction between content and presentation metadata
- **Real-world alignment**: Reflects how name data is actually structured and used

### **Final Recommendation**

The corrected mapping better represents the semantic relationship between ROR's structured names and Schema.org's organizational naming properties. This approach should be adopted over the previous ItemList-based recommendations.

---
*Original analysis conducted: 2024-09-10*  
*Refined analysis updated: 2024-09-10*  
*Target mapping lines: mappings/ror_v2-1_schema_org_human_in_loop.sssom.tsv:22-27*