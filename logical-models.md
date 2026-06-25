# Logiska modeller - Inera FHIR Implementation Guide Template v2.0.0

* [**Table of Contents**](toc.md)
* **Logiska modeller**

## Logiska modeller

# Logiska modeller

Denna sida beskriver de logiska informationsmodeller som ligger till grund för profilerna i denna Implementation Guide. Logiska modeller fångar domänens informationskrav oberoende av teknisk representation.

-------

### Syfte med logiska modeller

**Förklara varför logiska modeller ingår i denna IG. Exempel: "De logiska modellerna nedan representerar de informationskrav som överenskommits med kliniska och domänexperter. De utgör den auktoritativa definitionen av vilka data som måste utbytas och bildar underlag för de FHIR-profiler som definieras i denna IG."**

-------

### Modeller

**Lista varje logisk modell som definieras i denna IG. Ge för varje modell en kort beskrivning och länk till den genererade StructureDefinition-sidan.**

| | |
| :--- | :--- |
| **[[Modellnamn]](StructureDefinition-[Modellnamn].md)** | **Beskriv vilken information denna modell fångar och i vilket sammanhang den används.** |

-------

### Modellbeskrivningar

**Beskriv för varje logisk modell dess nyckelelement, kardinaliteter och eventuella viktiga begränsningar eller affärsregler som inte är självklara utifrån elementnamnen.**

#### [Modellens namn]

**Beskriv modellen. Lista de viktigaste elementen och förklara designbeslut, till exempel varför vissa element är obligatoriska eller varför specifika datatyper valts.**

-------

> **Vägledning för författare:** Definiera logiska modeller som FHIR `StructureDefinition`-resurser med `kind = logical` i FSH. Placera dem i `input/fsh/logicalmodels/`. Logiska modeller ska återspegla överenskomna kliniska eller domänkrav och granskas av ämnesexperter innan motsvarande FHIR-profiler skapas.

