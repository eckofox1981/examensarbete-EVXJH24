# Mall för Examensarbete

## Sammanfattning (Abstract)

_Skriv detta avsnitt sist, även om det kommer först i rapporten._

Detta är en kortfattad sammanfattning (max 250 ord) på **engelska** som ska innehålla:

- Bakgrund och problemområde
- Syfte med arbetet
- Metod/approach (teoretisk, praktisk eller kombinerad)
- Huvudresultat och slutsatser
- Praktisk betydelse

**Nyckelord:** _Lista 3-5 tekniska nyckelord på engelska_

---

## Förkortningar och Begrepp

_Alfabetisk lista över tekniska termer, förkortningar och begrepp som används i rapporten._

| Term/Förkortning | Förklaring                                                                             |
| ---------------- | -------------------------------------------------------------------------------------- |
| API              | Application Programming Interface - Gränssnitt för kommunikation mellan mjukvarusystem |
| Backend          | |
| EU               | Europeiska Unionen |
| Frontend         | |
| GDPR             | General Data Protection Regulation |
|                  | |
|                  | |
|                  | |
| OWASP            | Open Worldwide Application Security Project |
| PCI DSS          | Payment Card Industry Data Security Standard |
| REST             | Representational State Transfer - Arkitekturstil för webbaserade API:er                |
| Spring Boot | ett open-source Java-ramverk som förenklar utvecklingen av webbapplikationer genom att erbjuda en snabb och enkel konfiguration|
| | |
| | |
| | |

---

## 1. Inledning

### 1.1 Bakgrund

Internet anses ha genomgått tre perioder [[1](1)] sen dess specifikation i 1989. 
Man pratar om __Web 1.0__ där användarna kunde, för det mesta, bara söka och läsa innehåll online. Kommunikationen skulle kunna beskrivas, på ett förenklat sätt, ensidig och användarinputs var begränsade.

Sedan 2000-talet tog __Web 2.0__ över världen med interaktiva tjänster och sociala media. Användarna kan nu skicka information på ett enkelt sätt. Det fanns redan säkerhetsproblem under Web 1.0, men nu behöver online-tjänster kunna hantera, på ett säkert sätt, information och kommando som skickas till servrerna. Säkerhet tas på desto större allvar då många lagrar personlig information online som måste skyddas på ett säkert sätt. Web 2.0 gjorde det möjligt för företag som Meta, Google, Amazon, Twitter/X och andra att bli värdsledande och implementera ett affärssystem där användardatan är en produkt som säljs i marknadsföringssyften. Hackerkulturen fortsatte samtidigt att utvecklas och attackerna blev alltmer avancerade. Man-in-the-middle-attacks, brute force attacks, Denial Of Service mm är hot som alla online-leverantörer måste ta i beaktning. Andra aktörer som statligt sponsrade hacker gör det ännu svårare att skydda informationen online.

På senare år har misstron mot internetjättarnas sätt att hantera vår personliga information tilltagit. De stora företagens sätt att hantera vår data ifrågasätts och i Europa tar E.U fram ett regelverk för att skydda användarna; GDPR, som ska skydda både lagring och överföring av data online. Samtidigt försöker många att decentralisera sig från de stora nätverken (statliga eller privata) genom att förlita sig mer på _peer-to-peer_ filosofin. Detta anses vara **Web 3.0**, ett decentraliserat internet.

Samtidigt i Europa har EU startat många konkurrensmål mot IT-jättarna [[2](2),  [3](3)], de politiska spänningarna mellan USA (där de flesta internationella tjänsteleverantörer finns) och Europa [[4](4)][[5](5)] har bidragit till att vissa EU-länder börjar leta efter alternativa tjänster, som t.ex Frankrikes mål att ersätta Microsoft tjänster med Linux baserade system för att uppnå digitalt självständighet [[6](6)].

Sammanfattningsvis genomgår internet en ny era där säkerheten kan komma att läggas på mindre utvecklingsteam, en ny marknad kommer att öppnas i samband med att EU-regionen minskar sitt beroende av uteomeuropeiska tjänster. Internetanvändare kan förvänta sig att nya tjänster publiceras online men frågan om datasäkerhet kommer att kvarstå: _hur säker är min information online?_

Denna studie avser att studera hur man kan göra en applikation säker baserat på OWASP Top 10 hot.

Open Worldwide Application Security Project (OWASP) är en internationell non-profit organisation med målet att förbättra säkerheten i programvaror och webbtjänster [[7](7)]. Organisationen är öppen och transparent – all dokumentation, verktyg och forskning publiceras fritt tillgängligt online.
Ett av organisationens mest kända bidrag är OWASP Top 10, en lista över de tio mest förekommande och kritiska säkerhetshoten i webbapplikationer. Listan baseras på data insamlad från hundratals organisationer världen över och uppdateras regelbundet för att spegla det aktuella hotlandskapet. Den senaste versionen publicerades 2025 [[8](7)].
OWASP Top 10 används globalt som referensram inom websäkerhet – både av enskilda utvecklare och av stora organisationer. Listan fungerar som ett gemensamt språk mellan utvecklare, säkerhetsexperter och verksamheter för att identifiera, prioritera och åtgärda säkerhetsrisker. Flera regulatoriska ramverk, däribland PCI DSS (betalkortsstandarden), refererar explicit till OWASP Top 10 som en del av sina krav [[9](9)].
I denna studie används OWASP Top 10:2025 som referensram för att systematiskt identifiera och åtgärda säkerhetsbrister i EFbox REST API.

### 1.2 Syfte

Syftet med detta examensarbete är att, med OWASP Top 10:2025 som referensram, systematiskt identifiera och åtgärda säkerhetsbrister i ett befintligt Java Spring Boot REST API. Studien syftar därigenom till att demonstrera hur säkerhet kan integreras i ett redan existerande projekt och bidra med ett konkret exempel på säkerhetsanalys av ett verkligt system.

### 1.3 Frågeställningar

1. Vilka säkerhetsbrister identifieras i EFbox REST API utifrån OWASP Top 10:2025?
2. Hur kan de identifierade bristerna åtgärdas inom ramen för det befintliga systemets arkitektur?
3. Hur verifieras att implementerade åtgärder är effektiva?

### 1.4 Avgränsningar

- Projektet omfattar endast backend-API:et (EFbox) – ingen frontend analyseras
- Analysen begränsas till de OWASP Top 10-kategorier som är relevanta för applikationens typ och funktionalitet (A01, A02, A03, A04, A05, A07, A08, A09, A10*)
- Penetrationstestning mot en live-miljö ingår inte – testning sker i lokal utvecklingsmiljö
- Projektet inkluderar inte prestandaoptimering eller funktionsutveckling utanför säkerhetsåtgärder
- Applikationen är ej avsedd för produktionsdrift inom ramen för detta projekt

_*A06 - Insecure design är för subjektivt för att kunna bedömas på ett empiriskt sätt. Därför ignoreras den punkten i denna studie._

### 1.5 Metodöversikt

Kort beskrivning av din approach:[9](9)

- **Teoretisk studie:** Litteraturstudier, jämförande analys
- **Experimentell:** Praktiska test, mätningar, prototyping
- **Utveckling:** Systemutveckling, implementation
- **Kombinerad:** Mix av ovanstående

---

## 2. Teoretisk Grund och Relaterat Arbete

### 2.1 Tekniska Koncept

Förklara viktiga koncept inom ditt omgrundad år 2001råde:

- Grundläggande tekniska begrepp
- Relevanta programmeringsspråk/teknologier
- Arkitekturella mönster eller teorier

### 2.2 Befintlig Forskning och Lösningar

**För utforskande arbeten:**

- Tidigare studier och forskning
- Befintliga teorier och modeller
- Identifierade kunskapsluckor[1]

**För utvecklingsprojekt:**

- Liknande applikationer/system
- Open source-projekt
- Kommersiella lösningar

**För hybridprojekt:**

- Kombination av forskning och praktiska lösningar

### 2.3 Teknisk/Teoretisk Jämförelse

Analys av olika alternativ eller approaches inom ditt område.

_Använd källhänvisningar_

---

## 3. Metod och Genomförande

_Anpassa detta kapitel efter din typ av arbete:_

### 3.1 Övergripande Arbetsgång

Beskriv din systematiska approach:

**För teoretiska studier:**

- Litteratursökning och källkritik
- Analysmetod
- Syntes och jämförelse

**För experimentella studier:**

- Experimentdesign
- Testmiljö och verktyg
- Mätmetoder

**För utvecklingsprojekt:**

- Utvecklingsprocess (agile, iterativ etc.)
- Design och arkitektur
- Implementation och testning

### 3.2 Verktyg och Tekniker

**För teoretiska studier:**

- Databaser för litteratursökning
- Analysverktyg
- Kategoriseringsmetoder

**För praktiska studier:**

- Programmeringsverktyg
- Testverktyg och mätinstrument
- Utvecklingsmiljöer

### 3.3 Datainsamling och Analys

**För utforskande arbeten:**

- Sökstrategierförsöker
- Urvalskriterier
- Tidigare studier och forskning
- Befintliga teorier och modeller
- Analysmetoder

**För experimentella arbeten:**

- Experimentuppställning
- Datainsamlingsmetoder
- Statistiska metoder

**För utvecklingsprojekt:**

- Requirements gathering
- Teststrategier
- Utvärderingsmetoder

### 3.4 Kvalitetssäkring

- Metodkvalitet och tillförlitlighet
- Validering av resultat
- Hantering av bias eller fel

---

## 4. Resultat

_Anpassa detta kapitel efter din typ av arbete:_

### 4.1 Huvudresultat

**För teoretiska studier:**

- Sammanställning av litteraturfynd
- Identifierade mönster och trender
- Jämförelse mellan olika källor/teorier

**För experimentella studier:**

- Mätresultat och data
- Statistisk analys
- Mätmetoder

**För utvecklingsprojekt:**

- Utvecklingsprocess (agile, iterativ etc.)
- Observationer från experiment

**För utvecklingsprojekt:**

- Beskrivning av utvecklad lösning
- Funktionalitet och egenskaper
- Testresultat

### 4.2 Detaljerade Fynd

Fördjupade resultat relevanta för dina frågeställningar.

### 4.3 Oväntade Resultat

Resultat som inte förväntades eller avvikelser från hypoteser.

_Presentera alla resultat objektivt utan värderingar - spara analysen till diskussionsavsnittet_

---

## 5. Diskussion

### 5.1 Analys av Resultat

- Uppfylldes arbetets syfte?
- Svarar resultaten på frågeställningarna?
- Jämförelse med befintlig forskniförsökerng/lösningar

### 5.2 Reflektion över Metod

**För alla typer av arbeten:**

- Styrkor och svagheter i vald metod
- Problem och lösningar under arbetsgången
- Vad som fungerade bra/mindre bra
- Lärdomar från processen

### 5.3 Begränsningar och Kritisk Granskning

- Metodologiska begränsningar
- Tekniska eller teoretiska begränsningar
- Vad som kunde gjorts annorlunda
- Påverkan på resultatens tillförlitlighet

### 5.4 Bredare Perspektiv

- Implikationer för området
- Praktisk tillämpbarhet
- Teoretiskt bidrag

---

## 6. Slutsatser

### 6.1 Huvudslutsatser

Konkreta slutsatser baserade på resultaten:

- Tydliga svar på frågeställningarna
- Måluppfyllelse
- Nya insikter eller kunskap

### 6.2 Bidrag och Betydelse

- Vad tillför arbetet till området?
- Praktisk eller teoretisk betydelse
- Värde för yrkeskåren

### 6.3 Framtida Arbete

Förslag på:

- Vidareutveckling av forskning/utveckling
- Oresolerade frågor
- Nya forsknings-/utvecklingsområden
- Praktiska tillämpningar

---

## 7. Referenser

Använd valfri referenstil. Rekommentation: IEEE

I referenslistan 

[1] Bandar Alotaibi, “Cybersecurity Attacks and Detection Methods in Web 3.0 Technology: A Review”, Sensors, Januari 2025. Available: https://www.mdpi.com/1424-8220/25/2/342

[2] “Antitrust cases against Google by the European Union”, Wikipedia, Accessed Maj 2026. Available: https://en.wikipedia.org/wiki/Antitrust_cases_against_Google_by_the_European_Union

[3] Vatsala Gaur, “How the EU is taking on Big Tech: Meta, Apple, Google, face heightened scrutiny, penalties”, Invezz, December 2025. Available: https://invezz.com/news/2025/12/04/how-the-eu-is-taking-on-big-tech-meta-apple-google-face-heightened-scrutiny-penalties/

[4] “Digital Sovereignty in Tension: U.S. Pushback Against the EU’s Digital Services Act”, The Cyber Institute, Augusti 2025. Available: https://www.cyber-institute.org/post/digital-sovereignty-in-tension-u-s-pushback-against-the-eu-s-digital-services-act

[5] Clare Duffy, “Trump administration’s vision of US tech dominance is colliding with Europe ”, CNN, Januari 2026. Available: https://edition.cnn.com/2026/01/12/tech/us-eu-tech-regulation-fight-explained

[6] Marius Laffont, “Pour son indépendance numérique, l'État français souhaite passer à Linux”, RFI, April 2026. Available: https://www.rfi.fr/fr/france/20260410-pour-son-ind%C3%A9pendance-num%C3%A9rique-l-%C3%A9tat-fran%C3%A7ais-souhaite-passer-%C3%A0-linux

[7] OWASP about page, Accessed: Maj 2026. Available: https://owasp.org/about/

[8] OWASP Top 10 threats, Accessed: Januri -Juni 2026. Available: https://owasp.org/www-project-top-ten/

[9] Lorikeet Security Team, "PCI DSS Requirement 6: Secure Development Practices Your QSA Will Scrutinize", Accessed: Maj 2026. Available: https://lorikeetsecurity.com/blog/pci-dss-requirement-6-secure-development

Bok: [2] A. Author, Title of Book. City, State: Publisher, Year.

Webbsida: [3] Title of Website. Publisher. [Online]. Available:
http://www.website.com. [Accessed: Day Month Year].

Exempel I texten:
“Prestandan för neurala nätverk i Python har visat sig vara 30% bättre än
traditionella metoder [1].”

I referenslistan:
[1] K. Johnson and M. Smith, “Performance Analysis of Neural Networks in
Python,” Journal of Computer Science, vol. 12, no. 3, pp. 234-245, Mar. 2023.

---

## Bilagor

_Anpassa efter typ av arbete:_

**För teoretiska studier:**

- Bilaga A: Litteratursökning och urvalskriterier
- Bilaga B: Sammanställning av källor
- Bilaga C: Analysscheman

**För experimentella studier:**

- Bilaga A: Experimentdata
- Bilaga B: Testprotokoll
- Bilaga C: Statistiska beräkningar

**För utvecklingsprojekt:**

- Bilaga A: Källkod (utdrag eller repo-referens)
- Bilaga B: Teknisk dokumentation
- Bilaga C: Testresultat

**För alla typer:**

- Bilaga X: Projektplanering och tidsrapporter
