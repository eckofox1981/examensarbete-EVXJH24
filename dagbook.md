# DAGBOK
---
## 2026-04-27 => 2026-08
| Vecka | Milstolpe | Leverabler |
| ----- | --------- | ---------- |
| 1 | Bakgrund och teori klar | Teoriavsnitt (utkast), OWASP-sammanfattning per kategori |
- Research into OWASP Top 10
- Notes written down on Samsung Notes
- Owasp A06 browsed through (not to be included in essay)
- Started filling in some abbreviations and checked general layout of project
- created repo for essay
- set up github project
**Time estimated:** 15 hours
---
| Vecka | Milstolpe | Leverabler |
| ----- | --------- | ---------- |
| 2 | Planering, förberedelse och initial analys klar | GitHub Projects uppsatt, branches skapade, hotmodell, samlad analysrapport med kodreferenser |
## 2026-05-11
- bakgrund klar
- syfte klar
- frågeställningar klar (från projektplan)
- skapa ny container för efbox data
    -   frankoadmin
    -   wefv32146321dsdd
- Avgränsningar klar
- Test av appen i dess nuvarande läge (funkar den alls?) [X]
    - testat, allmän genomgång av funktioner och endpoints
    - redan upptäckt att appen meddelar att användar namnet redan finns när jag råkade "återskapa" en användare
    - inte testat allt men verkar ok, fick några icke definierade 403, bättre än för mkt info men sisådär.
    - ska försöka postman för att slippa spara variablar höger och vänster
- Postman test:
    - satte upp alla endpoint med env. variablar för snabbare testning
    - allt funkar, börjat fundera på att skriva meddelande till användaren samt en struktur runt error/exception logging 
- skapade branch i github
    - original
    - OWASP safe
        - forks will move from this branch as necessary
   
**NOTE**: måste
 kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

**Time:** 8.5 hours

## 2026-05-11
- skrivit har studerat för:
    - 1.5 Metodöversikt
    - 2.1.1 Olika attacker mot API (4 st än så länge)
    - 2.1.2 Autentisering och auktorisering JWT
    - 2.1.3 Asymmetrisk kryptografi
    - 2.1.4 Kryptering, lösenordhashing och salting
    - 2.1.5 Inputvalidering
    - 2.1.6 Filvalidering
- mkt research för filvalidering gick på mitt sidospår med Apache Tika och Commons Validator fruktlöst, OWASP lydiga på den punkten men länkar till ett färdigt projekt, räknar mycket tidsbesparing

**Time:** 9.5 hours **TOTAL**: 18 hours
### Mål för morgon dagen:
- CORS och hotmodellering/STRIDE
- halvdag minus tiden till klassråd

**NOTE**: måste
 kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

---