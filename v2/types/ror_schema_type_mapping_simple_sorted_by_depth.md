# ROR to Schema.org Type Mapping (Sorted by Depth)

| ROR Key | ROR Value Type | Schema.org Key | Schema.org Value Type |
|---------|----------------|----------------|----------------------|
| `id` | URI | `schema:identifier` | Text |
| `names` | array of objects | `schema:name` | Text |
| `types` | array of text | `schema:additionalType` | Text |
| `established` | integer (year) | `schema:foundingDate` | Date |
| `status` | text | `schema:dissolutionDate` | Date |
| `locations` | array of objects | `schema:address` | PostalAddress |
| `domains` | array of text | `schema:url` | URL |
| `links` | array of objects | `schema:url` | URL |
| `external_ids` | array of objects | `schema:sameAs` | URL |
| `relationships` | array of objects | `schema:parentOrganization` | Organization |
| `admin` | object | `schema:CreativeWork` | Class |
| `*` (ROR Record) | object | `schema:ResearchOrganization` | Class |
| `*` (ROR Record) | object | `schema:Organization` | Class |
| `names.value` | text | `schema:name` | Text |
| `names.lang` | text (ISO 639-3) | `schema:inLanguage` | Text |
| `names.types` | array of text | `schema:additionalProperty` | PropertyValue |
| `types.funder` | text | `schema:FundingAgency` | Class |
| `types.government` | text | `schema:GovernmentOrganization` | Class |
| `types.education` | text | `schema:EducationalOrganization` | Class |
| `types.company` | text | `schema:Corporation` | Class |
| `types.archive` | text | `schema:ArchiveOrganization` | Class |
| `types.healthcare` | text | `schema:MedicalOrganization` | Class |
| `types.nonprofit` | text | `schema:NGO` | Class |
| `types.facility` | text | `schema:ResearchOrganization` | Class |
| `types.other` | text | `schema:Organization` | Class |
| `locations.geonames_id` | integer | `schema:geoMidpoint` | GeoCoordinates |
| `links.value` | URI | `schema:url` | URL |
| `links.type` | text | `schema:additionalProperty` | PropertyValue |
| `external_ids.all` | array of text | `schema:sameAs` | URL |
| `external_ids.preferred` | text | `schema:sameAs` | URL |
| `external_ids.type` | text | `schema:additionalProperty` | PropertyValue |
| `relationships.id` | URI | `schema:parentOrganization` | Organization |
| `relationships.label` | text | `schema:name` | Text |
| `relationships.type` | text | `schema:parentOrganization` | Organization |
| `relationships.type` | text | `schema:subOrganization` | Organization |
| `relationships.type` | text | `schema:memberOf` | Organization |
| `relationships.type` | text | `schema:affiliation` | Organization |
| `admin.created` | object | `schema:dateCreated` | Date |
| `admin.last_modified` | object | `schema:dateModified` | Date |
| `locations.geonames_details.lat` | number (float) | `schema:latitude` | Number |
| `locations.geonames_details.lng` | number (float) | `schema:longitude` | Number |
| `locations.geonames_details.country_code` | text (ISO 3166-1) | `schema:addressCountry` | Text |
| `locations.geonames_details.country_name` | text | `schema:addressCountry` | Text |
| `locations.geonames_details.name` | text | `schema:addressLocality` | Text |
| `admin.created.date` | text (ISO 8601) | `schema:dateCreated` | Date |
| `admin.created.schema_version` | text | `schema:version` | Text |
| `admin.last_modified.date` | text (ISO 8601) | `schema:dateModified` | Date |
| `admin.last_modified.schema_version` | text | `schema:version` | Text |
