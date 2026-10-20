# Versionshantering, brancher och releaser

Den här instruktionen beskriver hur Ineras FHIR Implementation Guides versionshanteras, vilka brancher som används och hur en release publiceras. Den gäller IG:er skapade från [IneraFHIRTemplate](https://github.com/Inera-AB/IneraFHIRTemplate).

Se även:

- [Förbereda en IG för publicering och göra en release](forbereda-och-releasa-ig.md): förberedelser inför första publiceringen och checklista för varje release
- [Flytta en befintlig IG till publiceringsläget](flytta-befintlig-ig.md): för IG:er som publicerades med det tidigare upplägget

---

## FHIR har tre versionsdimensioner

| Dimension | Vad den styr | Hanteras av |
|---|---|---|
| FHIR-basversion (R4, R5) | Vilket paketekosystem IG:n lever i | Eget repo per basversion |
| IG-version (major.minor.patch) | Mognad och förändringarnas art | `version` i `sushi-config.yaml` och git-taggen |
| Profilversion (major.minor.patch) | En enskild profils mognad | `* ^version` i profilens FSH-definition |

Dimensionerna är oberoende. En patchrelease på IG-nivå (1.2.1) kan tekniskt innehålla en ny majorversion av en enskild profil. Här tillämpas konventionen för semver för tjänstekontrakt.

En profil utan egen `* ^version` får IG:ns version. Om profilerna ska versionshanteras separat måste varje profil ha en egen `^version`.

---

## Repostruktur och FHIR-versioner

En IG per FHIR-basversion, inte brancher. R4 och R5 ger skilda npm-paket med olika beroendegrafer. Om en migrering till R5 inleds öppnas ett nytt repo:

```
ig-formular-r4/    ← aktivt
ig-formular-r5/    ← nytt repo vid R5-migrering
```

Versionsnumreringen börjar om från 1.0.0 i det nya repot, och de två repona lever sedan oberoende av varandra.

Det nya repot behöver ett eget paket-id och en egen canonical. Tredje delen av paket-id:t blir IG:ns katalog på fhir.inera.se och måste vara unik, till exempel `inera.blankett.formular-r5` → `https://fhir.inera.se/ig/formular-r5`. Workflowsen använder terminologiservern för R4 (`tx.fhir.org/r4`) som standard. Ett R5-repo sätter `tx-server: https://tx.fhir.org/r5` med `with:` i anropen i `publish.yml` och `release.yml`.

---

## Brancher och deras syfte

```
main          ← senaste stabila release
develop       ← integrationsbranch, pågående arbete mot nästa minor/major
feature/*     ← kortlivad, skapas från develop, mergas tillbaka till develop
support/<M>.x ← underhåll av äldre major, lever tills versionen är EOL
```

Regler:

- `main` uppdateras bara genom merge från `develop` (ny minor/major) eller direkt patch-commit för kritiska redaktionella fel.
- Varje commit på `main` som representerar en release ska ha en versionstagg.
- Taggar sätts enbart på `main` eller `support/<M>.x`, aldrig på `develop` eller `feature/*`.
- `feature/*`-brancher tas bort efter merge. Namngivning: latinska tecken och bindestreck, inga mellanslag.

---

## Vad CI gör

Workflowsen i IG-repot är korta anrop till de delade workflowsen i [Inera-AB/fhir-ig-workflows](https://github.com/Inera-AB/fhir-ig-workflows). En rättning där når alla IG:er som anropar samma huvudversion (`@v1`).

| Händelse | Workflow | Publiceras till |
|---|---|---|
| Push till `main` | `publish.yml` | `https://inera-ab.github.io/<repo>/` |
| Push till `develop` | `publish.yml` | `https://inera-ab.github.io/<repo>/branches/develop/` |
| Push till `feature/<namn>` | `publish.yml` | `https://inera-ab.github.io/<repo>/branches/feature-<namn>/` |
| Push till `support/<M>.x` | `publish.yml` | `https://inera-ab.github.io/<repo>/branches/support-<M>-x/` |
| Releasetagg, t.ex. `1.2.0` | `release.yml` | `https://fhir.inera.se/ig/<code>/1.2.0/` |
| Manuell körning av *Publish release* på en branch | `release.yml` | Testwebbroten [inera-ab/fhir-portal-test](https://github.com/Inera-AB/fhir-portal-test) |

Branchbyggen publiceras bara till IG-repots egna GitHub Pages och aldrig till fhir.inera.se. I branchnamnet byts tecken som inte är bokstäver eller siffror, till exempel `/` och `.`, mot `-`. När en branch tas bort rensas dess katalog automatiskt (`cleanup.yml`).

En release publiceras med IG Publishers officiella publiceringsläge till den gemensamma webbroten på fhir.inera.se. `<code>` är tredje delen av paket-id:t: `inera.core.template` publiceras till `https://fhir.inera.se/ig/template/`.

En milestone-release kopieras dessutom till IG:ns rot, `https://fhir.inera.se/ig/<code>/`, som alltid visar den senaste milestone-releasen. Versionshistoriken finns på `https://fhir.inera.se/ig/<code>/history.html`.

En publicerad version ändras aldrig och publiceras aldrig om. Publishern uppdaterar bara bannern i tidigare versioner, så att de hänvisar till den nya versionen.

---

## Versionsformat

| Fas | Tagg | `mode` i `publication-request.json` | Exempel |
|---|---|---|---|
| Arbete på `develop` | ingen tagg (ci-build) | — | — |
| Release candidate | `x.y.z-rcN` | `working` | `2.0.0-rc1` |
| Stabil release | `x.y.z` | `milestone` | `2.0.0` |
| Patch på support-branch | `x.y.z` | `working` | `1.3.1` |

Taggar skrivs **utan `v`**. Taggen måste vara identisk med `version` i `sushi-config.yaml`, annars avbryter CI.

`working` publicerar versionen utan att ersätta IG:ns rot. Det används för en release candidate och för en patch på en äldre major. Annars skulle `https://fhir.inera.se/ig/<code>/` visa den äldre versionen.

---

## Release-flödet steg för steg

### Ny minor- eller major-release

```bash
git checkout develop && git pull
git checkout main && git pull
git merge develop
```

Gör sedan, på `main` via en PR:

1. Räkna upp versionen: ny `version` i `sushi-config.yaml` och en post i `version-history.md` som beskriver ändringarna.
2. Uppdatera `publication-request.json`: `desc` (en rad om releasen, ska skilja sig från tidigare releaser) och `"mode": "milestone"`.
3. Merga PR:en när CI och IG Publisher QA är gröna.

Tagga och pusha:

```bash
git fetch origin
git tag -a 2.1.0 -m "2.1.0" origin/main
git push origin 2.1.0
```

`release.yml` publicerar, uppdaterar versionshistoriken och IG-katalogen och sparar ändringen i FHIR:s IG-register som artefakt.

### Patch på äldre version (support-branch)

```bash
git checkout support/1.x
# Gör rättningen direkt eller via en feature-branch från support/1.x.
# Räkna upp version i sushi-config.yaml och sätt "mode": "working" och en ny
# desc i publication-request.json.
git tag -a 1.3.1 -m "1.3.1"
git push origin 1.3.1
```

Rättningen mergas inte upp till `develop` eller `main`.

---

## Registrering

- **IG-katalogen på fhir.inera.se** uppdateras automatiskt vid varje release. En milestone-release blir IG:ns senaste version i katalogen, en release med `working` blir det inte. Ingen manuell registrering behövs.
- **FHIR:s IG-register** ([FHIR/ig-registry](https://github.com/FHIR/ig-registry)): publishern lägger till IG:n, och vid senare releaser en ny utgåva, i sin kopia av `fhir-ig-list.json`. `release.yml` sparar ändringen som artefakt (`fhir-ig-list.diff`). Den skickas som PR till FHIR/ig-registry.
- **packages.fhir.org** hämtar paketen från Ineras paketflöde `https://fhir.inera.se/ig/package-feed.xml`. Flödet registreras en gång för hela Inera i `package-feeds.json` i FHIR/ig-registry. Ingenting behöver registreras per IG.
