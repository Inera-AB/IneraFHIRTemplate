# Inera FHIR Implementation Guide Template

[![Build and publish IG](https://github.com/Inera-AB/IneraFHIRTemplate/actions/workflows/publish.yml/badge.svg)](https://github.com/Inera-AB/IneraFHIRTemplate/actions/workflows/publish.yml)
[![Validate FSH](https://github.com/Inera-AB/IneraFHIRTemplate/actions/workflows/validate.yml/badge.svg)](https://github.com/Inera-AB/IneraFHIRTemplate/actions/workflows/validate.yml)
[![FHIR R4](https://img.shields.io/badge/FHIR-R4-blue)](https://hl7.org/fhir/R4/)
[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

This repository is a GitHub template for Inera FHIR Implementation Guides. It includes the standard IG structure, author guidance pages, an example FSH profile, and pre-configured GitHub Actions for validation and publishing.

---

## Design choices

Three deliberate choices shape how this template's narrative pages are organised:

- **Menu structure inspired by European specifications.** The top menu (Home / Inledning / Funktionellt / Implementering / Om / Artefakter — Home / Introduction / Functional / Implementation / About / Artifacts) mirrors the pattern used by specifications such as FHIR ePS, rather than the ad-hoc Background/Artifacts grouping used in earlier versions of this template. Funktionellt and Implementering are expandable dropdowns grouping several narrower topic pages; this gives implementers a familiar entry point across Inera and other European FHIR IGs, and separates background material (Inledning), business requirements (Funktionellt), and what implementers actually build against (Implementering) into distinct top-level sections.
- **Artifacts is a single, auto-generated page, not hand-maintained catalog pages.** Profiles, terminology (code systems/value sets) and examples are not given their own narrative pages — IG Publisher already lists them automatically on the generated `artifacts.html` page, so the "Artefakter" menu entry links straight there. Maintaining separate `profiles.md`/`terminology.md`/`examples.md` pages would just duplicate that listing and require manual upkeep as profiles change.
- **The information model is published externally, separate from the IG.** Concept models, information models and detailed business workflows are not restated in this template — they live in an authoritative, independently published information specification that the IG references (see `information-basis.md`'s "Informationsunderlag" page, under Funktionellt, and `mappings.md`). This avoids duplicate documentation that can drift out of sync between the information specification and the IG over time.

---

## Inera branding

The template uses Inera's visual identity by default. All branding is isolated to four files:

| File | What it does |
|------|-------------|
| `input/includes/fragment-css.html` | Loads Inera CSS and favicon |
| `input/includes/fragment-header.html` | Inera logo in the header |
| `input/images/inera.css` | All Inera colours, fonts and layout |
| `input/images/inera-*.svg / inera-*.png` | Logos and favicon |

Remove these four files (and the `input/images/inera-*` assets) to revert to standard HL7 FHIR styling.

---

## Creating a new IG from this template

### 1. Create a new repository

Go to this repository on GitHub and click **"Use this template"** → **"Create a new repository"**.

- Choose the `inera-ab` organisation as owner.
- Give the repository a name that reflects your IG, e.g. `inera-laboratory`.
- Set visibility to **Private** until the IG is ready for publication.
- Click **"Create repository"**.

All files — including GitHub Actions workflows — are copied into the new repository.

### 2. Configure the IG identity

Edit these files before doing anything else. The values here drive everything — build output, registry entries, and published URLs.

**`sushi-config.yaml`**

| Field | Example | Notes |
|-------|---------|-------|
| `id` | `inera.laboratory` | npm package id, dots allowed |
| `canonical` | `https://fhir.inera.se/ig/inera-laboratory` | Must match `package-list.json` exactly |
| `name` | `IneraLaboratory` | PascalCase, no spaces |
| `title` | `"Inera Laboratory IG"` | Human-readable, shown in IG header |
| `description` | `"FHIR-profiler för laboratoriesvar."` | Shown in fhir-portal index card |
| `version` | `0.1.0` | Must match tag when releasing |

**`package-list.json`**

Update `package-id`, `title`, `canonical`, and the `current` ci-build entry:

```json
{
  "package-id": "inera.laboratory",
  "title": "Inera Laboratory IG",
  "canonical": "https://fhir.inera.se/ig/inera-laboratory",
  "list": [
    {
      "version": "current",
      "desc": "CI build — latest commit on main",
      "path": "https://inera-ab.github.io/inera-laboratory",
      "status": "ci-build",
      "current": true
    }
  ]
}
```

> **Important:** The `canonical` field in `package-list.json` is the base URL that CI uses when adding versioned entries. It must be identical to `canonical` in `sushi-config.yaml`. IG Publisher also uses it to generate `history.html`.

### 3. Enable GitHub Pages

In the new repository, go to **Settings → Pages**.

- Under **Source**, select **"Deploy from a branch"**.
- Set branch to **`gh-pages`** / `/ (root)`.
- Save.

The `gh-pages` branch is created automatically on the first push to `main`. Until fhir-portal is configured (step 5), builds are published here:

| Branch / tag | Published at |
|---|---|
| `main` | `https://inera-ab.github.io/<repo>/` (root) |
| `develop` | `.../branches/develop/` |
| `feature/my-profile` | `.../branches/feature-my-profile/` |
| `v1.2.0` (tag) | `.../1.2.0/` |

### 4. Develop and validate

Create a feature branch for your work:

```bash
git checkout -b feature/my-first-profile
```

Every push to a non-main branch, and every pull request, triggers the **SUSHI validation** workflow. Full IG Publisher QA runs on pull requests and posts results as a PR comment. Fix all errors before merging.

Replace placeholder content:

- Profiles in `input/fsh/profiles/`
- Examples in `input/fsh/examples/`
- Narrative pages in `input/pagecontent/`

### 5. Connect to fhir-portal (optional)

All Inera IGs are aggregated at `fhir.inera.se/ig/` via the shared hub repo `inera-ab/fhir-portal`. Connecting is a one-time setup per IG repo.

**a) Add the secret**

In the IG repo: Settings → Secrets and variables → Actions → New repository secret:

| Secret | Value |
|--------|-------|
| `FHIR_PORTAL_TOKEN` | Fine-grained PAT with `Contents: Read and write` on `inera-ab/fhir-portal` |

Once set, `publish.yml` automatically:
- Deploys each tagged release to `ig/<repo-name>/<version>/` on fhir-portal
- Copies `history.html` and `package-list.json` to `ig/<repo-name>/` so the version history page works
- Fires a `repository_dispatch` event to fhir-portal, which regenerates `fhir.inera.se/ig/index.html` with an updated entry for your IG

The dispatch payload includes `ig_title` and `ig_description` read directly from the build output — no manual variables needed.

Without `FHIR_PORTAL_TOKEN` these steps are silently skipped; the IG still deploys to the repo's own GitHub Pages.

**b) Register in ig-registry**

Add an entry to `fhir-ig-list.json` in `Inera-AB/ig-registry`:

```json
{
  "package-id": "inera.laboratory",
  "title": "Inera Laboratory IG",
  "canonical": "https://fhir.inera.se/ig/inera-laboratory",
  "category": "National Base",
  "language": "sv",
  "country": "SE",
  "package-list": "https://raw.githubusercontent.com/Inera-AB/inera-laboratory/main/package-list.json"
}
```

This is done once. CI keeps `package-list.json` up to date automatically from then on.

For registration in the global HL7 registry (tools.fhir.org): submit a PR to `FHIR/ig-registry` with the same format.

### 6. Publish a versioned release

When the IG is ready for a numbered release:

**a)** Run release-please: Actions → "Release Please" → Run workflow.
It opens a PR that bumps `version` in `sushi-config.yaml` and adds a changelog entry to `version-history.md`. Review and merge the PR.

**b)** Merge to `main`, then tag:

```bash
git checkout main && git pull
git tag v1.0.0
git push origin v1.0.0
```

`publish.yml` detects the tag and:

1. Validates that the tag matches `version` in `sushi-config.yaml` — aborts if they differ.
2. Builds the IG with SUSHI + IG Publisher.
3. Reads `title`, `description`, and `version` from the build output (`package.manifest.json` and `ImplementationGuide-*.json`).
4. Appends a versioned entry to `package-list.json` using the `canonical` field as the base URL, commits it back to `main`.
5. Deploys the full IG output to `<version>/` on `gh-pages`.
6. Copies `history.html` and `package-list.json` to the IG root on `gh-pages` so `history.html` resolves at the canonical URL.
7. **If `FHIR_PORTAL_TOKEN` is set:** deploys to `ig/<repo-name>/<version>/` on fhir-portal, copies `history.html` to `ig/<repo-name>/`, and fires `repository_dispatch` so fhir-portal rebuilds `ig/index.html`.

---

## CI workflows

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `validate.yml` | Push to non-main branches; pull requests | Runs SUSHI, uploads generated artefacts |
| `pr-qa.yml` | Pull request (opened, synchronised) | Full IG Publisher build (no tx server), posts QA report as PR comment |
| `publish.yml` | Push to `main`, `develop`, `feature/**`, `support/**`, or `v*` tag; manual trigger | Full build + deploys to `gh-pages` under correct subdirectory; on tags: validates version, updates `package-list.json`, deploys to fhir-portal if token is set |
| `cleanup.yml` | Branch deleted | Removes the branch's build directory from `gh-pages` |
| `release-please.yml` | Manual trigger only | Opens a PR bumping `version` in `sushi-config.yaml` and adding a changelog entry to `version-history.md` |

---

## Local development

Install [SUSHI](https://fshschool.org/docs/sushi/installation/) and Java 17+, then:

```bash
# Compile FSH
sushi .

# Download the IG Publisher (first time)
mkdir -p input-cache
curl -sL https://github.com/HL7/fhir-ig-publisher/releases/latest/download/publisher.jar \
  -o input-cache/publisher.jar

# Build the IG
java -jar input-cache/publisher.jar -ig ig.ini

# Open the result
open output/index.html
```

---

## Repository structure

```
.github/workflows/
  validate.yml          Validate FSH on push / PR
  pr-qa.yml             IG Publisher QA on pull requests
  publish.yml           Build, deploy, and notify fhir-portal on push / tag
  cleanup.yml           Remove build dir for deleted branches
  release-please.yml    Bump version and generate changelog (manual)
package-list.json       IG version history — source of truth for canonical URLs
sushi-config.yaml       IG metadata, dependencies, pages and menu
ig.ini                  IG Publisher configuration
input/
  fsh/
    aliases.fsh         Shared FSH aliases
    profiles/           FHIR profile definitions (.fsh)
    examples/           Example instances (.fsh)
  pagecontent/          Narrative IG pages (.md)
  images/               Images, CSS and diagrams
  includes/             Fragment overrides (CSS, header) and menu.xml
```

---

## Governance

This template is maintained by Inera AB. See the [Inera FHIR Landing Page](https://www.inera.se/fhir) for conformance requirements that apply to all Inera FHIR IGs.
