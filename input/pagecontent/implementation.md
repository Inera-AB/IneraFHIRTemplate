# Implementering

Denna sida samlar det som implementatörer behöver bygga mot: konformanskrav, REST-interaktioner, sökparametrar, förväntade svar, felhantering, säkerhets- och behörighetsförutsättningar samt testning och validering.

---

### Skyldigheter

*Specificera vad varje aktör MÅSTE, BÖR eller FÅR göra vid implementering av denna IG. Använd HL7 FHIR:s terminologi för förmågor där det är lämpligt.*

| Aktör | Skyldighet |
|-------|------------|
| *[Aktörens namn]* | *Ange skyldigheten, t.ex. "SHALL kunna producera en [Resurs] som uppfyller [Profil]."* |
| *[Aktörens namn]* | *Ange skyldigheten, t.ex. "SHALL kunna konsumera och bearbeta en [Resurs] som uppfyller [Profil]."* |

---

### REST-interaktioner och sökparametrar

*Beskriv vilka REST-interaktioner (read, search, create, update, ...) varje aktör ska stödja, för vilka resurser/profiler. Lista de sökparametrar som stöds per resurs, inklusive eventuella kombinationer som ska stödjas.*

| Resurs/Profil | Interaktion | Sökparametrar |
|----------------|-------------|----------------|
| *[Profilnamn]* | *[t.ex. search, read]* | *[t.ex. `patient`, `date`]* |

---

### Förväntade svar

*Beskriv vilka svar en klient ska förvänta sig för respektive interaktion, inklusive statuskoder vid lyckat anrop, paginering vid sökningar, och eventuella `Bundle`-typer som används.*

---

### CapabilityStatement

*Om denna IG definierar formella CapabilityStatements, beskriv dem här och länka till respektive resurs. Ange vilken aktörsroll (t.ex. server/klient) varje CapabilityStatement representerar.*

---

### Felhantering

*Beskriv hur fel ska rapporteras, t.ex. via `OperationOutcome`. Ange vilka statuskoder och felkoder som kan förekomma och vad de betyder för respektive interaktion.*

---

### Säkerhet och behörighet

*Beskriv de säkerhets- och behörighetsförutsättningar som gäller för att ansluta till och använda denna IG:s gränssnitt, t.ex. autentiseringsmetod, behörighetsmodell och eventuella krav på samtycke. Hänvisa till Ineras generella säkerhetskrav på [FHIR på Inera](https://fhir.inera.se/) där tillämpligt.*

---

### Testning och validering

*Beskriv hur implementatörer kan testa och validera sina implementationer mot denna IG, t.ex. med hjälp av HL7 FHIR Validator och det publicerade IG-paketet (se [Nedladdningar](downloads.html)). Ange eventuell testmiljö eller testdatakälla.*

---

> **Vägledning för författare:** Håll innehållet konkret och tekniskt. Verksamhetsbakgrund hör hemma under [Introduktion](introduction.html), och verksamhetsansvar under [Funktionellt](functional.html).
