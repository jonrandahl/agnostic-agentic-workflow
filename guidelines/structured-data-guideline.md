# Structured data discovery

## When to assess

Assess machine-readable semantics during discovery for projects that publish
content or expose data to other systems. Reassess when introducing a domain
entity or content type, creating or materially changing a publishing template,
changing an external API representation or metadata integration, or auditing
semantics, search appearance, or social previews.

Start with the entity, its relationships, and the intended consumers. Record
one outcome per candidate and use case: **applicable**, **not applicable**, or
**deferred**, with the reason. For deferrals, record the missing evidence, data,
consumer need, or decision that would reopen the assessment.

## What makes a mapping suitable

- Identify the maintained vocabulary, relevant version or term status, and
  consumer requirements. Consider domain-specific vocabularies alongside
  Schema.org. Assess social metadata such as Open Graph for its own use case.
- Choose the most specific accurate type and check inherited properties,
  expected value types, cardinality where specified, and range or units.
  Keep the content page, the thing described, and related media separate when
  they represent distinct entities. Use stable identifiers and references
  where the format supports them.
- Map each proposed value to an authoritative project source. Record ownership,
  transformations, missing-value behaviour, and whether the value is suitable
  for disclosure. Omit unsupported claims. Check consistency with published
  content and reconcile overlapping output from templates and integrations.
- Prefer JSON-LD for web graphs when appropriate to the consumer and existing
  output. Preserve established API contracts and assess compatibility before
  proposing changes to a public representation.

## Review and validation

Use the [schema-discovery skill](../skills/schema-discovery/SKILL.md) for a
read-only assessment of the target project. Discovery covers inspection,
research, and recommendations. Keep source, dependencies, stored data, and
published output outside the discovery scope. Apply
[action authorisation](action-authorisation.md) and
[review-first](review-first-pattern.md) to any follow-on implementation or
external submission.

Record the proposed mapping, implementation owners, and validation criteria
for review. Store project-specific decisions and tests with the project when
authorised. For approved implementation, verify emitted output against the
[validation reference](../skills/schema-discovery/references/validation.md).
Report syntax, vocabulary fit, data accuracy, and consumer eligibility
independently. Record warnings, gaps, and checks that could not run.
