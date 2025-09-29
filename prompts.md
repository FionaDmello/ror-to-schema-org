# Prompts to Claude
## ROR.external_id
#### prompt 1
given the mappings @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:20-46, I want to find mappings for @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:48-51 that is valid  both semantically and type wise. The mapping should be bidirectionally sound. Do not auto generate information that is not provided and expected from ROR. Use @context/ror/ror_schema_v2_1.json and @context/schemaorg-current-https.jsonld for context and reference. Break down the problem into small steps and explain your reasoning at each step in clear, simple and concise fashion. Ask clarifying questions when required

#### prompt 2
1. Scope Concern: I am looking for the best possible mapping for the lines that focuses on minimizing data loss and maintaining data integrity as top priority
2. Implementation Priority: Keep things as simple as possible with the top priority in mind. A balance should be maintained in the complications of implementation and structural integrity
3. Coverage gap: I do not understand what you mean by there being mappings already. Did you mean @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:20-46?
4. Concern: I understand your reasoning. But ROR also has a ROR.id field mapped to schema.identifier. The approach you suggest creates multiple identifiers with no distinction for the original identifier that is the original identifier and the other external identifiers. How do we navigate this? Explain your analysis in small steps and give practical examples where possible.

#### prompt 3
converting all identifiers into URLs is modifying data that ROR provides. I am not okay with that. We should work with data as it comes from ROR. Secondly, how do we infer the ROR.external_ids.preferred property when we map back from schema.org to ROR in this case? Also how do we handle the reverse mapping when @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:46?

#### prompt 4
```
{
  "@type": "Organization",
  "identifier": [
    "https://ror.org/012abc123",
    {
      "@type": "PropertyValue",
      "propertyID": "wikidata",
      "value": ["Q123456"],
      "valueReference":{
        "@type": "PropertyValue",
        "additionalType": "preferred",
        "value": "Q123456"
      }
    },
    {
      "@type": "PropertyValue",
      "propertyID": "grid",
      "value": ["grid.12345.67", "grid.old.78"],
      "valueReference":{
        "@type": "PropertyValue",
        "additionalType": "preferred",
        "value": "grid.12345.67"
      }
    }
  ]
}
```
would this be valid both semantically and type wise? Would it allow bidirectional mapping?

#### prompt 5
Great. I have one small problem. The identifier array, contains one string (for the ROR ID) and the rest are objects. This looks like an array of mixed types. Can something be done about this? I do not want to change the approach we discussed for external_ids. So maintain this solution but tell me how I could simplistically adapt the ROR ID.

#### prompt 6
Great update the mappings for ROR.id and ROR.external_ids and related properties @mappings/tsv/ror_to_schema_human_in_loop.tsv and @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv.

#### prompt 7
Now create a folder each for id and external_ids at @docs. In each create a analysis.md and implementation_example.md. Use @docs/template_analysis.md and @docs/template_implementation_example.md as reference and create documentation giving reasoning for the mapping for ROR.id @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:30 and ROR.external_ids @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:31-34 respectively. Do not autogenerate content that ROR does not provide. Provide some information on bidirectional mapping in the example files for each property. Also, mention how the two properties are linked and why they have been mapped into the same property in schema.org, and how to distinguish them apart when reverse mapping. Keep things simple, clear, concise and practical


## ROR.locations

#### prompt 1
given the mappings @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:20-50, I want to find the best mappings for the locations key in ROR, as mentioned in @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:53-64. The mapping needs to be valid both semantically and data type wise. The mapping should be bidirectionally sound. Do not auto generate information that is not provided and expected from ROR. Use @context/ror/ror_schema_v2_1.json and @context/schemaorg-current-https.jsonld for context and reference. Break down the problem into small steps and explain your reasoning at each step in clear, simple and concise fashion. Ask clarifying questions when required

  ### prompt 2
  how about
  {
   location: {
     type: "Place",
     identifier: 5378538,
     name: "Oakland",
     latitude: 37.8044,
     longitude: -122.2711
     containedInPlace: {
       @type: "State",
       name: "California",
       identifier: "CA",
       address: {
         type: "PostalAddress",
         addressCountry: {
           @type: "Country",
           name: "United States"
           identifier: "US",
           containedInPlace: {
             @type: "Continent",
             name: "North America",
             identifier: "NA"
           }
         }
       },
     },
   }
  } ? evaluate the following mapping in detail. Does this cover everything ROR could potentially provide? What are the limitations of this mapping?

  ### prompt 3
  what is the reasoning behind 'Issue: PostalAddress.addressCountry expects Text or Country, but you cannot embed a full Country object with containedInPlace within a PostalAddress. This violates
    Schema.org's expected data structure.'? Where is the proof. Explain in steps in clear concise fashion

### prompt 4
Give me a clear report of how well this mapping works and how to best implement it

### prompt 5
Now create a folder each for location  at @docs. Create a analysis.md and implementation_example.md. Use @docs/template_analysis.md and @docs/template_implementation_example.md as reference and create documentation giving reasoning for the mapping for ROR.location @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:51-62. Do not autogenerate content that ROR does not provide. Provide some information on bidirectional mapping in the example files for each property. Make particular practical note of implementation complexity and how to handle it. Consider the potential for nested properties and how to handle them efficiently.


## ROR.relationships

### prompt 1
given the mappings @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:20-62, I want to find the best mappings for the relationships key in ROR, as mentioned in @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:64-72. The mapping needs to be valid both semantically and data type wise. The mapping should be bidirectionally sound. Do not auto generate information that is not provided and expected from ROR. Use @context/ror/ror_schema_v2_1.json and @context/schemaorg-current-https.jsonld for context and reference. Break down the problem into small steps and explain your reasoning at each step in clear, simple and concise fashion. Ask clarifying questions when required

### prompt 2 
Can you use https://ror.org/0281dp749 as an example and tell me how the mapping you recommend would look like?

### prompt 3
what was the full recommended mapping?

### prompt 4
go ahead and proceed to update the mappings @mappings/tsv/ror_to_schema_human_in_loop.tsv  and @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv for the relationships key, with the recommended mapping

### prompt 5
Now create a folder each for relationships at @docs. Create a analysis.md and implementation_example.md. Use @docs/template_analysis.md and @docs/template_implementation_example.md as reference and create documentation giving reasoning for the mapping for ROR.relationships @mappings/sssom/ror_to_schema_human_in_loop.sssom.tsv:64-72. Do not autogenerate content that ROR does not provide. Provide some information on bidirectional mapping in the example files for each property. Make particular practical note of implementation complexity and how to handle it. Consider the potential for nested properties and how to handle them efficiently.

### prompt 6
update @CLAUDE.md and @README.md with information on changes made today as well. Do not modify existing content, just append related information to it with todays changes.
