# DAGBOK

---

## 2026-04-27 => 2026-08

| Vecka | Milstolpe               | Leverabler                                               |
| ----- | ----------------------- | -------------------------------------------------------- |
| 1     | Bakgrund och teori klar | Teoriavsnitt (utkast), OWASP-sammanfattning per kategori |

- Research into OWASP Top 10
- Notes written down on Samsung Notes
- Owasp A06 browsed through (not to be included in essay)
- Started filling in some abbreviations and checked general layout of project
- created repo for essay
- set up github project
  **Time estimated:** 15 hours

---

| Vecka | Milstolpe                                       | Leverabler                                                                                   |
| ----- | ----------------------------------------------- | -------------------------------------------------------------------------------------------- |
| 2     | Planering, förberedelse och initial analys klar | GitHub Projects uppsatt, branches skapade, hotmodell, samlad analysrapport med kodreferenser |

## 2026-05-11

- bakgrund klar
- syfte klar
- frågeställningar klar (från projektplan)
- skapa ny container för efbox data
  - frankoadmin
  - wefv32146321dsdd
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

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

**Time:** 8.5 hours **TOTAL**: 23.5 hours

## 2026-05-12

- skrivit har studerat för:
  - 1.5 Metodöversikt
  - 2.1.1 Olika attacker mot API (4 st än så länge)
  - 2.1.2 Autentisering och auktorisering JWT
  - 2.1.3 Asymmetrisk kryptografi
  - 2.1.4 Kryptering, lösenordhashing och salting
  - 2.1.5 Inputvalidering
  - 2.1.6 Filvalidering
- mkt research för filvalidering gick på mitt sidospår med Apache Tika och Commons Validator fruktlöst, OWASP lydiga på den punkten men länkar till ett färdigt projekt, räknar mycket tidsbesparing

**Time:** 9.5 hours **TOTAL**: 34 hours

### Mål för morgon dagen:

- CORS och hotmodellering/STRIDE
- halvdag minus tiden till klassråd

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

## 2026-05-13

- förlorad tid på klassråd (2timmar) och ingen skola i em
- skrivit och studerat för:
  - 2.1.5 HTTP and API security
  - 2.1.6 Threat modeling
  - 2.1.7 Owasp top 10
  - 2.1.7.1 Broken Access Control
  - 2.1.7.2 Security Misconfigurations
- se till att överblicka innehåll, orolig att för lite (eller för mkt?)

**Time:** 5.75 hours **TOTAL**: 39.75 hours

### Mål för morgon dagen:

- fortsatt top 10

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

## 2026-05-14

- Nästan klar med kapitel 2, kommer behöva se över påstående och eventuellt backa/förbättra på några punkter
- skrivit och studerat för: - 2.1.7.3 Software Supply Chain Failures - 2.1.7.4 Cryptographic Failures - 2.1.7.5 Injection - 2.1.7.6 insecure design - 2.1.7.7 Authentication failures - 2.1.7.8 Software integrity - 2.1.7.9 Software integrity - 2.1.7.10 Mishandling of exceptions
- se till att överblicka innehåll, orolig att för lite (eller för mkt?)
- har börjat på befintliga lösningar med anser att OWASP i sig är svaret. La till en studie från Barcelona för att visa att jag sökt.

**Time:** 7 hours **TOTAL**: 46 hours

### Mål för morgon dagen:

- vore bra att få till ZAP

### TODO

- boka tid med William för kontroll

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

## 2026-05-15

- half day, started existing studies

### Mål för morgon dagen:

- vore bra att få till ZAP
- ha svårt att få ZAP att fungerar (fick det att funka men få svaren i HTTPS...)

### TODO

- boka tid med William för kontroll

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

**Time:** 4 hours **TOTAL**: 50 hours

## 2026-05-16

- ett par timmar ikväll
- ZAP fungerar (!), "enklaste" var att skaffa ett lokalt certifikat och skicka request med HTTPS från början

### Mål för morgon dagen:

- Börja analys av programmet

### TODO

- boka tid med William för kontroll

**NOTE**: måste kartlägga alla endpoint och JSON-objekt

**NOTE**: problem med markdown, vägra länka till referencerna

**Time:** 3 hours **TOTAL**: 53 hours

## 2026-05-17

- 3.3.1 kartläggning av EFbox, struktur och end-points
  - 3.3.1.1 Översikt
  - 3.3.1.2 Struktur
  - 3.3.1.3 End-points
- 3.3.2 Hotmodellering av EFbox
- 3.3.3 Automatisk och Manuell kodgranskning av EFbox ur ett säkerhetsperspektiv
  - 3.3.3.1 Analys med SonarQube
- har skapat en originalForEssay bransch från main
- bra dag!

### Mål för morgon dagen:

- Börja manauelanalys av programmet

### TODO

- boka tid med William för kontroll

**NOTE**: problem med markdown, vägra länka till referencerna

**Time:** 6 hours **TOTAL**: 59 hours

## SUMMERING FÖR VECKAN 2 (44 hours)

[x] Planering,
[x] förberedelse och initial analys klar -> att debatera vare sig klar eller ej
[x] GitHub Projects uppsatt,
[] branches skapade, -> blir under arbetetsgång för att skapa bransch från godkända bransch
[x] hotmodell,
[] samlad analysrapport med kodreferenser -> nästan

---

| Vecka | Milstolpe                       | Leverabler                                       |
| ----- | ------------------------------- | ------------------------------------------------ |
| 3     | Implementering av åtgärder klar | Åtgärdad kod i feature-branch, commit per åtgärd |

## 2026-05-17

- bokat med wiliam: kl 11 imorgon
- kommit halvägs på manuell granskningsrapport
- gått igenom GitHub project

### Mål för morgon dagen:

- göra klar Granskning
- ZAP test med rapport

**Time:** 8 hours **TOTAL**: 67 hours

## 2026-05-17

- möte med william, utan nån större anmärkning, dock rapport lite lång (kämpa på nu, fixa sen). Glömde länk till referenserna (googla!)
- Alla granskningar klar (inkl. ZAP), t.o.m hunnit med planeringen och kvalitetssäkringen
- lagt till https (kortare version pga feedback)

### Mål för morgon dagen:

- konfiguration
- börja (eller hinna?) med infrastruktur **OBS: github project!**
-
- **NOTE**: fixa länkar på egen hand

**Time:** 8.5 hours **TOTAL**: 75.5 hours

## 2026-05-17

- `config-fixes` branchen klar och mergeat, Claude AI duktig på review men feta rapporter.
- Påbörjat `new-exception-handling` brach på G, skapat GlobalException handler, enums for exceptions, custom exceptions, har testimplementerat i UserController och UserService, verkar funka. Har skapat en errorMessage för att förberreda inför logging.
- insett under `config-fixes` att Spring Boot kommer med färdiga security headers, kul men fick redigera Manuella Granskning och rapport.

### Mål för morgon dagen:

- Göra klart `config-fixes`, få det reviewat, påbörja `logging-exception`
- **NOTE**: fixa länkar på egen hand

## **Time:** 9 hours **TOTAL**: 85.5 hours

| Vecka | Milstolpe                 | Leverabler                                                    |
| ----- | ------------------------- | ------------------------------------------------------------- |
| 4     | Testning och rapport klar | Testrapport (före/efter per brist), fullständig rapportutkast |

---

| Vecka | Milstolpe   | Leverabler                                       |
| ----- | ----------- | ------------------------------------------------ |
| 5     | Redovisning | Inlämnad rapport, genomförd muntlig presentation |

---
