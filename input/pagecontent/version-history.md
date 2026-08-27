# Version History

This page is maintained automatically by [release-please](https://github.com/googleapis/release-please).
Entries are generated from commit messages that follow [Conventional Commits](https://www.conventionalcommits.org/).

---

## [2.2.1] — 2026-08-27

### Bug Fixes

- Reservindexet som byggs när IG Publisher inte genererar `history.html`
  skrevs bara till `index.html`. Publishern stämplar in en länk till
  `<canonical>/history.html` på varje sida, så den länken gav 404 medan
  `<canonical>/` fungerade. Filen skrivs nu under båda namnen

---

## [2.2.0] — 2026-08-27

Version 2.1.0 sattes i `sushi-config.yaml` men släpptes aldrig — den fick
ingen tagg och ingen changelog-post. Ändringarna därifrån ingår i den här
releasen.

### Features

- Byte till `fhir2.base.template`, HL7:s språkmedvetna bastemplate. Sidorna
  genereras under `output/sv/` med en omdirigering i roten, och template-
  texterna följer språkinställningen
- Omstrukturerad meny: Hem, Inledning, Funktionellt, Implementering, Om och
  Artefakter, med nya sidor för användningsfall, roller och ansvar,
  REST-interaktioner, förväntade svar, felhantering, säkerhet och testning
- Vägledande kommentarer för profilering inbäddade i exempelprofilen
- QA-skriptet utbrutet till `.github/scripts/qa-comment.js` med en testsvit
  som körs vid varje push och pull request

### Bug Fixes

- `i18n-default-lang` ersätter det felstavade `i18n-default-language`, som
  publishern ignorerade tyst. Det var orsaken till att ett tidigare försök
  med fhir2 lade alla sidor under `/en` trots att svenska var konfigurerat
- QA-kommentaren rapporterade noll fel och noll varningar för bygget som
  hade sex varningar. Siffrorna hämtas nu från publisherns egen summering i
  `build.log` och detaljerna från `output/qa.txt`; en trasig eller saknad
  källa redovisas öppet i stället för att visas som ett rent bygge
- `history.html` söks även i språkkatalogen, så releasesteget använder
  publisherns riktiga versionslista i stället för reservindexet
- Nedladdningslänken pekar på canonical och paketet kopieras till IG-roten
  vid release
- Platshållaren `<namn>` i `index.md` tolkades som ett HTML-element

---

## [2.0.0] — 2026-06-25

### Features

- Svenska sidtexter och menyrubriker i alla mallsidor
- Återgång till `fhir.base.template` (eliminerar `/en`-routing och aktiv bugg i fhir2.base.template)
- `i18n-default-language: sv` konfigurerat

---

## [1.0.1] — 2026-06-24

### Bug Fixes

- Read IG metadata from build output instead of sushi-config.yaml
- Fix `is_release` flag in fhir-portal dispatch to use tag ref check
- Align dispatch payload fields with fhir-portal registry workflow

---

## [1.0.0] — 2026-06-24

### Features

- Multi-branch CI: `develop`, `feature/**` and `support/<M>.x` branches each build and deploy to their own subdirectory
- fhir-portal integration: optionally push built IG to `inera-ab/fhir-portal` and notify index rebuild via `repository_dispatch`
- QA report posted as PR comment by `pr-qa.yml`
- `cleanup.yml` removes build directories for deleted branches
- `release-please.yml` for automated version bumping and changelog (manual trigger)
- WCAG 2.1 AA accessibility fixes in `inera.css`

---

## [0.1.0] — 2026-06-16

### Features

- Initial IG template structure with Inera branding
- `IneraPatient` example profile with personnummer identifier slice
- Navigation pages: Workflow, Logical Models, Mappings, Examples, Terminology, Downloads
- GitHub Actions for SUSHI validation, full IG Publisher build, and GitHub Pages deployment
- ig-registry integration via `package-list.json`
- WCAG 2.1 AA-compliant CSS
