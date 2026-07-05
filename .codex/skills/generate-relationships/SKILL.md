---
name: generate-relationships
description: Generate realistic KYC relationship JSON between existing companies and individuals only, using schemas/relationships and referenced entity and primitive schemas as the source of truth.
---

# Generate KYC Relationships

Use this skill when asked to generate synthetic KYC relationships between previously generated companies and individuals.

The output must be valid JSON only. Do not include Markdown, comments, prose, or explanations in a successful response.

## Source of Truth

Before generating relationships, read:

- Every schema in `schemas/relationships/`
- Referenced schemas under `schemas/entities/`
- Referenced schemas under `schemas/primitives/`

Treat the schemas as authoritative. Do not invent fields outside schema `properties`. Use only enum values defined by the schemas. Populate every required field, and populate optional fields whenever reasonable.

If a requested relationship type has no schema in `schemas/relationships/`, do not invent a structure for it. Return the JSON error shape instead.

## Inputs

Expect one or more previously generated entities in the user input. The input JSON should contain companies and individuals.

Never modify input entities. Never create entities. Only create relationship objects that reference existing entity IDs.

Support requests such as:

- Generate relationships for this company.
- Generate relationships for all companies.
- Generate more authorised signatories 
- Generate more related parties.

## Output Shape

Return one JSON object with arrays for relationship types supported by the available schemas:

```json
{
  "shareholders": [],
  "directors": [],
  "beneficial_owners": [],
  "authorised_signatories": [],
  "related_parties": []
}
```

Objects in each array must conform exactly to their schemas.

## Entity Reference Rules

- Use only existing entity IDs from the input.
- Never create new companies / individuals
- Never invent IDs.
- Every referenced ID must exist in the input entities.
- Company relationship target fields must reference existing `company_id` values.
- Directors must reference existing individuals.
- Beneficial owners must reference existing individuals.
- Shareholders may reference existing companies or existing individuals when permitted by the schema.
- Authorised signatories must reference existing individuals.
- Related parties must reference existing companies or individuals.

## Business Rules

Generate realistic corporate ownership and governance structures suitable for KYC, AML, sanctions screening, and beneficial ownership testing.

Shareholders:

- Total direct shareholding for each company should equal exactly 100%, unless the user explicitly requests partial ownership data.
- Shareholding percentages must be realistic and within schema limits.
- Ownership type must use permitted enum values.
- Do not create shareholder records that conflict with the declared `shareholder_type`.

Directors:
- Every company should normally have at least one director.
- Directors must be existing individuals.
- Appointment dates, when present, must be valid ISO 8601 dates after the target company's incorporation date.
- Resignation dates, when present, must be after appointment dates.

Beneficial owners:
- Beneficial owners must be existing individuals.
- Ownership percentages must be internally consistent with the shareholder structure.
- Indirect ownership should only exist where intermediate ownership exists.
- Indirect beneficial ownership must be derivable from generated shareholder relationships.

Authorised signatories:
- Signatories should normally be directors or senior officers unless otherwise requested.
- Signatories must be existing individuals.

Related parties:
- Relationship types must come from the schema enum.
- Related parties must reference existing companies or individuals.

## Corporate Structure Rules

When multiple companies are provided, generate realistic corporate ownership structures where appropriate, such as:

- Holding companies
- Parent and subsidiary relationships
- Intermediate holding companies
- Cross-border ownership structures
- Indirect beneficial ownership through one or more corporate shareholders

Unless explicitly requested by the user:

- Do not generate circular ownership relationships.
- Do not create impossible ownership chains.
- Ensure indirect beneficial ownership can be derived from shareholder relationships.
- Keep ownership structures plausible for the jurisdictions and company types involved.

## Validation Checklist

Before returning successful JSON, verify:

- The JSON parses.
- All relationship objects conform to their schemas.
- All required fields exist and are non-empty.
- No fields outside schema `properties` are present.
- Referenced IDs exist exactly once in the input entities.
- No orphan references exist.
- Shareholding totals equal exactly 100% for each company, unless explicitly requested otherwise.
- Appointment and resignation dates are valid ISO 8601 dates and internally consistent.
- Ownership percentages are valid and within schema limits.
- Enum values use permitted schema values only.
- No duplicate relationships exist.
- The ownership graph contains no circular ownership unless explicitly requested.
- Indirect ownership relationships are derivable from generated shareholder relationships.

## Error Handling

If the request cannot be satisfied, return JSON only in this shape:

```json
{
  "error": {
    "message": "Cannot satisfy request while conforming to schema.",
    "constraint": "Describe the violated schema or business rule."
  }
}
```

Do not fabricate entities or relationships that violate the schemas.
