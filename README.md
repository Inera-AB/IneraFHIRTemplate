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

## How publishing works

Every Inera IG has three kinds of build:

| Build | Trigger | Published to |
|---|---|---|
| Branch build | Push to `main`, `develop`, `feature/**`, `support/**` (`publish.yml`) | The IG repo's own GitHub Pages: `https://inera-ab.github.io/<repo>/` (`main`), `…/branches/<branch>/` (other branches) |
| Release | Push of a release tag such as `1.2.0` (`release.yml`) | `https://fhir.inera.se/ig/<code>/<version>/`, with the latest milestone also at `https://fhir.inera.se/ig/<code>/` |
| Test publication | Manual run of `release.yml` on a branch | The test web root [inera-ab/fhir-portal-test](https://github.com/Inera-AB/fhir-portal-test), never fhir.inera.se |

Releases use IG Publisher's publication mode (`-go-publish`). The publisher publishes into a web root shared by all Inera IGs, the `ig/` folder of the `gh-pages` branch of [inera-ab/fhir-portal](https://github.com/Inera-AB/fhir-portal), and takes care of:

- the version folder and the copy of the latest milestone at the IG root
- the release banner on every page, including earlier versions ("The current version which supersedes this version is …")
- the version history: `package-list.json` and `history.html` at `https://fhir.inera.se/ig/<code>/history.html`
- the package and publication feeds (`package-feed.xml`, `publication-feed.xml`), through which packages reach packages.fhir.org
- the IG's entry in the FHIR IG registry (`fhir-ig-list.json`), saved as a workflow artifact for a PR to [FHIR/ig-registry](https://github.com/FHIR/ig-registry)

`<code>` is the third part of the package id: `inera.core.template` publishes to `https://fhir.inera.se/ig/template`. The layout rule in the web root's `publish-setup.json` only matches package ids with exactly three parts, and `<code>` must be unique among Inera IGs.

The Swedish instructions in [`docs/`](docs/) describe the process in full:

- [Versionshantering, brancher och releaser](docs/versionshantering-och-release.md)
- [Förbereda en IG för publicering och göra en release](docs/forbereda-och-releasa-ig.md)
- [Flytta en befintlig IG till publiceringsläget](docs/flytta-befintlig-ig.md)

---

## Creating a new IG from this template

### 1. Create a new repository

Go to this repository on GitHub and click **"Use this template"** → **"Create a new repository"**.

- Choose the `inera-ab` organisation as owner.
- Give the repository a name that reflects your IG, e.g. `inera-laboratory`.
- Click **"Create repository"**.

All files — including GitHub Actions workflows — are copied into the new repository.

### 2. Configure the IG identity

Edit these files before doing anything else. The values here drive the build output, the published URLs and the registry entries.

**`sushi-config.yaml`**

| Field | Example | Notes |
|-------|---------|-------|
| `id` | `inera.lab.laboratory` | npm package id, exactly three parts: `inera.<domain>.<code>`. `<code>` becomes the folder on fhir.inera.se and must be unique |
| `canonical` | `https://fhir.inera.se/ig/laboratory` | Must be `https://fhir.inera.se/ig/<code>`. Never change it after the first release |
| `name` | `IneraLaboratory` | PascalCase, no spaces |
| `title` | `"Inera Laboratory IG"` | Shown in the IG header and in the IG catalog |
| `description` | `"FHIR-profiler för laboratoriesvar."` | Shown in the IG catalog |
| `version` | `0.1.0` | The version the next release will get |

**`ig.ini`** — set `ig = fsh-generated/resources/ImplementationGuide-<id>.json` with the same `id`.

**`publication-request.json`** — fill in for the first release (see [Publish a release](#publish-a-release)). Set `"first": true` and replace the template's values for `title`, `introduction`, `ci-build` (`https://inera-ab.github.io/<repo>`), `registry-description` and `desc`.

**`input/pagecontent/version-history.md`** — remove the template's entries. Keep the first line, `{::options header_offset="1" /}` (see [Version history](#version-history)).

**`.release-please-manifest.json`** — set to the starting version.

**`input/pagecontent/downloads.md`** — point the package link to `https://fhir.inera.se/ig/<code>/package.tgz`, and update the matching line in `input/ignoreWarnings.txt`.

There is no `package-list.json` in the IG repo. The publisher keeps it in the web root and refuses to publish from a source folder that has one.

### 3. Configure the repository

- **GitHub Pages** (for branch builds): Settings → Pages → *Deploy from a branch* → `gh-pages` / `(root)`. The branch is created by the first push to `main`.
- **Workflow permissions:** Settings → Actions → General → *Read and write permissions*. Tick *Allow GitHub Actions to create and approve pull requests* if you use release-please.
- **`FHIR_PORTAL_TOKEN`:** Settings → Secrets and variables → Actions. A fine-grained PAT with **Contents: Read and write** on `inera-ab/fhir-portal` and `inera-ab/fhir-portal-test`. Releases need it to push to the web root and to update the IG catalog. Note the token's expiry date.

### 4. Develop and validate

Create a feature branch for your work:

```bash
git checkout -b feature/my-first-profile
```

Every push to a non-main branch, and every pull request, triggers the **SUSHI validation** workflow. Full IG Publisher QA runs on pull requests and posts results as a PR comment. Fix all errors before merging.

Replace placeholder content:

- Profiles in `input/fsh/profiles/`. Give each profile its own `* ^version` if profiles are versioned separately; otherwise they get the IG version.
- Examples in `input/fsh/examples/`
- Narrative pages in `input/pagecontent/`

---

## Publish a release

### Before tagging

On the branch that is released (`main`, or `support/<major>.x` for a patch to an older major version), via a pull request:

1. **Version:** set `version` in `sushi-config.yaml` and `.release-please-manifest.json`, and add an entry to `input/pagecontent/version-history.md`. release-please can do this for you: Actions → *Release Please* → *Run workflow* opens a pull request with all three.
2. **Publication request:** update `publication-request.json`:
   - `desc`: one line describing this release. It must differ from every earlier release.
   - `mode`: `milestone` for a release that becomes the current version at the IG root; `working` for a publication that does not replace the root, such as a release candidate or a patch to an older major version. (`technical-correction` also needs a `tech-correction.template.html` in the web root's templates, which is not set up yet.)
   - `status` and `sequence`: for example `release` / `Releases`.
   - `first`: `true` only for the IG's very first publication.

   `package-id`, `version` and `path` are filled in by the workflow from `sushi-config.yaml`.
3. **Dependencies:** pin them to published versions; the publisher refuses `current` dependencies.
4. **Test publication (optional):** Actions → *Publish release* → *Run workflow* on the branch publishes to the test web root.

### Tag

Release tags have no `v` prefix and must equal `version` in `sushi-config.yaml`:

```bash
git fetch origin
git tag -a 1.2.0 -m "1.2.0" origin/main
git push origin 1.2.0
```

`release.yml` then:

1. Checks that the tag, `sushi-config.yaml` and `publication-request.json` agree, and that the version and its description are new.
2. Builds the IG.
3. Checks out the web root (only this IG's folder and the shared files).
4. Runs `-go-publish`, which rebuilds the IG for publication and updates the web root.
5. Pushes the web root and updates the IG catalog on fhir.inera.se.
6. Saves the FHIR IG registry change (`fhir-ig-list.diff`) and the publication log as an artifact.

### After the release

- Check `https://fhir.inera.se/ig/<code>/<version>/`, `https://fhir.inera.se/ig/<code>/history.html` and the IG catalog at `https://fhir.inera.se/ig/`.
- Open a pull request to [FHIR/ig-registry](https://github.com/FHIR/ig-registry) with the change in `fhir-ig-list.diff`.

### Rules

- **Never publish the same version twice.** Package caches and packages.fhir.org may already hold it, and the feeds get duplicates. Publish a new patch version instead. The workflow refuses a version that is already in the web root.
- A failed run that never reached *Push web root* has changed nothing and can be re-run.

---

## Version history

`input/pagecontent/version-history.md` is the IG's change log. The publisher's `history.html` lists the published versions; the change log describes what changed in each.

release-please writes entries as `##` (version) and `###` (sections). The first line of `version-history.md`, `{::options header_offset="1" /}`, makes kramdown render them one level down (`<h3>`/`<h4>`), below the `<h2>` page title the template adds. Keep that line; without it IG Publisher reports a second `<h2>` on the page as an error.

---

## CI workflows

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `validate.yml` | Push to non-main branches; pull requests | Runs SUSHI and the tests of the CI helper scripts |
| `pr-qa.yml` | Pull request (opened, synchronised) | Full IG Publisher build (no tx server), posts QA report as PR comment |
| `publish.yml` | Push to `main`, `develop`, `feature/**`, `support/**`; manual trigger | Branch build, deployed to `gh-pages` of this repo under the branch's directory; updates the IG catalog entry |
| `release.yml` | Push of a release tag (`1.2.0`, `1.2.0-rc1`); manual trigger | Publishes the release with `-go-publish`: a tag to fhir.inera.se, a manual run on a branch to the test web root |
| `cleanup.yml` | Branch deleted | Removes the branch's build directory from `gh-pages` |
| `release-please.yml` | Manual trigger only | Opens a PR bumping `version` in `sushi-config.yaml` and adding a changelog entry to `version-history.md`. Creates no tag or GitHub release |

Repository variables `IG_WEB_REPO` and `IG_TEST_WEB_REPO` override the web roots (`inera-ab/fhir-portal`, `inera-ab/fhir-portal-test`).

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
  validate.yml             Validate FSH on push / PR
  pr-qa.yml                IG Publisher QA on pull requests
  publish.yml              Branch builds to this repo's GitHub Pages
  release.yml              Releases with -go-publish to the web root
  cleanup.yml              Remove build dir for deleted branches
  release-please.yml       Bump version and generate changelog (manual)
docs/                      Swedish instructions for versioning and publishing
publication-request.json   What the next release is (desc, mode, status, sequence)
sushi-config.yaml          IG metadata, dependencies, pages and menu
ig.ini                     IG Publisher configuration
input/
  fsh/
    aliases.fsh            Shared FSH aliases
    profiles/              FHIR profile definitions (.fsh)
    examples/              Example instances (.fsh)
  pagecontent/             Narrative IG pages (.md); no leading "# Title", the template renders the page title
  images/                  Images, CSS and diagrams
  includes/                Fragment overrides (CSS, header) and menu.xml (only the <ul>; the IG template supplies the surrounding navbar markup)
```

---

## Governance

This template is maintained by Inera AB. See the [Inera FHIR Landing Page](https://www.inera.se/fhir) for conformance requirements that apply to all Inera FHIR IGs.
