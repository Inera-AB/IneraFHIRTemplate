# Inera FHIR Implementation Guide Template

This repository is a GitHub template for Inera FHIR Implementation Guides. It includes the standard IG structure, author guidance pages, an example FSH profile, and pre-configured GitHub Actions for validation and publishing.

---

## Creating a new IG from this template

### 1. Create a new repository

Go to this repository on GitHub and click **"Use this template"** → **"Create a new repository"**.

- Choose the `inera-ab` organisation (or your own) as owner.
- Give the repository a name that reflects your IG, e.g. `inera-fhir-laboratory`.
- Set visibility to **Private** until the IG is ready for publication.
- Click **"Create repository"**.

> All files — including GitHub Actions workflows — are copied into the new repository.

### 2. Enable GitHub Pages

In the new repository, go to **Settings → Pages**.

- Under **Source**, select **"GitHub Actions"**.
- Save.

The IG will be published automatically at `https://inera-ab.github.io/<repo-name>/` the first time a push lands on `main`.

### 3. Configure the IG

Edit the following files to reflect your IG:

| File | What to update |
|------|---------------|
| `sushi-config.yaml` | `id`, `canonical`, `name`, `title`, `description`, `version` |
| `input/pagecontent/index.md` | Domain, Scope, Purpose, and the governance section |
| `input/pagecontent/version-history.md` | Initial version and date |

### 4. Replace template content

- Replace placeholder profiles in `input/fsh/profiles/` with your own.
- Replace or extend examples in `input/fsh/examples/`.
- Fill in the narrative pages in `input/pagecontent/`.

### 5. Develop on a branch

Create a feature branch for your work:

```bash
git checkout -b feature/my-first-profile
```

Every push to a non-main branch, and every pull request, triggers the **SUSHI validation** workflow. Fix any errors before merging.

### 6. Publish

Merge your branch to `main`. The **Build and publish IG** workflow runs automatically:

1. SUSHI compiles FSH to FHIR JSON.
2. The HL7 FHIR IG Publisher builds the full HTML IG.
3. The output is deployed to GitHub Pages.

---

## CI workflows

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `validate.yml` | Push to any branch except `main`; pull requests | Runs SUSHI and uploads the generated artefacts |
| `publish.yml` | Push to `main`; manual trigger | Runs SUSHI + IG Publisher and deploys to GitHub Pages |

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
  publish.yml           Build and deploy IG on push to main
sushi-config.yaml       IG metadata, dependencies, pages and menu
ig.ini                  IG Publisher configuration
input/
  fsh/
    aliases.fsh         Shared FSH aliases
    profiles/           FHIR profile definitions (.fsh)
    examples/           Example instances (.fsh)
  pagecontent/          Narrative IG pages (.md)
  images/               Images and diagrams
  includes/menu.xml     Navigation menu
```

---

## Governance

This template is maintained by Inera AB. See the [Inera FHIR Landing Page](https://www.inera.se/fhir) for conformance requirements that apply to all Inera FHIR IGs.
