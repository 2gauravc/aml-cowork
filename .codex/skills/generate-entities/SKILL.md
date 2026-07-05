---
name: generate-entities
description: Generate realistic, entirely fictional KYC synthetic companies, individuals, and addresses as JSON that conforms exactly to the repository's schemas/entities and schemas/primitives JSON schemas.
---

# Generate KYC Entities

Use this skill when asked to generate synthetic KYC entities: companies, individuals, or addresses.

The output must be valid JSON only. Do not include Markdown, comments, prose, or explanations in a successful response.

## Source of Truth

Before generating data, read the relevant schemas from the current repository:

- `schemas/entities/company.json`
- `schemas/entities/individual.json`
- `schemas/entities/address.json`
- Relevant referenced primitives under `schemas/primitives/`

Treat the schemas as authoritative. Do not invent fields outside `properties`. Use only enum values defined by the schemas. Populate every field listed in `required`, and populate optional fields whenever reasonable.

If the user requests a value or identifier format that conflicts with the schemas, follow the schema. If the request cannot be satisfied while conforming to schema, return a JSON error object describing the schema constraint that cannot be met.

## Supported Inputs

Honor user constraints when they are compatible with the schemas, including:

- Number of companies, individuals, and addresses
- Country or region
- Industry or business activity
- Incorporation year or date range
- Legal form
- Status
- Director or owner demographics if the requested output includes individuals

If no counts are specified, generate one company, one individual, and the addresses needed by those entities.

## Generation Rules

- Generate realistic but entirely fictional data.
- Never generate real people, real companies, or known real-world registration numbers.
- Use internally consistent dates, countries, addresses, legal forms, status values, and paid-up capital amounts.
- Individuals who could act as directors or owners must be adults. Date of birth must make them at least 18 years old as of the current date.
- Incorporation dates must be valid ISO 8601 dates and plausible for the requested company profile.
- Country values must be ISO 3166-1 alpha-2 country codes.
- Legal forms and status values must come from the corresponding schema enum.
- IDs must be unique within their entity type and must not conflict across generated objects.
- Do not leave required fields blank, null, or placeholder-only.
- Do not create duplicate companies or duplicate individuals.

## ID Rules

Read the ID patterns from the schemas each time and generate IDs that match those patterns.

For the current schemas, this means:

- Company IDs match `company_unique_id`, such as `CMP001`.
- Individual IDs match `individual_id`, such as `IND001`.
- Address IDs match `address_id`, such as `ADD001`.

Do not use longer IDs such as `CMP000001`, `IND000001`, or `ADDR000001` unless the schemas have been changed to allow them.

## Output Shape

Return a single JSON object containing separate arrays:

```json
{
  "companies": [],
  "individuals": [],
  "addresses": []
}
```

Objects in each array must conform exactly to their corresponding schema. Do not embed related objects inside other objects; refer to related objects by ID.

When a field refers to another object by ID, the referenced ID must exist exactly once in the corresponding top-level array. Do not add any fields that are not defined by the schemas.

## Validation Checklist

Before returning successful JSON, verify:

- The JSON parses.
- Each object conforms to its entity schema and referenced primitive schemas.
- All required fields exist and are non-empty.
- No fields outside the schema `properties` are present.
- IDs are unique across all generated objects and match schema patterns.
- Every ID reference resolves to exactly one object in the corresponding top-level array.
- Countries are ISO 3166-1 alpha-2 codes.
- Dates are valid ISO 8601 calendar dates.
- Enum fields use permitted values only.
- Companies are not duplicates by legal name, incorporation country, and incorporation date.
- Individuals are not duplicates by full name, date of birth, and nationality.

## Error Handling

If the request cannot be satisfied, return JSON only in this shape:

```json
{
  "error": {
    "message": "Cannot satisfy request while conforming to schema.",
    "constraint": "Describe the specific schema constraint or incompatible user instruction."
  }
}
```

Do not fabricate invalid data to satisfy an incompatible request.
