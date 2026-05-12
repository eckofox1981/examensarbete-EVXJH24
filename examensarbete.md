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

| Term/Förkortning | Förklaring                                                                                                                                                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| API              | Application Programming Interface - Gränssnitt för kommunikation mellan mjukvarusystem                                                                       |
| Backend          | basbearbetningen (ofta på servernivå)                                                                                                                        |
| CORS             | Cross-origin resource sharing, en teknik som begränsar åtkomst till websidor från specifierade                                                               |
| ECDSA            | Elliptisk kurva digital signaturalgoritm, en av de mer komplexa offentliga nyckelkrypteringsalgoritmer                                                       |
| EU               | Europeiska Unionen                                                                                                                                           |
| Frontend         | Användargränssnittsorienterad bearbetning (webbsida, mobilapplikationer mm)                                                                                  |
| GDPR             | General Data Protection Regulation                                                                                                                           |
| Header           | "Huvud" på svenska, början på ett meddelande inom datateknik som innehåller metadata om meddelandet (hur den ska tolkas)                                     |
| HTML             | HyperText Markup Language, programmeringsspråket som lägger grunden till webbsidor                                                                           |
| HTTP             | Hypertext Transfer Protocol, ett kommunikationsprotokoll som används för att överföra information på internet                                                |
| HTTPS            | Hypertext Transfer Protocol Secure, protokoll för krypterad transport av data för HTTP-protokollet                                                           |
| IDE              | Integrated Development Environment, miljön där utvecklare skriver sin kod                                                                                    |
| IntelliJ         | En IDE utvecklad av JetBrain                                                                                                                                 |
| Java             | Ett av de vanligaste programmeringspråken                                                                                                                    |
| JavaScript       | Ett programmeringsspråk som ger interaktivitet och dynamik till webbsidor, främst när de körs i webbläsaren                                                  |
| Json             | JavaScript Object Notation ett kompakt, textbaserat format som används för att utbyta data                                                                   |
| JWT              | Json Web Token, ett standardiserat sätt att överföra information som Json-objekt                                                                             |
| Open-source      | Öppen källkod som inte är proprietärt, dvs illgänglig att använda, läsa, modifiera och vidaredistribuera för den som vill                                    |
| OWASP            | Open Worldwide Application Security Project                                                                                                                  |
| PCI DSS          | Payment Card Industry Data Security Standard                                                                                                                 |
| Plug-in          | Tilläggsprogram om inte körs fristående utan installeras som ett tillägg i ett annat program                                                                 |
| Postman          | Ett verktyg för utvecklare för att testa API                                                                                                                 |
| RSA              | ett krypteringsalgoritm döpt uppkallad efter dess skapare Rivest, Shamir och Adleman. Systemet kräver en nyckel för kryptering och en annan för avkryptering |
| REST             | Representational State Transfer - Arkitekturstil för webbaserade API:er                                                                                      |
| Spring Boot      | ett open-source Java-ramverk som förenklar utvecklingen av webbapplikationer genom att erbjuda en snabb och enkel konfiguration                              |
| SQL              | Structured Query Language, ett programmeringsspråk som används för hantera och manipulera relationsdatabaser                                                 |
| TLS              | Transport Layer Security är ett kryp­te­rings­pro­to­koll som sä­ker­stäl­ler säker da­taö­ver­fö­ring på internet.                                          |
| ZAP              | Zed Attack Proxy, en _open-source_ programvara som används i samband säkerhetstestning av applikationer                                                      |

---

## 1. Inledning

### 1.1 Bakgrund

Internet anses ha genomgått tre perioder [1] sen dess specifikation i 1989.
Man pratar om **Web 1.0** där användarna kunde, för det mesta, bara söka och läsa innehåll online. Kommunikationen skulle kunna beskrivas, på ett förenklat sätt, ensidig och användarinputs var begränsade.

Sedan 2000-talet tog **Web 2.0** över världen med interaktiva tjänster och sociala media. Användarna kan nu skicka information på ett enkelt sätt. Det fanns redan säkerhetsproblem under Web 1.0, men nu behöver online-tjänster kunna hantera, på ett säkert sätt, information och kommando som skickas till servrerna. Säkerhet tas på desto större allvar då många lagrar personlig information online som måste skyddas på ett säkert sätt. Web 2.0 gjorde det möjligt för företag som Meta, Google, Amazon, Twitter/X och andra att bli värdsledande och implementera ett affärssystem där användardatan är en produkt som säljs i marknadsföringssyften. Hackerkulturen fortsatte samtidigt att utvecklas och attackerna blev alltmer avancerade. Man-in-the-middle-attacks, brute force attacks, Denial Of Service mm är hot som alla online-leverantörer måste ta i beaktning. Andra aktörer som statligt sponsrade hacker gör det ännu svårare att skydda informationen online.

På senare år har misstron mot internetjättarnas sätt att hantera vår personliga information tilltagit. De stora företagens sätt att hantera vår data ifrågasätts och i Europa tar E.U fram ett regelverk för att skydda användarna; GDPR, som ska skydda både lagring och överföring av data online. Samtidigt försöker många att decentralisera sig från de stora nätverken (statliga eller privata) genom att förlita sig mer på _peer-to-peer_ filosofin. Detta anses vara **Web 3.0**, ett decentraliserat internet.

Samtidigt i Europa har EU startat många konkurrensmål mot IT-jättarna [2][3], de politiska spänningarna mellan USA (där de flesta internationella tjänsteleverantörer finns) och Europa [4][5] har bidragit till att vissa EU-länder börjar leta efter alternativa tjänster, som t.ex Frankrikes mål att ersätta Microsoft tjänster med Linux baserade system för att uppnå digitalt självständighet [6].

Sammanfattningsvis genomgår internet en ny era där säkerheten kan komma att läggas på mindre utvecklingsteam, en ny marknad kommer att öppnas i samband med att EU-regionen minskar sitt beroende av uteomeuropeiska tjänster. Internetanvändare kan förvänta sig att nya tjänster publiceras online men frågan om datasäkerhet kommer att kvarstå: _hur säker är min information online?_

Denna studie avser att studera hur man kan göra en applikation säker baserat på OWASP Top 10 hot.

Open Worldwide Application Security Project (OWASP) är en internationell non-profit organisation med målet att förbättra säkerheten i programvaror och webbtjänster [7]. Organisationen är öppen och transparent – all dokumentation, verktyg och forskning publiceras fritt tillgängligt online.
Ett av organisationens mest kända bidrag är OWASP Top 10, en lista över de tio mest förekommande och kritiska säkerhetshoten i webbapplikationer. Listan baseras på data insamlad från hundratals organisationer världen över och uppdateras regelbundet för att spegla det aktuella hotlandskapet. Den senaste versionen publicerades 2025 [8].
OWASP Top 10 används globalt som referensram inom websäkerhet – både av enskilda utvecklare och av stora organisationer. Listan fungerar som ett gemensamt språk mellan utvecklare, säkerhetsexperter och verksamheter för att identifiera, prioritera och åtgärda säkerhetsrisker. Flera regulatoriska ramverk, däribland PCI DSS (betalkortsstandarden), refererar explicit till OWASP Top 10 som en del av sina krav [9].
I denna studie används OWASP Top 10:2025 som referensram för att systematiskt identifiera och åtgärda säkerhetsbrister i EFbox REST API.

### 1.2 Syfte

Syftet med detta examensarbete är att, med OWASP Top 10:2025 som referensram, systematiskt identifiera och åtgärda säkerhetsbrister i ett befintligt Java Spring Boot REST API. Studien syftar därigenom till att demonstrera hur säkerhet kan integreras i ett redan existerande projekt och bidra med ett konkret exempel på säkerhetsanalys av ett verkligt system.

### 1.3 Frågeställningar

1. Vilka säkerhetsbrister identifieras i EFbox REST API utifrån OWASP Top 10:2025?
2. Hur kan de identifierade bristerna åtgärdas inom ramen för det befintliga systemets arkitektur?
3. Hur verifieras att implementerade åtgärder är effektiva?

### 1.4 Avgränsningar

- Projektet omfattar endast backend-API:et (EFbox) – ingen frontend analyseras
- Analysen begränsas till de OWASP Top 10-kategorier som är relevanta för applikationens typ och funktionalitet (A01, A02, A03, A04, A05, A07, A08, A09, A10\*)
- Penetrationstestning mot en live-miljö ingår inte – testning sker i lokal utvecklingsmiljö
- Projektet inkluderar inte prestandaoptimering eller funktionsutveckling utanför säkerhetsåtgärder
- Applikationen är ej avsedd för produktionsdrift inom ramen för detta projekt

_\*A06 - Insecure design är för subjektivt för att kunna bedömas på ett empiriskt sätt. Därför ignoreras den punkten i denna studie._

### 1.5 Metodöversikt

Målet med detta arbete är att åstadkomma en **kombinerad teoretisk och utvecklingsstudie** där teori och praktik sammanstrålar.

I första stadiet studeras de olika hot listade i OWASP Top 10 följd av en analys av EFBox-API:et för att identifiera dess svaghet. Nästa steg är att åtgärda dessa brister på ett effektivt sätt dvs genom att lösa flera stycken på en gång (ex: log och felhantering är vanligtvis närbesläktade).
Sista steg är att återanalysera API:et för att se om åtgärdena är effektiva.

De verktyg som används i denna studie är:

- IntelliJ (IDE)
- Java version 21 (programmeringspråk)
- Postman
- ZAP
- SonarQube för IDE (en plug-in för IDE:er för kodkvalitetsgranskning och olika komplexitetsmätningar)
- Claude AI kommer också att användas för kodgranskning (eftersom arbetet bedrivs på egen hand) <u>inte för att driva studien</u>.
- FILLER /TODO: ta bort om ej mer

---

## 2. Teoretisk Grund och Relaterat Arbete

### 2.1 Tekniska Koncept

#### 2.1.1 Olika attacker mot API

##### 2.1.1.1 Man in middle attack (MITM)

En Man-in-the-middle-attack (MITM), "mannen i mitten" på svenska, är en cyberattack där angriparen i hemlighet fångar upp och vidarebefordrar meddelanden mellan två parter som tror att de kommunicerar direkt med varandra. Genom att positionera sig som mellanhand kan anfallaren läsa av och kontrollera informationsflödet. Detta gör det möjligt att skicka egen data eller kod [10] [11] [12]. En jämförelse skulle vara att man skickar en beställning till ett företag med posten. Under postgången får en tredje part tag på beställningsformulär, ändrar dess innehåll (kanske ändra mottagaradressen till sitt eget) och skickar det vidare.

<u>Skyddas med hjälp utav:</u> HTTPS/TLS (kryptering av dataflödet)

##### 2.1.1.2 Code-Injection

En attack där hackern _injicerar_ sin egen kod i en applikation som körs för att ändra dess beteende. En vanlig injection är så kallade **SQL-injection** där användarinputsfält används för att anropa databasen direkt. T.ex kan anfallaren försöka skriva ett SQL-kommando i ett textinput\* och läsa av svaret från servern om denna inte är skyddad mot detta. Det är även möjligt att ändra data i databasen (som att göra sitt eget konto till admin).
**Cross-site scripting (XSS)** är också en vanlig form av injection där kod injiceras med hjälp av HTML- eller javascriptkod [12] [13].

<u>Skyddas med hjälp utav:</u> inputvalidering (kontroll att inga kommando injiceras)

_*Förenklad exempel: 'SELECT * FROM users;' skulle kunna ge all användardata från serverns databas. I verklighet skulle en anfallare använda sig av sk. or-statements som "' OR '1'=1" som betyder: "om det inte funkar: ger mig allt"._

##### 2.1.1.3 Brute force attack

Brute force (engelska för råstyrka) är en metod för att hitta exempelvis lösenord genom att pröva alla möjliga kombinationer. Termen brute force syftar oftast på att hitta lösenord och nycklar [14]. Om ett lösenord är enkelt går attack snabbare medan väldigt komplicerade lösenord tar längre tid att hitta. Detta kan jämföras med ett kombinationslås med bara tre siffror och ett med sex stycken där en tjuv testar alla möjliga kombinationer [15].

<u>Skyddas med hjälp utav:</u> varningar vid upprepade misslyckade inloggningsförsök och låsning av berörda konton, starkt lösenordspolicy

##### 2.1.1.4 Cross Site Request Forgery

En hacker kan använda en annans rättigheter hos en tjänst och lura till sig en oönskad handling. Istället för att, på ett avancerat sätt, få tag på en användarens uppgifter kan man använda dess rättigheter direkt (_Request Forgery_). Detta kan ske via en extern länk från en annan sajt (_Cross Site_).

De flesta online-leverantörer delar ut en nyckel, så kallad token eller session-token, till sina användare som fungerar som ett tillstånd att använda tjänsten och komma åt sin data. En hacker kan lyckas nyttja nyckelns privilegier genom att få användaren att omedvetet utföra handlingar [12].

Ett bra exempel är att anfallaren skickar en phishinglänk via email till offret som klickar på det. Länken skickar egentligen en förfrågan till offrets bank om att överföra pengar till anfallaren. Om offret är inloggad på bankenshemsida vid klickandet kan överföringen ske [16].

Detta kan jämföras med att en tjuv lura ett offer att låsa upp hemmet för att kunna komma in.

<u>Skyddas med hjälp utav:</u> skydda känsliga handlingar som tex en banköverföring, header-verifiering (mm)

##### 2.1.1.5 /TODO: spare

#### 2.1.2 Autentisering och auktorisering JWT

JSON Web Token (JWT) är en standard öppen för utvecklare som definierar ett säkert sätt att överföra information. Eftersom datan signeras digitalt är den tillförlitlig. Signaturen kan ske med en så kallad _secret_ ("hemlighet", använder sig av HMAC algoritmen) eller ett allmänt/privat nyckelpar med hjälp av RSA eller ECDSA.
Denna studie fokuserar på JWT-användningen inom signerade token och inte de krypterade token eftersom de signerade används för identifiering av användare. När en token är signerad får servern en bekräftelse på att avsändaren är den som signerat det.

JWT används vid **auktorisering** för att bekräfta att användaren har tillgång till serverns olika tjänster, i såna fall skickas JWT:n med varje förfrågan till skyddade system.

```
//exempel på en JWT token, notera strukturen i tre delar med punkter (.)
 Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30
```

JWT-strukturen har följande mönster "xxxx.yyyy.zzzzz" där "xxxx" representerar en _header_ (metadata med tokentyp och signeringsalgoritm), "yyyy" är själva innehållet och "zzzz" står för signeringen.

JWT kan skickas i Authorization-headern och implementerar 'Bearer'-schemat (dvs att det står 'Bearer', bärare på svenska, före token med mellanslag). Alternativt kan JWT:n skickas som en cookie men kräver då att CORS konfigureras för detta [12].

#### 2.1.3 Asymmetrisk kryptografi

Vid asymmetrisk kryptografi används ett nyckelpar – en offentlig och en privat nyckel. Den offentliga nyckeln kan delas fritt medan den privata aldrig lämnar ägaren.
Det som signeras med den privata nyckeln kan verifieras av vem som helst med den offentliga nyckeln. Den offentliga nyckeln kan vara identifierad av en certifikatutfärdare (CA), vilket säkerställer att parterna verkligen är ägare till sina respektive offentliga nycklar [17]. På så sätt försäkras parterna att innehållet i meddelanden inte har manipulerats under utbytet (se [MITM](#2111-man-in-middle-attack-mitm)).

#### 2.1.4 Kryptering, lösenordhashing och salting

**Kryptering** är konverteringen av data till ett format som inte kan avkodas utan en hemlig nyckel. Det gör det alltså möjligt att hålla information hemlig från de utan nyckeln [18].

**Hashing** omvandlar data till en bestämd längd av bokstäver och siffror. Denna process <u>kan inte</u> bli ogjord och hashad data kan inte användas för att avkryptera originalinnehållet [18].
Hashing är ett populärt sätt att förvara **lösenord** i databaser pga envägsprocessen. När en användare loggar in, hashas det angivna lösenordet och det jämförs med hashen i databasen.
Stark lösenordspolicy som långa komplex tecken och best practice hashing (med system som t.ex Argon2id) gör det nästan omöjligt att komma åt datan [19].

Hackers kan använda sig av så kallade _Rainbow Table_ [20], en tabell där fördefinerade lösenord har hashats enligt bestämda metoder, och försöka hitta ett hashat lösenord som överensstämmer med det i den attackerade databasen. Om ett hittas, kan anfallaren enkelt kolla upp klartextversionen av hashet (dvs användarens lösenord). Som en ytterligare säkerhetsåtgärd mot detta kan **salting** implementeras.
Salting lägger till en slumpmässig vald radtecken <u>unik för varje användare</u> till lösenordet vilket gör det ännu svårare för hackers att komma åt datan [18].

#### 2.1.4 Inputvalidering

#### 2.1.5 HTTP- och API-säkerhet (CORS)

#### 2.1.6 STRIDE (check OWASP)

#### 2.1.7 Filhantering och validering

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

[1]: Bandar Alotaibi, “Cybersecurity Attacks and Detection Methods in Web 3.0 Technology: A Review”, Sensors, Januari 2025. Available: https://www.mdpi.com/1424-8220/25/2/342

[2]: “Antitrust cases against Google by the European Union”, Wikipedia, Accessed Maj 2026. Available: https://en.wikipedia.org/wiki/Antitrust_cases_against_Google_by_the_European_Union

[3]: Vatsala Gaur, “How the EU is taking on Big Tech: Meta, Apple, Google, face heightened scrutiny, penalties”, Invezz, December 2025. Available: https://invezz.com/news/2025/12/04/how-the-eu-is-taking-on-big-tech-meta-apple-google-face-heightened-scrutiny-penalties/

[4]: “Digital Sovereignty in Tension: U.S. Pushback Against the EU’s Digital Services Act”, The Cyber Institute, Augusti 2025. Available: https://www.cyber-institute.org/post/digital-sovereignty-in-tension-u-s-pushback-against-the-eu-s-digital-services-act

[5]: Clare Duffy, “Trump administration’s vision of US tech dominance is colliding with Europe ”, CNN, Januari 2026. Available: https://edition.cnn.com/2026/01/12/tech/us-eu-tech-regulation-fight-explained

[6]: Marius Laffont, “Pour son indépendance numérique, l'État français souhaite passer à Linux”, RFI, April 2026. Available: https://www.rfi.fr/fr/france/20260410-pour-son-ind%C3%A9pendance-num%C3%A9rique-l-%C3%A9tat-fran%C3%A7ais-souhaite-passer-%C3%A0-linux

[7]: OWASP about page, Accessed: Maj 2026. Available: https://owasp.org/about/

[8]: OWASP Top 10 threats, Accessed: Januri -Juni 2026. Available: https://owasp.org/www-project-top-ten/

[9]: Lorikeet Security Team, "PCI DSS Requirement 6: Secure Development Practices Your QSA Will Scrutinize", Accessed: Maj 2026. Available: https://lorikeetsecurity.com/blog/pci-dss-requirement-6-secure-development

[10]: NordVPN, Accessed May 2026. Available: https://nordvpn.com/sv/blog/mitm-attack/

[11]: Wikipedia, Accessed May 2026. Available: https://en.wikipedia.org/wiki/Man-in-the-middle_attack

[12]: JWT HANDBOOK, Sebastiàn Peyrott, Publisher; Auth0 by Okta. Available: https://www.jwt.io

[13]: ItSecurityDemand, Accessed: May 2026. Available: https://www.itsecuritydemand.com/insights/security/code-injection-attacks-a-guide-to-security-prevention/

[14]: Wikipedia, Accessed May 2026. Available: https://sv.wikipedia.org/wiki/Brute_force

[15]: Tom Krantz & Alexandra Jonker, IBM, Accessed: May 2026. Accessible: https://www.ibm.com/think/topics/brute-force-attack

[16]: Mozilla, Accessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/CSRF

[17]: Asymmetrisk kryptografi: Vad är det och varför använda det inom cybersäkerhet?, HMS, Ewon Ewon, översatt till svenska, Accessed: May 2026. Available: https://www.hms-networks.com/sv/industrial-iot-blog/blogpost/hms-blog/2024/01/08/asymmetric-cryptography-what-is-it-and-why-use-it-in-cyber-security

[18]: Encryption vs Hashing vs Salting, Geeks For Geeks, July 2025. Available: https://www.geeksforgeeks.org/computer-networks/encryption-vs-hashing-vs-salting/

[19]: Password Storage Cheat Sheet, OWASP, Accessed: May 2026. Available: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

[20]: Understanding Rainbow Table Attack, Geeks For Geeks, Feb 2023. Available: https://www.geeksforgeeks.org/ethical-hacking/understanding-rainbow-table-attack/

[20]:

[20]:

[20]:

[20]:

[20]:

[20]:

[20]:

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
