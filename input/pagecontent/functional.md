# Funktionellt

Denna sida beskriver de funktionella/verksamhetsmässiga kraven på förmågan: vilka användningsfall som stöds och vilka roller som deltar med vilket ansvar. Tekniska REST-detaljer hör hemma under Implementering, se t.ex. [CapabilityStatement](capabilitystatement.html).

---

### Användningsfall

*Beskriv varje användningsfall som denna IG stöder. För varje användningsfall, förklara vad som utlöser det, vilka aktörer som deltar och vilket resultat det leder till.*

#### [Användningsfallets namn]

*Beskriv användningsfallet. Exempel: "En vårdgivare behöver hämta en patients fasta kontakter för att avgöra vem som ska kontaktas vid en vårdhändelse."*

---

### Roller och ansvar

*Lista de system eller mänskliga roller som deltar i användningsfallen ovan. Beskriv för varje roll dess verksamhetsansvar i sammanhanget för denna IG — vem som ansvarar för vad, inte hur anropen tekniskt går till.*

| Roll | Ansvar |
|------|--------|
| *[Rollens namn]* | *Beskriv rollen och ansvarsområdena för denna aktör.* |
| *[Rollens namn]* | *Beskriv rollen och ansvarsområdena för denna aktör.* |

*Ange för varje roll vad den förväntas kunna göra vid implementering av denna IG, t.ex. med HL7 FHIR:s terminologi för förmågor där det är lämpligt.*

| Roll | Förväntning |
|------|-------------|
| *[Rollens namn]* | *Ange förväntningen, t.ex. "SHALL kunna producera en [Resurs] som uppfyller [Profil]."* |
| *[Rollens namn]* | *Ange förväntningen, t.ex. "SHALL kunna konsumera och bearbeta en [Resurs] som uppfyller [Profil]."* |

Fr.o.m. FHIR R5 kan `CapabilityStatement` uttrycka denna typ av förväntningar formellt via elementet `obligations`. Om denna IG definierar formella CapabilityStatements, se [CapabilityStatement](capabilitystatement.html) under Implementering.

*Referera till eller återge kortfattat det arbetsflöde där dessa roller interagerar — vad som utlöser flödet, i vilken ordning rollerna agerar och vad resultatet blir. Se informationsunderlaget under [Introduktion](introduction.html) för den fullständiga, auktoritativa beskrivningen.*

---

> **Vägledning för författare:** Referera till relevanta nationella föreskrifter eller RIVTA-tjänstekontrakt där det är tillämpligt.
