# Mappningar - Inera FHIR Implementation Guide Template v2.0.0

* [**Table of Contents**](toc.md)
* **Mappningar**

## Mappningar

# Mappningar

Denna sida dokumenterar hur element i de logiska modellerna i denna IG mappas till element i motsvarande FHIR-profiler.

-------

### Syfte

**Förklara rollen för dessa mappningar. Exempel: "Mappningarna nedan spårar varje element i de logiska modellerna till det FHIR-profilelement som bär den informationen. Detta gör det möjligt att verifiera täckning och vägleder implementatörer som behöver förstå sambandet mellan kliniska krav och deras tekniska representation."**

-------

### Mappningstabeller

**Tillhandahåll en mappningstabell per logisk modell. Varje rad ska visa ett logiskt modellelement, dess kardinalitet och motsvarande FHIR-profilsökväg.**

#### [Logisk modells namn] → [Profilnamn]

| | | | |
| :--- | :--- | :--- | :--- |
| **[Elementnamn]** | **1..1** | **[Profil.element](StructureDefinition-[Profil].md)** | **Noteringar om mappningen, t.ex. värdemängdsbegränsningar eller transformationsregler.** |
| **[Elementnamn]** | **0..*** | **[Profil.element](StructureDefinition-[Profil].md)** |   |

-------

### Omappade element

**Om logiska modellelement inte täcks av ett profilelement, lista dem här och förklara varför (utanför scope, uppskjutet till framtida version, täcks av separat profil, etc.).**

| | |
| :--- | :--- |
| **[Elementnamn]** | **Förklaring.** |

-------

> **Vägledning för författare:** Håll mappningarna uppdaterade i takt med att profilerna utvecklas. Mappningar kan också uttryckas formellt med FHIR `StructureMap`-resurser om maskinbearbetningsbar transformation behövs. Placera StructureMaps i `input/fsh/maps/`.

