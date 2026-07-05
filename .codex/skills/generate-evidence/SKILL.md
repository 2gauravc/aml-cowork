---
name: generate-evidence
description: Generate realistic fictional KYC evidence records for existing companies and individuals, using schemas/evidence plus referenced entity and primitive schemas as the source of truth.
---

# Generate KYC Evidence Records

Use this skill when asked to generate synthetic document evidence records for previously generated KYC entities. The evidence records will be used to create document evidence (i.e.g PDF generation) 

The output must be valid JSON only. Do not include Markdown, comments, prose, or explanations in a successful response.

## Source of Truth

Before generating evidence records, read:

- `schemas/evidence/company-registration-doc.json`
- `schemas/evidence/individual-identity-doc.json`
- Referenced schemas under `schemas/entities/`
- Referenced schemas under `schemas/primitives/`

Treat the schemas as authoritative. Do not invent fields outside schema `properties`. Use only enum values defined by the schemas. Populate every required field, and populate optional fields whenever reasonable.

## Core Rule

Generate evidence records only. Do not create or modify entities, relationships, or addresses.

Internal bank identifiers are for matching only:

- Company evidence uses `internal_reference.company_id` to match one existing company.
- Individual evidence uses `internal_reference.individual_id` to match one existing individual.
- Internal reference fields are not document-visible and must not be treated as content for generated document images or PDFs.

Document-visible identifiers belong in document fields:

- Company registration evidence must use `business_registration_number` as the document-visible company registration identifier.
- Identity evidence must use `document_number` as the document-visible identity document identifier.

## Inputs

Expect previously generated input data. The input JSON may contain:

- `companies`
- `individuals`
- `addresses`
- `relationships`

Never modify the input. Use only existing company and individual records. If an evidence record cannot be matched to exactly one existing entity, return the JSON error shape.

Support requests such as:

- Generate evidence records for all entities.
- Generate evidence records for companies.
- Generate evidence records for individuals.
- Generate evidence for this company and associated company/individuals.
- Write evidence to `output/evidence.json`.

## Output

Return or write one JSON object in this shape:

```json
{
  "company_registration_documents": [],
  "individual_identity_documents": []
}
```

When the user asks to write evidence to a file, write valid JSON to `output/evidence.json`.

Objects in `company_registration_documents` must conform exactly to `schemas/evidence/company-registration-doc.json`. Objects in `individual_identity_documents` must conform exactly to `schemas/evidence/individual-identity-doc.json`.

## Generation Rules

- Use only existing company and individual data from the input.
- Never create companies, individuals, addresses, or relationships.
- Never invent `company_id` or `individual_id`.
- Every internal reference ID must exist exactly once in the input.
- Generate realistic but entirely fictional document data.
- Never generate real document numbers or real business registration numbers.
- Ensure document data matches the referenced entity data.
- Use ISO 3166-1 alpha-2 country codes.
- Use valid ISO 8601 dates.
- Use only schema enum values.

## Company Registration Evidence

Generate company registration evidence only for existing companies.

For each company registration document:

- `internal_reference.company_id` must match exactly one input company `company_id`.
- `internal_reference` is for matching only and is not document-visible.
- `business_registration_number` must match the referenced company `business_registration_number`.
- `business_registration_number` is document-visible.
- `legal_name` must match the referenced company.
- `legal_form`, when populated, must match the referenced company.
- `incorporation_date` must match the referenced company.
- `incorporation_country` must match the referenced company.
- `company_status` should match the referenced company `status` when available.
- `registered_address` must conform to `schemas/entities/address.json`.
- If the company stores `registered_address` as an address ID, resolve that ID from the input `addresses` array and place the full address object in the evidence record because the evidence schema requires an address object.
- `issuing_country` should normally match `incorporation_country`.
- `issue_date` must be on or after `incorporation_date`.
- `document_type` must come from the schema enum.
- `document_number` must be fictional, plausible, and distinct from internal bank IDs.

## Individual Identity Evidence

Generate individual identity evidence only for existing individuals.

For each identity document:

- `internal_reference.individual_id` must match exactly one input individual `individual_id`.
- `internal_reference` is for matching only and is not document-visible.
- `holder_name` must match the referenced individual.
- `holder_date_of_birth`, when populated, must match the referenced individual.
- `holder_nationality`, when populated, must match the referenced individual unless the user explicitly requests a plausible exception.
- `issuing_country` should normally match nationality unless another country is plausible.
- `issue_date` must be after date of birth when date of birth is available.
- `expiry_date`, when present, must be after `issue_date`.
- `document_type` must come from the schema enum.
- `document_number` must be fictional, plausible, and distinct from internal bank IDs.
- Do not generate identity documents for minors unless explicitly requested and allowed by the input data.

## Validation Checklist

Before returning or writing successful JSON, verify:

- The JSON parses.
- All evidence records conform to their schemas.
- All required fields are present and non-empty.
- No fields outside schema `properties` are present.
- Every `internal_reference.company_id` resolves to exactly one input company.
- Every `internal_reference.individual_id` resolves to exactly one input individual.
- Internal reference fields are used only for matching and are not document-visible.
- Document-visible fields do not contain internal bank IDs.
- Company evidence uses `business_registration_number` as the company registration identifier.
- Document data matches the referenced entity data.
- Resolved addresses exist exactly once and conform to `schemas/entities/address.json`.
- Country codes are ISO 3166-1 alpha-2.
- Dates are valid ISO 8601 calendar dates.
- Issue and expiry dates are logically consistent.
- Enum values use permitted schema values only.
- No duplicate evidence records exist.

## Error Handling

If the request cannot be satisfied, return JSON only in this shape:

```json
{
  "error": {
    "message": "Cannot satisfy request while conforming to schema.",
    "constraint": "Describe the violated schema, entity reference, or business rule."
  }
}
```

Do not fabricate invalid evidence. Do not create missing entities.
