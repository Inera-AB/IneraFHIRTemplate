# Introduktion

### [Domännamn]

*Beskriv det kliniska eller administrativa område som denna IG hanterar. Förklara varför området är viktigt inom svensk hälso- och sjukvård, vilken roll det spelar i patientvård eller informationsutbyte, och varför det prioriterats för FHIR-standardisering. Exempel: kliniska laboratoriesvar, remisser, vårdplaner, recept.*

---

### Omfattning

*Definiera vad denna IG täcker och vad den inte täcker. Specificera vilka typer av användningsfall, kliniska scenarier eller informationsutbyten som ingår. Nämn eventuella explicita avgränsningar. Om denna IG är avsedd att användas som bas för mer specialiserade guider, säg det här. Exempel: "Denna IG gäller för X inom ramen för Y. Den täcker inte Z. Härledda guider kan begränsa eller utöka omfattningen vid behov."*

---

### Syfte

*Beskriv målet med denna IG. Förklara vilket problem den löser, vem den riktar sig till (implementatörer, leverantörer, vårdgivare) och hur den passar in i det bredare Inera- och e-hälsolandskapet. Hänvisa till nationella eller regionala program, standarder eller mandat som motiverat arbetet.*

---

### Terminologi

På [Inera Terminologitjänst](https://www.inera.se/tjanster/alla-tjanster-a-o/terminologitjanst-for-nationell-e-halsa/) finns alla refererade kodsystem och värdemängder som utvecklats av Inera.

---

### Beroenden

Denna IG har ett beroende till SE-core, definierat av HL7 Sweden. Detta återspeglas i de profiler som ärver från SECore-profiler.

---

### Dokumentation

Mer information om Inera Core finns [här](https://www.inera.se/tjanster/alla-tjanster-a-o/inera-core/). Inera Core är en del av RIVTA – referensarkitekturen för svensk hälso- och sjukvård.

---

### Styrning och vägledning

FHIR-profiler förvaltas av Inera: [Källkod](https://github.com/inera-ab).

En beskrivning av krav på konformans och vägledning för Ineras FHIR-IG:ar finns på [Inera FHIR-landningssidan](https://www.inera.se/fhir).

Landningssidan ger en översikt av de konformanskrav som gäller för Ineras FHIR-standarder. Det inkluderar bland annat obligatoriska regler för tolkning av MustSupport, formella valideringskrav som implementatörer måste följa, förväntningar på narrativa texter och styrning av hur Inera Terminologi versioneras, samt säkerhets- och icke-funktionella krav för klienter och API:er.

Landningssidan erbjuder även stöd för utvecklare som vill förstå hur de arbetar med Ineras FHIR-standarder, inklusive en guide för att läsa en Implementation Guide och instruktioner för hur man använder FHIR-verktyg som valideringsuppsättning.
