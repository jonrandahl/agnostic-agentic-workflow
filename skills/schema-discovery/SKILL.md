---
name: schema-discovery
description: >
  Discover applicable machine-readable vocabularies and propose evidence-backed
  mappings when planning or auditing published content, domain entities,
  templates, or external API representations.
---

# Schema discovery

Apply the [structured data guideline](../../guidelines/structured-data-guideline.md).
Inspect the target project read-only and deliver an assessment for review.

1. **Inventory.** Trace representative output back to its templates,
   serializers, metadata sources, and integrations. Identify entities,
   relationships, audiences, and consumers. Record existing markup or payload
   fields, including overlapping emitters and generated output.
2. **Research.** Check the current authoritative vocabulary and consumer
   documentation. Compare plausible types, including more specific subtypes
   and domain vocabularies. Record sources and the reason for each applicability
   decision.
3. **Map.** For each candidate entity, sketch its identifier and relationships.
   For each proposed property, record the source field and owner, observed
   format, target property and expected value, transformation or fallback,
   and supporting evidence or unresolved gap.
4. **Hand off.** Give the reviewer the baseline, candidate decisions, mapping,
   proposed change locations and their authoritative source, and validation
   criteria. Use the [validation reference](references/validation.md) to check
   available evidence and identify tests for approved implementation. Report
   checks run separately from planned or blocked checks. Stop at the assessment.
