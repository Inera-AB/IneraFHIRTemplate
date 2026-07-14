# Hem

### [Domännamn]

*Ange ett namn som ringar in det avgränsade område som denna IG realiserar, t.ex. "Fasta kontakter" eller "Öppenvårdsemiss". 

Välj ett kort, stabilt och beskrivande namn som speglar informationsmängden eller API:ets syfte, snarare än projekt-, organisations- eller versionsnamn. Domännamnet (canonical URL) ska ligga under en långsiktigt förvaltad Inera-domän, https://inera.se/fhir/ig/<namn>, och ska inte innehålla versionsnummer eftersom versioner hanteras i FHIR-paketet och Implementation Guidens metadata. Använd samma namn konsekvent i paketnamn, repository, canonical URL och dokumentation för att underlätta identifiering och återanvändning.*

---

### Omfattning

*Definiera vad denna IG täcker och vad den inte täcker. Specificera vilka typer av användningsfall, kliniska scenarier eller informationsutbyten som ingår. Nämn eventuella explicita avgränsningar. Om denna IG är avsedd att användas som bas för mer specialiserade guider, säg det här. Exempel: "Denna IG gäller för X inom ramen för Y. Den täcker inte Z. Härledda guider kan begränsa eller utöka omfattningen vid behov."*

---

### Syfte

*Beskriv målet med denna IG. Förklara vilket problem den löser och hur den passar in i det bredare Inera- och e-hälsolandskapet. Hänvisa till nationella eller regionala program, standarder eller mandat som motiverat arbetet.*

---

### Målgrupp

*Ange vem denna IG riktar sig till, t.ex. implementatörer, systemleverantörer, vårdgivare och/eller förvaltningsorganisationer. Beskriv vilken bakgrundskunskap läsaren förväntas ha (t.ex. grundläggande FHIR-kunskap) och hänvisa till [Inledning](introduction.html) och Implementering (se t.ex. [REST-interaktioner och sökparametrar](rest-interactions.html)) för den som ska bygga mot IG:n.*

---

### Terminologi

På [Inera Terminologitjänst](https://www.inera.se/tjanster/alla-tjanster-a-o/terminologitjanst-for-nationell-e-halsa/) finns alla refererade kodsystem och värdemängder som utvecklats av Inera.

---

### Beroenden

Om denna IG har ett beroende till t ex SE-core, definierat av HL7 Sweden ska detta beskrivas här. Detta återspeglas även i de profiler som ärver från eller refererar till SECore-profiler.

---

### Dokumentation

Mer information om FHIR på Inera  finns [här](https://fhir.inera.se/). FHIR på Inera är en del av RIVTA – referensarkitekturen för svensk hälso- och sjukvård.

Information om hur denna IG förvaltas finns under [Om](about.html).

---

### Om mallens struktur

Menyn i denna IG är medvetet utformad efter samma mönster som europeiska specifikationer, t.ex. FHIR ePS (Hem / Inledning / Funktionellt / Implementering / Om / Artefakter), för att ge implementatörer en igenkännbar ingång oavsett vilken europeisk FHIR-IG de arbetar med.

Informationsmodellen för denna förmåga publiceras externt och fristående i en egen informationsspecifikation (se [Informationsunderlag](information-basis.html) under Funktionellt) snarare än i denna IG. Det håller informationsspecifikationen som den auktoritativa källan för begrepp och informationsstruktur, och undviker att IG:n och specifikationen glider isär över tid.
