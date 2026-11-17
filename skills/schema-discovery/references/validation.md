# Structured data validation

## Authoritative references

- [Schema.org vocabulary](https://schema.org/docs/full.html) for type
  hierarchy, property definitions, and expected values. Check the individual
  term's status and the vocabulary release applicable to the project.
- [W3C JSON-LD 1.1](https://www.w3.org/TR/json-ld11/) for contexts, identifiers,
  references, and graph serialisation.
- [Open Graph protocol](https://ogp.me/) for social metadata and its fields.
- [Google Search structured data](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data)
  and the relevant feature documentation for Google's eligibility rules.
- For another vocabulary or consumer, use its maintained specification and
  conformance or integration documentation.

## Checks

1. **Extraction and syntax.** Inspect actual rendered pages or API responses.
   Inspect both raw and rendered HTML where client code emits metadata. Parse
   every relevant block or payload and check contexts, identifiers, and value
   encoding. Check representative populated and sparse records and any
   materially different template, response, or rendering path.
2. **Vocabulary and graph.** Check types, properties, and values against the
   selected vocabulary. Use the
   [Schema Markup Validator](https://validator.schema.org/) for Schema.org
   where disclosure is permitted. Check canonical URLs and graph references,
   including conflicting claims about the same entity across emitters.
3. **Evidence.** Compare output against source data and published claims.
   Check missing fields, units, links, and normalisation. For implementation
   changes, use project tests against emitted output to protect the mapping
   and ensure that missing values produce the intended omissions.
4. **Consumer support.** Check the targeted consumer's required and recommended
   fields and delivery constraints. For Google features, use the
   [Rich Results Test](https://search.google.com/test/rich-results) where
   applicable. Vocabulary validity and consumer eligibility are independent
   findings. Eligibility does not guarantee display.

Use local parsing and specification checks for private material. Follow
[action authorisation](../../../guidelines/action-authorisation.md) before
sharing a URL or payload externally. Sanitised fixtures can verify syntax and
mappings, with live delivery or crawler checks reported as pending where needed.

Record the sample, environment, date, tool or specification, and result for
each check. Distinguish failures from warnings, inapplicable checks, and checks
that could not run.
