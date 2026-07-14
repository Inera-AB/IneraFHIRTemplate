# Mappningar

Denna sida dokumenterar hur element i informationsunderlaget (se [Introduktion](introduction.html)) mappas till element i motsvarande FHIR-profiler. Om denna IG definierar egna logiska modeller i FSH (`input/fsh/logicalmodels/`), länka till respektive genererade StructureDefinition-sida i mappningstabellerna nedan.

---

### Syfte

*Förklara rollen för dessa mappningar. Exempel: "Mappningarna nedan spårar varje element i de logiska modellerna till det FHIR-profilelement som bär den informationen. Detta gör det möjligt att verifiera täckning och vägleder implementatörer som behöver förstå sambandet mellan kliniska krav och deras tekniska representation."*

---

### Mappningstabeller

*Tillhandahåll en mappningstabell per logisk modell. Varje rad ska visa ett logiskt modellelement, dess kardinalitet och motsvarande FHIR-profilsökväg.*

#### [Logisk modells namn] → [Profilnamn]

| Logiskt modellelement | Kard. | FHIR-profilelement | Noteringar |
|-----------------------|-------|--------------------|------------|
| *[Elementnamn]* | *1..1* | *[Profil.element](StructureDefinition-[Profil].html)* | *Noteringar om mappningen, t.ex. värdemängdsbegränsningar eller transformationsregler.* |
| *[Elementnamn]* | *0..\** | *[Profil.element](StructureDefinition-[Profil].html)* | |

---

### Omappade element

*Om logiska modellelement inte täcks av ett profilelement, lista dem här och förklara varför (utanför scope, uppskjutet till framtida version, täcks av separat profil, etc.).*

| Logiskt modellelement | Orsak till att det inte mappas |
|-----------------------|-------------------------------|
| *[Elementnamn]* | *Förklaring.* |

---

> **Vägledning för författare:** Håll mappningarna uppdaterade i takt med att profilerna utvecklas. Mappningar kan också uttryckas formellt med FHIR `StructureMap`-resurser om maskinbearbetningsbar transformation behövs. Placera StructureMaps i `input/fsh/maps/`.
