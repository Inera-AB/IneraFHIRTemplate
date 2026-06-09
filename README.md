# Inera FHIR Implementation Guide Template

This repository is a template for Inera FHIR Implementation Guides.

## Structure

```
sushi-config.yaml           - SUSHI/IG configuration
ig.ini                      - IG Publisher configuration
input/
  fsh/
    aliases.fsh             - Shared FSH aliases
    profiles/               - FHIR profile definitions (.fsh)
    examples/               - Example instances (.fsh)
  pagecontent/              - Narrative IG pages (.md)
  images/                   - Images and diagrams
  includes/                 - Reusable HTML fragments (e.g. menu)
```

## Building

Install [SUSHI](https://fshschool.org/docs/sushi/installation/) and the [HL7 FHIR IG Publisher](https://confluence.hl7.org/display/FHIR/IG+Publisher+Documentation), then run:

```bash
sushi .
java -jar publisher.jar -ig ig.ini
```

The built IG will be in `output/`.

## Governance

This IG is governed by Inera AB. See the [Inera FHIR Landing Page](https://www.inera.se/fhir) for conformance requirements.
