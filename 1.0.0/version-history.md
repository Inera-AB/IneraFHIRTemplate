# Version History - Inera FHIR Implementation Guide Template v1.0.0

* [**Table of Contents**](toc.md)
* **Version History**

## Version History

# Version History

This page is maintained automatically by [release-please](https://github.com/googleapis/release-please). Entries are generated from commit messages that follow [Conventional Commits](https://www.conventionalcommits.org/).

-------

## [1.0.1] — 2026-06-24

### Bug Fixes

* Read IG metadata from build output instead of sushi-config.yaml
* Fix `is_release` flag in fhir-portal dispatch to use tag ref check
* Align dispatch payload fields with fhir-portal registry workflow

-------

## [1.0.0] — 2026-06-24

### Features

* Multi-branch CI: `develop`, `feature/**` and `support/<M>.x` branches each build and deploy to their own subdirectory
* fhir-portal integration: optionally push built IG to `inera-ab/fhir-portal` and notify index rebuild via `repository_dispatch`
* QA report posted as PR comment by `pr-qa.yml`
* `cleanup.yml` removes build directories for deleted branches
* `release-please.yml` for automated version bumping and changelog (manual trigger)
* WCAG 2.1 AA accessibility fixes in `inera.css`

-------

## [0.1.0] — 2026-06-16

### Features

* Initial IG template structure with Inera branding
* `IneraPatient` example profile with personnummer identifier slice
* Navigation pages: Workflow, Logical Models, Mappings, Examples, Terminology, Downloads
* GitHub Actions for SUSHI validation, full IG Publisher build, and GitHub Pages deployment
* ig-registry integration via `package-list.json`
* WCAG 2.1 AA-compliant CSS

