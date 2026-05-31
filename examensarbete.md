# Säkringen av EFBox API:et enligt OWASP Top 10 2025

## Sammanfattning (Abstract)

Many systems are developed as proof-of-concept projects without security as a primary focus, leaving them vulnerable when deployed. This study examines how an existing Java Spring Boot REST API, EFBox originally created as a school project, can be systematically secured using OWASP Top 10 2025 as a reference framework, combining theoretical research with practical implementation.

Security vulnerabilities were identified across nine OWASP categories and subsequently remediated. Verification was performed using OWASP ZAP, SonarQube, OSV dependency scanning and manual testing. Results demonstrate that securing an existing API requires extending its architecture rather than modifying it, and that systematic iteration is essential.

The study contributes a concrete methodology for security analysis of existing REST APIs, an area where comparable studies are scarce.

**Keywords:** REST API, OWASP Top 10, web security, Spring Boot, threat modeling

---

## Innehållsförteckning

- [Säkringen av EFBox API:et enligt OWASP Top 10 2025](#säkringen-av-efbox-apiet-enligt-owasp-top-10-2025)
  - [Sammanfattning (Abstract)](#sammanfattning-abstract)
  - [Innehållsförteckning](#innehållsförteckning)
  - [Förkortningar och Begrepp](#förkortningar-och-begrepp)
  - [1. Inledning](#1-inledning)
    - [1.1 Bakgrund](#11-bakgrund)
    - [1.2 Syfte](#12-syfte)
    - [1.3 Frågeställningar](#13-frågeställningar)
    - [1.4 Avgränsningar](#14-avgränsningar)
    - [1.5 Metodöversikt](#15-metodöversikt)
  - [2. Teoretisk Grund och Relaterat Arbete](#2-teoretisk-grund-och-relaterat-arbete)
    - [2.1 Tekniska Koncept](#21-tekniska-koncept)
      - [2.1.1 Olika attacker mot API](#211-olika-attacker-mot-api)
        - [2.1.1.1 Man in middle attack (MITM)](#2111-man-in-middle-attack-mitm)
        - [2.1.1.2 Code-Injection](#2112-code-injection)
        - [2.1.1.3 Brute force attack](#2113-brute-force-attack)
        - [2.1.1.4 Cross Site Request Forgery](#2114-cross-site-request-forgery)
      - [2.1.2 Autentisering och auktorisering JWT](#212-autentisering-och-auktorisering-jwt)
      - [2.1.3 Asymmetrisk kryptografi](#213-asymmetrisk-kryptografi)
      - [2.1.4 Kryptering, lösenordhashing och salting](#214-kryptering-lösenordhashing-och-salting)
      - [2.1.5 Inputvalidering](#215-inputvalidering)
      - [2.1.6 Filvalidering](#216-filvalidering)
      - [2.1.7 HTTP- och API-säkerhet (CORS)](#217-http--och-api-säkerhet-cors)
        - [2.1.7.1 Grundläggande om HTTP](#2171-grundläggande-om-http)
      - [2.1.7.2 HTTPS](#2172-https)
      - [2.1.7.3 Cross-origin resource sharing (CORS) - API: säkerhet](#2173-cross-origin-resource-sharing-cors---api-säkerhet)
      - [2.1.8 Hotmodellering med STRIDE](#218-hotmodellering-med-stride)
      - [2.1.9 OWASP Top 10 hot 2025](#219-owasp-top-10-hot-2025)
        - [2.1.9.1 Broken Access Control (Bristfällig åtkomstkontroll)](#2191-broken-access-control-bristfällig-åtkomstkontroll)
        - [2.1.9.2 Security Misconfiguration (Felaktig säkerhetskonfiguration)](#2192-security-misconfiguration-felaktig-säkerhetskonfiguration)
        - [2.1.9.3 Software Supply Chain Failures (Brister i Mjukvarans Leveranskedja)](#2193-software-supply-chain-failures-brister-i-mjukvarans-leveranskedja)
        - [2.1.9.4 Cryptographic Failures (Kryptografibrister)](#2194-cryptographic-failures-kryptografibrister)
        - [2.1.9.5 Injection (Injektionsattacker)](#2195-injection-injektionsattacker)
        - [2.1.9.6 Insecure Design (Osäker design)](#2196-insecure-design-osäker-design)
        - [2.1.9.7 Authentication Failures (Autentiseringsbrister)](#2197-authentication-failures-autentiseringsbrister)
        - [2.1.9.8 Software och Data Integrity Failures (Brister i mjukvaru- och dataintegritet)](#2198-software-och-data-integrity-failures-brister-i-mjukvaru--och-dataintegritet)
        - [2.1.9.9 Security Logging and Alerting Failures (Brister i säkerhetsloggning och larmhantering)](#2199-security-logging-and-alerting-failures-brister-i-säkerhetsloggning-och-larmhantering)
        - [2.1.9.10 Mishandling Of exceptional Conditions (Felhantering av undantagstillstånd)](#21910-mishandling-of-exceptional-conditions-felhantering-av-undantagstillstånd)
    - [2.2 Befintlig Forskning och Lösningar](#22-befintlig-forskning-och-lösningar)
    - [2.3 Teknisk/Teoretisk Jämförelse](#23-tekniskteoretisk-jämförelse)
  - [3. Metod och Genomförande](#3-metod-och-genomförande)
    - [3.1 Övergripande Arbetsgång](#31-övergripande-arbetsgång)
    - [3.2 Verktyg och Tekniker](#32-verktyg-och-tekniker)
    - [3.3 Datainsamling och Analys](#33-datainsamling-och-analys)
      - [3.3.1 Kartläggning av EFbox, struktur och end-points](#331-kartläggning-av-efbox-struktur-och-end-points)
        - [3.3.1.1 Översikt](#3311-översikt)
        - [3.3.1.2 Struktur](#3312-struktur)
      - [3.3.1.3 End-points](#3313-end-points)
      - [3.3.2 Hotmodellering av EFbox](#332-hotmodellering-av-efbox)
      - [3.3.3 Automatisk och Manuell kodgranskning av EFbox ur ett säkerhetsperspektiv](#333-automatisk-och-manuell-kodgranskning-av-efbox-ur-ett-säkerhetsperspektiv)
        - [3.3.3.1 Analys med SonarQube](#3331-analys-med-sonarqube)
        - [3.3.3.2 Manuel kodgranskning ur ett säkerhetsperspektiv](#3332-manuel-kodgranskning-ur-ett-säkerhetsperspektiv)
        - [3.3.3.3 Säkerhetstestning av EFBox API:et med OWASP ZAP](#3333-säkerhetstestning-av-efbox-apiet-med-owasp-zap)
        - [3.3.3.3.1 Analys av ZAP-säkerhetsrapporten](#33331-analys-av-zap-säkerhetsrapporten)
        - [3.3.3.3.1.1 SQL Injection (High Risk, Medium confidence)](#333311-sql-injection-high-risk-medium-confidence)
        - [3.3.3.3.1.2 Buffer Overflow (Medium Risk, Medium confidence) :](#333312-buffer-overflow-medium-risk-medium-confidence-)
        - [3.3.3.3.1.3 Application Error Disclosure (Low Risk, Medium confidence):](#333313-application-error-disclosure-low-risk-medium-confidence)
        - [3.3.3.3.1.4 Insights (information som kan vara relevant):](#333314-insights-information-som-kan-vara-relevant)
        - [3.3.3.3.2 Sammanfattning](#33332-sammanfattning)
    - [3.4 Planering av åtgärder](#34-planering-av-åtgärder)
    - [3.5 Kvalitetssäkring](#35-kvalitetssäkring)
      - [3.5.1 Metodkvalitet och tillförlitlighet](#351-metodkvalitet-och-tillförlitlighet)
      - [3.5.2 Validering av resultat](#352-validering-av-resultat)
      - [3.5.3 Hantering av bias eller fel](#353-hantering-av-bias-eller-fel)
  - [4. Resultat](#4-resultat)
    - [4.1 Huvudresultat](#41-huvudresultat)
      - [4.1.1 Det nya EFBox-API:et](#411-det-nya-efbox-apiet)
    - [4.1.2 OSV-rapport](#412-osv-rapport)
      - [4.1.3 ZAP-säkerhetsrapport (efter åtgärder)](#413-zap-säkerhetsrapport-efter-åtgärder)
      - [4.1.4 Manuellt testande via Postman](#414-manuellt-testande-via-postman)
    - [4.2 Detaljerade fynd](#42-detaljerade-fynd)
      - [4.2.1 Detaljerade fynd per OWASP-kategori](#421-detaljerade-fynd-per-owasp-kategori)
      - [4.2.2 Kontroll med hotmodelleringen](#422-kontroll-med-hotmodelleringen)
    - [4.3 Oväntade Resultat](#43-oväntade-resultat)
  - [5. Diskussion](#5-diskussion)
    - [5.1 Analys av Resultat](#51-analys-av-resultat)
      - [5.1.1. Vilka säkerhetsbrister identifieras i EFbox REST API utifrån OWASP Top 10:2025?](#511-vilka-säkerhetsbrister-identifieras-i-efbox-rest-api-utifrån-owasp-top-102025)
      - [5.1.2. Hur kan de identifierade bristerna åtgärdas inom ramen för det befintliga systemets arkitektur?](#512-hur-kan-de-identifierade-bristerna-åtgärdas-inom-ramen-för-det-befintliga-systemets-arkitektur)
      - [5.1.3. Hur verifieras att implementerade åtgärder är effektiva?](#513-hur-verifieras-att-implementerade-åtgärder-är-effektiva)
    - [5.2 Reflektion över Metod](#52-reflektion-över-metod)
    - [5.3 Begränsningar och Kritisk Granskning](#53-begränsningar-och-kritisk-granskning)
    - [5.4 Bredare Perspektiv](#54-bredare-perspektiv)
  - [6. Slutsatser](#6-slutsatser)
    - [6.1 Huvudslutsatser](#61-huvudslutsatser)
    - [6.2 Bidrag och Betydelse](#62-bidrag-och-betydelse)
    - [6.3 Framtida Arbete](#63-framtida-arbete)
  - [Bilagor](#bilagor)
  - [Referenser](#referenser)

## Förkortningar och Begrepp

| Term/Förkortning                | Förklaring                                                                                                                                                                                        |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| API                             | Application Programming Interface - Gränssnitt för kommunikation mellan mjukvarusystem                                                                                                            |
| Backend                         | Basbearbetningen (ofta på servernivå)                                                                                                                                                             |
| Cookie                          | Kaka på svenska (efter sagan om Hans och Greta) är information som sparas i webbläsaren, oftast används det för prestanda förbättringar och/eller för att lagra användarinformation (t.ex en JWT) |
| CORS                            | Cross-origin resource sharing, en teknik som begränsar åtkomst till websidor från specifierade domäner                                                                                            |
| CRUD                            | Create Read Update Delete (Skapa, Läsa, Ändra, Radera), ett begrepp som beskriver möjlig hantering av data                                                                                        |
| CSP                             | Content-Security-Policy, en response header som definierar vilka resurser en webbläsare få använda (ofta en servers ursprung)                                                                     |
| dependency                      | beroende eller avhängighet, inom programmering syftas det på program eller ramverk som en applikation är beroende av                                                                              |
| DFD                             | Data Flow Diagram (Dataflödesdiagram)                                                                                                                                                             |
| ECDSA                           | Elliptisk kurva digital signaturalgoritm, en av de mer komplexa offentliga nyckelkrypteringsalgoritmer                                                                                            |
| Dependency (inom programmering) | Beroende på svenska, syftar på mjukvara som en applikation är beroende av                                                                                                                         |
| EU                              | Europeiska Unionen                                                                                                                                                                                |
| Frontend                        | Användargränssnittsorienterad bearbetning (webbsida, mobilapplikationer mm)                                                                                                                       |
| GDPR                            | General Data Protection Regulation                                                                                                                                                                |
| Git                             | Versionhateringsprogram                                                                                                                                                                           |
| GitHub                          | Ett kodbibliotek för olika programmeringsprojekt där innehåll kan hämtas eller laddas upp                                                                                                         |
| Header                          | "Huvud" på svenska, början på ett meddelande inom datateknik som innehåller metadata om meddelandet (hur den ska tolkas)                                                                          |
| HTML                            | HyperText Markup Language, programmeringsspråket som lägger grunden till webbsidor                                                                                                                |
| HTTP                            | Hypertext Transfer Protocol, ett kommunikationsprotokoll som används för att överföra information på internet                                                                                     |
| HTTPS                           | Hypertext Transfer Protocol Secure, protokoll för krypterad transport av data för HTTP-protokollet                                                                                                |
| IDE                             | Integrated Development Environment, miljön där utvecklare skriver sin kod                                                                                                                         |
| IntelliJ                        | En IDE utvecklad av JetBrain                                                                                                                                                                      |
| ISO                             | International Organization for Standardization                                                                                                                                                    |
| Java                            | Ett av de vanligaste programmeringspråken                                                                                                                                                         |
| JavaScript                      | Ett programmeringsspråk som ger interaktivitet och dynamik till webbsidor, främst när de körs i webbläsaren                                                                                       |
| Json                            | JavaScript Object Notation ett kompakt, textbaserat format som används för att utbyta data                                                                                                        |
| JWT                             | Json Web Token, ett standardiserat sätt att överföra information som Json-objekt                                                                                                                  |
| MIME-type                       | Multipurpose Internet Mail Extensions Type, används för att ange filtyp. Namnet härstammar från dess ursprungliga användning för att identidera emails innehåll och bifogade filer.               |
| Open-source                     | Öppen källkod som inte är proprietärt, dvs illgänglig att använda, läsa, modifiera och vidaredistribuera för den som vill                                                                         |
| MFA                             | Multi-Factor Authentication                                                                                                                                                                       |
| NIST                            | National Institute of Standards and Technology                                                                                                                                                    |
| ORM                             | Object Relational Mapping, en programmeringsteknik som tjänar till att transformera data som används i ett objektorienterade programmeringsspråk eller programmet och relationsdatabasen          |
| OWASP                           | Open Worldwide Application Security Project                                                                                                                                                       |
| PCI DSS                         | Payment Card Industry Data Security Standard                                                                                                                                                      |
| Plug-in                         | Tilläggsprogram om inte körs fristående utan installeras som ett tillägg i ett annat program                                                                                                      |
| Postman                         | Ett verktyg för utvecklare för att testa API                                                                                                                                                      |
| Pull request (PR)               | begäranden för att ändra, granska och slå samman kod i en Git-lagringsplats                                                                                                                       |
| RSA                             | Ett krypteringsalgoritm döpt uppkallad efter dess skapare Rivest, Shamir och Adleman. Systemet kräver en nyckel för kryptering och en annan för avkryptering                                      |
| Repository                      | Kodbibliotek                                                                                                                                                                                      |
| REST                            | Representational State Transfer - Arkitekturstil för webbaserade API:er                                                                                                                           |
| Spring Boot                     | Ett open-source Java-ramverk som förenklar utvecklingen av webbapplikationer genom att erbjuda en snabb och enkel konfiguration                                                                   |
| Statefull                       | Syftar på att information (eller _state_) sparas för kommunikationen för snabbare åtkomst                                                                                                         |
| Stateless                       | Syftar på att ingen information (eller _state_) sparas för kommunikationen, all information relevant för informationsutbyttet måste skickas med varje meddelande                                  |
| STRIDE                          | Spoofing-Tampering-Repudiation-Information-Disclosure-Denial of service-Elevation of privileges, ett hotmodelleringsramverk utvecklad av Microsoft                                                |
| SQL                             | Structured Query Language, ett programmeringsspråk som används för hantera och manipulera relationsdatabaser                                                                                      |
| TLS                             | Transport Layer Security, ett kryp­te­rings­pro­to­koll som sä­ker­stäl­ler säker da­taö­ver­fö­ring på internet.                                                                                 |
| URI                             | Uniform Resource Identifier, en teckensträng som används för att identifiera en resurs. URI kan användas för att lokalisera en webbplats, fil eller en specifik del av data                       |
| URL                             | Uniform Resource Locator, är den teckensträng som identifierar en viss resurs på internet, till exempel en webbsida. I folkmun kallas URL i för "webbadress"                                      |
| white hats                      | hackers som angripper i syftet att dela med sig av sina fynder till utvecklarna av en applikation                                                                                                 |
| XML                             | Extensible Markup Language                                                                                                                                                                        |
| XSS                             | [Cross Site Scripting](#2112)                                                                                                                                                                     |
| ZAP                             | Zed Attack Proxy, en _open-source_ programvara som används i samband säkerhetstestning av applikationer                                                                                           |

---

## 1. Inledning

### 1.1 Bakgrund

Internet anses ha genomgått tre perioder [^1] sen dess specifikation i 1989.
Man pratar om **Web 1.0** där användarna kunde, för det mesta, bara söka och läsa innehåll online. Kommunikationen skulle, förenklat, kunna beskrivas ensidig och användarinputs var begränsade.

Sedan 2000-talet tog **Web 2.0** över världen med interaktiva tjänster och sociala medier. Användarna kan nu skicka information på ett enkelt sätt. Det fanns redan säkerhetsproblem under Web 1.0, men nu behöver online-tjänster kunna hantera, på ett säkert sätt, information och kommando som skickas till servrerna. Säkerhet tas på desto större allvar då många lagrar personlig information online som måste skyddas på ett säkert sätt. Web 2.0 gjorde det möjligt för företag som Meta, Google, Amazon, Twitter/X och andra att bli värdsledande och implementera ett affärssystem där användardatan är en produkt som säljs i marknadsföringssyften. Hackerkulturen fortsatte samtidigt att utvecklas och attackerna blev alltmer avancerade. Man-in-the-middle-attacks, brute force attacks, Denial Of Service mm är hot som alla online-leverantörer måste ta i beaktning. Andra aktörer som statligt sponsrade hacker gör det ännu svårare att skydda informationen online.

På senare år har misstron mot internetjättarnas sätt att hantera vår personliga information tilltagit. De stora företagens sätt att hantera vår data ifrågasätts och i Europa tar E.U fram ett regelverk för att skydda användarna; GDPR, som ska skydda både lagring och överföring av data online. Samtidigt försöker många att decentralisera sig från de stora nätverken (statliga eller privata) genom att förlita sig mer på _peer-to-peer_ filosofin. Detta anses vara **Web 3.0**, ett decentraliserat internet.

Samtidigt i Europa har EU startat många konkurrensmål mot IT-jättarna [^2][^3], de politiska spänningarna mellan USA (där de flesta internationella tjänsteleverantörer finns) och Europa [^4][^5] har bidragit till att vissa EU-länder börjar leta efter alternativa tjänster, som t.ex Frankrikes mål att ersätta Microsoft tjänster med Linux baserade system för att uppnå digitalt självständighet [^6].

Sammanfattningsvis genomgår internet en ny era där säkerheten kan komma att läggas på mindre utvecklingsteam, en ny marknad kommer att öppnas i samband med att EU-regionen minskar sitt beroende av utomeuropeiska tjänster. Internetanvändare kan förvänta sig att nya tjänster publiceras online men frågan om datasäkerhet kommer att kvarstå: _hur säker är min information online?_

Denna studie avser att studera hur man kan göra en applikation säker baserat på OWASP Top 10 hot.

Open Worldwide Application Security Project (OWASP) är en internationell non-profit organisation med målet att förbättra säkerheten i programvaror och webbtjänster [^7]. Organisationen är öppen och transparent – all dokumentation, verktyg och forskning publiceras fritt tillgängligt online.
Ett av organisationens mest kända bidrag är OWASP Top 10, en lista över de tio mest förekommande och kritiska säkerhetshoten i webbapplikationer. Listan baseras på data insamlad från hundratals organisationer världen över och uppdateras regelbundet för att spegla det aktuella hotlandskapet. Den senaste versionen publicerades 2025 [^8].
OWASP Top 10 används globalt som referensram inom websäkerhet – både av enskilda utvecklare och av stora organisationer. Listan fungerar som ett gemensamt språk mellan utvecklare, säkerhetsexperter och verksamheter för att identifiera, prioritera och åtgärda säkerhetsrisker. Flera regulatoriska ramverk, däribland PCI DSS (betalkortsstandarden), refererar explicit till OWASP Top 10 som en del av sina krav [^9].
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
- Hänsyn till GDPR tas inte
- Penetrationstestning mot en live-miljö ingår inte utan testning sker i lokal utvecklingsmiljö
- Projektet inkluderar inte prestandaoptimering eller funktionsutveckling utanför säkerhetsåtgärder
- Applikationen är ej avsedd för produktionsdrift inom ramen för detta projekt

_\*A06 - Insecure design är för subjektivt för att kunna bedömas på ett empiriskt sätt. Därför ignoreras den punkten i denna studie._

### 1.5 Metodöversikt

Målet med detta arbete är att åstadkomma en **kombinerad teoretisk och utvecklingsstudie** där teori och praktik sammanstrålar.

I första stadiet studeras hoten listade i OWASP Top 10 följd av en analys av EFBox-API:et för att identifiera dess svaghet. Nästa steg är att åtgärda dessa brister på ett effektivt sätt dvs genom att lösa flera stycken på en gång (ex: log och felhantering är vanligtvis närbesläktade).
Sista steg är att återanalysera API:et för att se om åtgärdena är effektiva.

De verktyg som används i denna studie är:

- IntelliJ (IDE)
- Java version 23 (programmeringspråk)
- Postman
- ZAP
- SonarQube för IDE (en plug-in för IDE:er för kodkvalitetsgranskning och olika komplexitetsmätningar)
- Claude AI kommer också att användas för kodgranskning (eftersom arbetet bedrivs på egen hand) <u>inte för att driva studien</u>.
- Git Hub för att lagra repositoryn

---

## 2. Teoretisk Grund och Relaterat Arbete

### 2.1 Tekniska Koncept

#### 2.1.1 Olika attacker mot API

##### 2.1.1.1 Man in middle attack (MITM)

En Man-in-the-middle-attack (MITM), "mannen i mitten" på svenska, är en cyberattack där angriparen i hemlighet fångar upp och vidarebefordrar meddelanden mellan två parter som tror att de kommunicerar direkt med varandra. Genom att positionera sig som mellanhand kan anfallaren läsa av och kontrollera informationsflödet. Detta gör det möjligt att skicka egen data eller kod [^10] [^11] [^12]. En jämförelse skulle vara att man skickar en beställning till ett företag med posten. Under postgången får en tredje part tag på beställningsformulär, ändrar dess innehåll (kanske ändra mottagaradressen till sitt eget) och skickar det vidare.

<u>Skyddas med hjälp utav:</u> HTTPS/TLS (kryptering av dataflödet)

##### 2.1.1.2 Code-Injection

En attack där hackern _injicerar_ sin egen kod i en applikation som körs för att ändra dess beteende. En vanlig injection är så kallade **SQL-injection** där användarinputsfält används för att anropa databasen direkt. T.ex kan anfallaren försöka skriva ett SQL-kommando i ett textinput\* och läsa av svaret från servern om denna inte är skyddad mot detta. Det är även möjligt att ändra data i databasen (som att göra sitt eget konto till admin).
**Cross-site scripting (XSS)** är också en vanlig form av injection där kod injiceras med hjälp av HTML- eller javascriptkod [^12] [^13].

<u>Skyddas med hjälp utav:</u> inputvalidering (kontroll att inga kommando injiceras)

_*Förenklad exempel: 'SELECT * FROM users;' skulle kunna ge all användardata från serverns databas. I verklighet skulle en anfallare använda sig av sk. or-statements som "' OR '1'=1" som betyder: "om det inte funkar: ger mig allt"._

##### 2.1.1.3 Brute force attack

Brute force (engelska för råstyrka) är en metod för att hitta exempelvis lösenord genom att pröva alla möjliga kombinationer. Termen brute force syftar oftast på att hitta lösenord och nycklar [^14]. Om ett lösenord är enkelt går attack snabbare medan väldigt komplicerade lösenord tar längre tid att hitta. Detta kan jämföras med ett kombinationslås med bara tre siffror och ett med sex stycken där en tjuv testar alla möjliga kombinationer [^15].

<u>Skyddas med hjälp utav:</u> varningar vid upprepade misslyckade inloggningsförsök och låsning av berörda konton, starkt lösenordspolicy

##### 2.1.1.4 Cross Site Request Forgery

En hacker kan använda en annans rättigheter hos en tjänst och lura till sig en oönskad handling. Istället för att, på ett avancerat sätt, få tag på en användarens uppgifter kan man använda dess rättigheter direkt (_Request Forgery_). Detta kan ske via en extern länk från en annan sajt (_Cross Site_).

De flesta online-leverantörer delar ut en nyckel, så kallad token eller session-token, till sina användare som fungerar som ett tillstånd att använda tjänsten och komma åt sin data. En hacker kan lyckas nyttja nyckelns privilegier genom att få användaren att omedvetet utföra handlingar [^12].

Ett bra exempel är att anfallaren skickar en phishinglänk via email till offret som klickar på det. Länken skickar egentligen en förfrågan till offrets bank om att överföra pengar till anfallaren. Om offret är inloggad på bankens hemsida vid klickandet kan överföringen ske [^16].

Detta kan jämföras med att en tjuv lura ett offer att låsa upp hemmet för att kunna komma in.

<u>Skyddas med hjälp utav:</u> skydda känsliga handlingar som tex en banköverföring, header-verifiering (mm)

#### 2.1.2 Autentisering och auktorisering JWT

JSON Web Token (JWT) är en standard öppen för utvecklare som definierar ett säkert sätt att överföra information. Eftersom datan signeras digitalt är den tillförlitlig. Signaturen kan ske med en så kallad _secret_ ("hemlighet", använder sig av HMAC algoritmen) eller ett allmänt/privat nyckelpar med hjälp av RSA eller ECDSA.
Denna studie fokuserar på JWT-användningen inom signerade token och inte de krypterade token eftersom de signerade används för identifiering av användare. När en token är signerad får servern en bekräftelse på att avsändaren är den som signerat det.

JWT används vid **auktorisering** för att bekräfta att användaren har tillgång till serverns olika tjänster, i sådana fall skickas JWT:n med varje förfrågan till skyddade system.

```
//exempel på en JWT token, notera strukturen i tre delar med punkter (.)
 Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30
```

JWT-strukturen har följande mönster "xxxx.yyyy.zzzzz" där "xxxx" representerar en _header_ (metadata med tokentyp och signeringsalgoritm), "yyyy" är själva innehållet och "zzzz" står för signeringen.

JWT kan skickas i Authorization-headern och implementerar 'Bearer'-schemat (dvs att det står 'Bearer', bärare på svenska, före token med mellanslag). Alternativt kan JWT:n skickas som en cookie men kräver då att CORS konfigureras för detta [^12].

#### 2.1.3 Asymmetrisk kryptografi

Vid asymmetrisk kryptografi används ett nyckelpar – en offentlig och en privat nyckel. Den offentliga nyckeln kan delas fritt medan den privata aldrig lämnar ägaren.
Det som signeras med den privata nyckeln kan verifieras av vem som helst med den offentliga nyckeln. Den offentliga nyckeln kan vara identifierad av en certifikatutfärdare (CA), vilket säkerställer att parterna verkligen är ägare till sina respektive offentliga nycklar [^17]. På så sätt försäkras parterna att innehållet i meddelanden inte har manipulerats under utbytet (se [MITM](#2111-man-in-middle-attack-mitm)).

#### 2.1.4 Kryptering, lösenordhashing och salting

**Kryptering** är konverteringen av data till ett format som inte kan avkodas utan en hemlig nyckel. Det gör det alltså möjligt att hålla information hemlig från de utan nyckeln [^18].

**Hashing** omvandlar data till en bestämd längd av bokstäver och siffror. Denna process <u>kan inte</u> bli ogjord och hashad data kan inte användas för att avkryptera originalinnehållet [^18].
Hashing är ett populärt sätt att förvara **lösenord** i databaser pga envägsprocessen. När en användare loggar in, hashas det angivna lösenordet och det jämförs med hashen i databasen.
Stark lösenordspolicy som långa komplex tecken och best practice hashing (med system som t.ex Argon2id) gör det nästan omöjligt att komma åt datan [^19].

Hackers kan använda sig av så kallade _Rainbow Table_ [^20], en tabell där fördefinerade lösenord har hashats enligt bestämda metoder, och försöka hitta ett hashat lösenord som överensstämmer med det i den attackerade databasen. Om ett hittas, kan anfallaren enkelt kolla upp klartextversionen av hashet (dvs användarens lösenord). Som en ytterligare säkerhetsåtgärd mot detta kan **salting** implementeras.
Salting lägger till en slumpmässig vald radtecken <u>unik för varje användare</u> till lösenordet vilket gör det ännu svårare för hackers att komma åt datan [^18].

#### 2.1.5 Inputvalidering

**Inputvalidering** är kontrollen av data som skickas direkt av användaren, som t.ex tecken i ett textflält på en hemsida. Valideringen kontrollerar även Json-objekt som skickas från clienten [^21].

Inputvalidering uppfyller två syfte:

- att säkerställa att data som sparas inte är korrupt eller felformaterat så att dess användning senare orsakar fel eller oväntat beteende,
- att <u>bidra</u> som skydd mot [code injection attacker](#2112-code-injection), förutsagt att det implementeras korrekt.

Inputvalidering kan ske på olika sätt, denna studie kommer att fokusera på REGEX-validering: vissa strängkombinationer, längd eller tecken förbjuds.

#### 2.1.6 Filvalidering

Eftersom en filhanteringstjänst studeras i denna studie måste ansträngningar läggas på filvalidering. Hackers kan ladda upp filer som antingen orsakar skador på servern (t.ex enorma filer eller filer som innehåller farlig kod) eller används för sekundära attacker som t.ex phishing.

Filvalidering liknar delvis inputvalidering då man försäkrar sig att själva filnamnet inte är farligt för systemet (kanske innehåller den systemrelaterade tecken som semi-colon ( ; )), för försök till injection.
Dess extension inspekteras också för att identifiera dess typ (efter punkten t.ex image<strong>.png</strong>). En vanlig filhanteringsapplikation skulle behöva tillåta många olika filtyper men denna studie är en _proof of concept_ och antalet tillåtna filer kommer att begränsas (se [Resultat](#411-det-nya-efbox-apiet)). Denna metod har dock en svaghet då man kan namnge en körbar fil med en annan extension. T.ex filen _virus.exe_ kan få sitt namn bytt till _flower.png_. I ett sådant fall skulle den körbara filen fortfarande ta sig igenom serverns försvar [^22].

I en förfrågans header kan nyckeln 'Content-type' hittas med en beskrivning av innehållet. Data i Content-type defineras av användaren och kan inte litas på men det kan agera som ett första steg i valideringen (dvs om Content-type värdet är bristfälligt så avslutas behandlingen av förfrågan)[^23].

De ovannämnda steg är viktiga men otillräckliga pga sina brister. Om en förfrågans header och filnamn valideras måste filinnehållet fortfarande valideras. OWASP anger inte specifikt vilket ramverk de föredrar men en länk till Dominique Righettos javaprojekt på GitHub ([länk till DocumentUpload-klassen](https://github.com/righettod/document-upload-protection/blob/master/src/main/java/eu/righettod/poc/web/DocumentUpload.java)) visar en sätt som använder I/O- (Input/Output) och NIO-importen (Non-blocking Input / output) med stöd av [Aspose](https://www.aspose.com/) API:et. Righettos i sitt projekt följer alla ovannämnda steg för att sen parsa filerna i en DocumentDetector subklass för att kontrollera deras filtyp.

I denna studie används Righettos approach som inspiration för implementationen av filvalidering i EFbox.

#### 2.1.7 HTTP- och API-säkerhet (CORS)

##### 2.1.7.1 Grundläggande om HTTP

HyperText Transfer Protocol (HTTP) är ett underliggande nätverksprotokoll för överföring av hypermedia dokument. I de flesta fallen sker denna överföring mellan en klient (t.ex en browser) och en server. HTTP-kommunikation är textbaserad, all information skickas i klar text, vilket gör det lättare för människor att läsa. HTTP är _stateless_, vilket innebär att all kommunikation sker utan minne av tidigare utbyte [^24]. Skulle en stateful kommunikation behövas kan _cookies_ användas. Dessa sparar relevant information (t.ex inloggningsinformation) i webbläsaren [^25] [^26].
Dessa egenskaper är fördelaktiga när man utvecklar REST-API:er eftersom dessa ska utvecklas stateless, i.e nödvändig information hämtas vid behov och sparas inte i det aktiva minne med vissa undantag för caching.

HTTP meddelande innehåller en så kallad _header_ som innehåller information om hur det ska läsas. Innehållet i en header varierar men kan hålla information om vilken sorts anrop som görs till servern (GET, POST, PUT mm.) eller om innehållet (text, fil mm).

```
// exempel på en header som skickas från en webbläsare (User-Agent)
// för att hämta (GET) en sida på Mozilla sajt (Host).
// Notera att ursprunget (Origin) nämns (las till för framtida förklaringar).
GET /static/client/styles-global.cac2f06a61438497.css HTTP/2
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:150.0) Gecko/20100101 Firefox/150.0
Accept: text/css,*/*;q=0.1
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br, zstd
Connection: keep-alive
Origin: https://foo.example
```

HTTP-headern är central för hur [CORS](#2153-cross-origin-resource-sharing-cors---api-säkerhet) fungerar.

#### 2.1.7.2 HTTPS

HTTPS (S för _secure_, säkrad) är ett säkrare transportprotokoll för HTTP-meddelande. En tredje part tillhandhåller ett undertecknat digitaltcertifikat som kontrolleras av klienten med hjälp av förinstallerade certifikat.
Med HTTPS skall förbindelsen inte kunna avlyssnas av tredje part och användaren skall kunna lita på att webbservern är densamma som den utger sig för att vara [^28].

#### 2.1.7.3 Cross-origin resource sharing (CORS) - API: säkerhet

Cross-origin resource sharing är ett HTTP-baserat system som gör det möjligt för en server att lista vilka domäner, schema eller port en webbläsare bör tillåta för att ladda resurser [^26].

Om ingen CORS-inställning defineras, accepteras enbart förfågningar från servern, sk _same-origin_. CORS gör det möjligt att tillåta förfrågningar från andra källor **på ett kontrollerat sätt**.

Webbläsaren gör en _preflight request_ (preflight: kontroll före flygning på svenska), dvs den granskar förfrågan, för att försäkra sig att servern kommer att tillåta den. I denna granskning skickas en OPTIONS-header som beskriver HTTP-metod (GET, POST, PUT, DEL mm) och den ursprungliga header för förfrågan.
Man kan säga att preflight request kontrollerar att förfrågan är giltig innan den skickas.

Enklare förfrågor behöver inte alltid trigga en preflight. Dessa defineras med headers med specifika HTTP-metod (GET, HEAD, POST), ett begränsat val av användardefinerade headers (ex: Accept, Accept-language mm) och enklare Content-type (ex: text eller formdata)[^27].

En viktig del av CORS är kontrollen av ursprunget för förfrågan, detta bl.a för att förebygga [Cross Site Request Forgery (CSRF)](#2114-cross-site-request-forgery). Vissa servrar tillåter bara same-origin (se ovan) men andra kan tillåta en lista med domäner vars förfrågningar accepteras. Om servern inte hanterar känslig information (t.ex [Dog API](https://dog.ceo/dog-api/)) kan alla ursprung tillåtas.

För att webbläsaren ska veta om förfrågan kommer att godkännas svarar servern med en _access-allow-origin_ header med värdet på det tillåtna ursprung:

```
//EXEMPLE 1:
access-control-allow-origin: * // alla kan anropa servern
//EXEMPEL 2:
Access-Control-Allow-Origin: https://foo.example //bara Foo Example kan anropa servern
Access-Control-Allow-Methods: POST, GET, OPTIONS //enbart dessa HTTP-request kan utföras
```

En felkonfigurerad CORS-inställning – som att tillåta alla ursprung (med '\*') för en tjänst som hanterar känslig information – kan exponera API:et för obehöriga förfrågningar. Korrekt CORS-konfiguration är därför en viktig säkerhetsåtgärd som analyseras vidare i denna studie. Vilket behandlas vidare under [Security Misconfiguration](#2172-security-misconfiguration-felaktig-säkerhetskonfiguration).

#### 2.1.8 Hotmodellering med STRIDE

Allt som kan störa en tjänst eller data den hanterar anses vara ett hot [^29].

Genom en systematisk och strukturerad process som **hotmodellering** kan man få inblick i säkerhetskarakteristikerna av en applikation. För att uppnå detta identifierar man de relevanta hot och responsen mot dessa [^29] [^30].

För att skapa en hotmodell kan man ställa sig fyra frågor [^30]:

1. Vad jobbar vi med?
2. Vad kan gå fel?
3. Vad ska vi göra åt det?
4. Är modelleringen tillräckligt effektiv?

**1. Vad jobbar vi med?**
För att definiera en hotmodell bör man identifiera följande i applikationen [^31]:

- Systemelement (tillgångar, komponenter)
- Dataflöden och interaktioner med tredje part
- Intressenter
- Hot
- Åtgärder mot hot
- Iterera

**2. Vad kan gå fel?**
Som hjälp för att svara på dessa frågor kan man använda sig av ramverk för att kategorisera hoten. Ett populärt sådant är STRIDE [^29] [^32]. Varje bokstav motsvarar en hotkategori (se tabell nedan). Ramverket underlättar kategoriseringen på ett systematiskt sätt när man ställer sig frågan "Vad kan gå fel?".

| Hotkategori             | Påverkan         | Exempel                                                                                 |
| ----------------------- | ---------------- | --------------------------------------------------------------------------------------- |
| Spoofing                | Autentisering    | Hackern får tag på en användarens JWT och identifierar sig som denna                    |
| Tampering               | Integritet       | Hackern missbrukar applikation för att genomföra oönskade updateringar i databasen      |
| Repudiation             | Logging          | Hackern manipulerar loggarna för att dölja sina spår                                    |
| Information Disclosure  | Konfidentialitet | Hackern får ut information om en användare från databasen                               |
| Denial Of Service       | Tillgänglighet   | Hackern låser ut en användare från tjänsten genom att utföra förmånga inloggningsförsök |
| Elevation of Privileges | Auktorisering    | Hackern manipulerar en JWT för att ändra sin roll till administratör                    |

Vanligtvis ordnas hoten efter produkten av hur sannolikt hotet är och dess påverkan. I denna studie kommer detta utföras i förminskad omfattning då hotmodellering är ett ämne i sig.

**3. Vad ska vi göra åt det?**
När hoten är identifierade ska responser utvecklas [^29]:

- Mitigera: förebygga att hotet kommer att genomföras
- Eliminera: Ta bort komponenten eller tjänsten som orsakar hotet
- Överför (Transfer): Flytta ansvaret till annan (ex: identifiering via OpenID eller lägg ansvar på kunden via lösenordspolicy)
- Acceptera: Beroende på den kommersiella modellen får tjänstleverantören acceptera hoten

**4. Är modelleringen tillräckligt effektiv?**
Hotmodellering måste granskas av alla berörda aktörer [^29]:

- Är det använda schemat representativt?
- Har alla hot identiferats?
- När mitigering implementerats, har risken minskats till acceptabla nivåer?
- Har modelleringen dokumenterats och är tillgänglig för det som behöver kan komma åt det?
- Kan mitigeringen testas?

**Hotmodellering i denna studie**:

För att lyckas skydda EFBox-applikationen på ett effektivt sätt kommer en förenklad version av hotmodellering, som täcker de mest kritiska hoten, skapas. Systemet kommer att inventeras och tillgängliga funktioner granskas för definera hoten med STRIDE. Men utan ett team av säkerhetsexperter bli en fullskalig hotmodellering orealistiskt och det kommer att implementeras som en _proof of concept_.

#### 2.1.9 OWASP Top 10 hot 2025

Open Worldwide Application Security Project (OWASP) publicerar regelbundet en lista på de mest kritiska säkerhetsrisker för webbapplikationer[^8]. Denna studie kommer att säkra EFBox-API enligt OWASP Top 10 med undantag för [Insecure Design](#2176). I detta avsnitt behandlas dessa hot och hur en utvecklare kan åtgärda dem.

##### 2.1.9.1 Broken Access Control (Bristfällig åtkomstkontroll)

Med Broken Access Control menar man åtkomst till tillstånd eller data bortom de tänkta av administratörerna. Utvecklare måste försäkra sig att tjänsterna är designade så att åtkomsten till funktioner, tillstånd eller data kontrolleras via kod eller CORS-konfiguration.
OWASP rekommenderar att kontrollen är tillståndbaserad (_permission based_) snarare än rollbaserad (_role based_) för att skydda sig mot en Elevation of Privilege attack. Med rollbaserade kontroller kan en anfallare som lyckats höja sin användarroll får han automatiskt alla privilegier kopplade till rollen. Med tillståndbaserade kontroller måste varje tillstånd listas ut och sen inskaffas.
Åtkomstkontroll kan implementeras till viss del i frontend men måste alltid implementeras i backend för att vara effektiv.
Några åtgärder för att förebygga Bristfällig Åtkomstkontroll:
| Åtgärd | Förklaring |
|--|--|
| Deny by default (Neka som standard) | Respekterar "least privilege"-filosofin fär tillstånd inte utfärdas om det inte behövs |
| Implementerar kontroll en gång | Det är bättre att implementera kontrollen en gång (i en klass till exempel) och använda den genom applikationen |
| Åtkomstkontroll före datahantering | Kontrollera att användaren äger data före CRUD-operationer|
| Logga fel och skapa varningar i åtkomstkonrollen | Genom att spara historik för åtkomstfel får administratören reda på vad som har hänt. Vid suspekta åtkomstfel (t.ex upprepade fel) bör varningar skickas till berörda aktörer |
| Kortvariga JWT | Långlivade JWT riskerar att missbrukas efter att användaren slutat använda tjänsten. Dessa bör har en kort giltighetstid med mekanismer för att förnyas på ett användarvänligt sätt. |
| Rate Limiting (Hastighetsbegränsning)| För att reglera antalet begäranden som en användare kan göra under en viss tid och på så sätt skyddas mot [Brute Force Attacks](#2113-brute-force-attack)|

Kodanalys av EFBox-API:et kommer att genomföras för bedöma hur lämplig Åtkomstkontrollen är.

##### 2.1.9.2 Security Misconfiguration (Felaktig säkerhetskonfiguration)

Om säkerheten i en applikation är felkonfigurerad (eller inte konfigurerad alls) kan den vara sårbar för attacker.
Förebyggande åtgärder kan vara:
| Åtgärd | Förklaring |
|--|--|
| Säkrad appliction-deployment | Säkerhetskonfigurationen bör standardiseras och automatiseras så att utvecklings-, test- och produktionsmiljöer alltid är identiskt konfigurerade. Detta minimerar risken för mänskliga fel och säkerställer att nya miljöer snabbt kan driftsättas med rätt säkerhetsinställningar (behandlas ej i denna studie). |
| Minimera attackyta | onödiga funktioner skall tas bort, likaså onödig dokumentation och ramverk |̣̣
| Implementation av Security headers | Säkerhetsheaders är instruktioner till webbläsaren för hur säkerhet skall hanteras. Dessa bör definieras med korrekta direktiv. Exempel för en filhanteringsapplikation skulle vara x-content-type (för att definiera MIME-typ) eller CSP |
| Kontroll vid uppgradering| Vissa uppgraderingar till projektets ramverk eller dependencies kan påverka säkerhetskonfigurationen eller radera den (se [Software Supply Chain Failures](#2173-software-supply-chain-failures-bristermjukvarans-leveranskedja)) |
| Logging| Som backup bör en centraliserad konfiguration implementeras för att fånga och varna vid ovanligt många felmeddelanden |
| CORS-konfiguration| I OWASP A02 Security Miconfiguration nämns inte CORS specifikt. CORS-konfiguration är dock nära relaterad till A02 och behandlas i denna studie inom ramen för säkerhetskonfiguration. CORS skall konfigureras för att användaren enbart kommer åt tillåtna tjänster (se även [Broken Access Control](#2171-broken-access-control-bristfällig-åtkomstkontroll)) eller att förfrågan tas emot från godkända domäner |

Eftersom EFBox var en _proof of concept_ förväntas säkerhetskonfigurationen behöva ses över och stärkas, i synnerhet CORS och logging. Spring Boot implementerar automatiskt vissa konfigurationer som t.ex _security headers_ [^33].

##### 2.1.9.3 Software Supply Chain Failures (Brister i Mjukvarans Leveranskedja)

I en applikation, kan dess dependencies och tredjepartsverktyg vara dess sårbarhet. Om själva depency har råkat ut för en lyckad attack kan hela leveranskedjan till system bli sårbar. Ett annat problem skulle kunna vara att själva dependency var designad i syfte att göra system sårbara.
Därför är det viktigt att dokumentera och övervaka levereranskedjan. Källor som _[National Vulnerability Database (NVD)](https://nvd.nist.gov/), [Common Vulnerability Exposure (CVE)](https://www.cve.org/)_ eller _[Open Source Vulnerability (OSV)](https://osv.dev/)_ kan hjälpa utvecklare att se om de komponenterna som används i en applikation har blivit sårbara.
Andra åtgärd för Brister i Mjukvarans Leveranskedja kan vara:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Övervakning av **hela** leveranskedja | Utvecklare bör inte bara övervaka applikationens egna dependency utan även deras dependency osv. |
| Minimera attackyta | Onödiga dependencies tas bort för att inte utsättas för risk i onödan |̣̣
| Regelbundna updateringar | Updateringar skall ske regelbundet för alla använda verktyg. Detta gäller också för IDE:er och andra utvecklingsverktyg |
| Välja en version | En specifik version av en dependency bör användas och updateras |
| Sandboxing | Uppdateringar eller nya dependencies bör testas i separata miljöer (sandboxes) |

Att övervaka Brister i Mjukvarans Leveranskedja är en komplicerad process med många fallgropar. OWASP har utvecklat verktyg som [OWASP Dependency Track](https://owasp.org/www-project-dependency-track/) för att hjälpa utvecklare.

För denna studie kommer Brister i Mjukvarans Leveranskedja att granskas med hjälp av [Open Source Vulnerabilities (OSV) verktyget](https://osv.dev/) och uppdateras vid behov. Valet av OSV över OWASP egna verktyg beror på snabbare implementering.

##### 2.1.9.4 Cryptographic Failures (Kryptografibrister)

Generellt bör all data som skickas vara krypterad. Detsamma gäller för _känslig_ data som lagras. Det europeiska GDPR har även specifika krav på vilken typ av data som bör sparas krypterat\*.

Kryptografibrister syftar på bristande eller icke-implementerad kryptering, vilket inkluderar brister i hantering av krypteringsnycklar.

Som all IT-teknologi är kryptografi ett område som råkar ständigt för en snabb utveckling samtidigt som olika aktörer uppnår samma utveckling på avkryptering. Kvantumdatokraft riskerar att göra traditionell kryptering irrelevant och framtiden kommer säkert att få se kvantumkryptografi bli en standard [^33]. Därför bör tjänster som behandlar känslig information vara välkonfigurerade för att hantera kryptering på ett säkert sätt.
| Åtgärd | Förklaring |
| ------ | ---------- |
| Klassifiera data | Data som sparas på servern bör bedömas för dess känslighet och eventuellt krypteras |
| Tillförlitliga, uppdaterade algoritm | Krypteringalgoritmerna som används skall komma från är tillförlitlig källa och hållas uppdaterade |
| Minimera attackyta | Spara inte känslig data i onödan utan radera det så fort den inte behövs mer|̣̣
| Lösenordskryptering | Lösenord bör sparas krypterad och hashade med hjälp av starka funktioner som Argon2. |

OWASP varnar även utvecklare för att redan börja förberreda sig för _the quantum age_ för att säkra sina system inför 2030.

I EFbox hanteras lösenord med BCrypt, vilket är en acceptabel lösning enligt OWASP om man följer deras guide för _legacy system_. Som en del av denna studie kommer lösenordskrypteringen att uppgraderas till Argon2id i enlighet med moderna rekommendationer. Övrig kryptografisk konfiguration, såsom hantering av JWT-hemligheten, kommer också att granskas.

_\*GDPR anses vara utanför denna studies omfattning_

##### 2.1.9.5 Injection (Injektionsattacker)

Applikationer ska vara säkrade mot [injektion](#2112-code-injection) för att olovlig kod inte körs. Injektion, [inputvalidering](#215-inputvalidering) och [filvalidering](#216-filvalidering) diskuteras tidigare och denna sektion kommer att fokusera på OWASP föreslagna åtgärd:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Använd ett säkert API | Undvik att använda tolken (interpretern) direkt, ett parametriserat gränssnitt eller ett ORM-verktyg som genererar SQL-frågor automatiskt bör användas istället |
| Om ovan inte är möjligt | Använd inputvalidering, detta är inte optimalt då många tjänster behöver använda speciella tecken. Se till att åtminstone förbjuda tecken kopplade till applikationens databaspråk (ex: "OR '1'=1;") |

Eftersom EFbox-API:Et använder JPA/Hibernate i Spring Boot (ett ORM-verktyg) kan det förväntas att den inte är så känslig mot klassiska injektionsattacker.

OWASP pratar inte om kodinjektion via filer men för denna studie anses det vara relevant och kommer att behandlas under granskning av EFbox.

##### 2.1.9.6 Insecure Design (Osäker design)

Säker design är en kultur och metodologi som konstant utvärdera hot och ser till att koden är designad på ett sätt som förebygger mot kända hot. Detta uppnås genom att titta på hur datan flödar genom applikationen. För objektorienterade programmeringspråk som Java handlar t.ex om segregering av klasser/objekt och hur de fördelas genom projektet. Ett verktyg för att kontrollera designen kan vara Unit Testing (som testar funktioner och komponenter) men även det behöver designas.
Säker design förblir en bedömningsfråga och är svår att sätta ett värde på. Därför kommer detta hot inte behandlas (se [Avgränsningar](#14-avgränsningar)).

##### 2.1.9.7 Authentication Failures (Autentiseringsbrister)

Autentiseringsbrister uppstår när en anfallare olovligt identifierar sig till tjänsten. Hackern kan ha listat ut en användares inloggningsdata via dataintrång online eller genom en [brute force attack](#2113). OWASP huvudfokus för detta hot ligger på följande punkter:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Stark lösenordspolicy | Lösenord skall vara komplicerade och svåra att gissa sig till. Användaren bör informeras om riskerna med svaga eller vanliga lösenord (de sistnämnda bör förbjudas helt) och förespråka användningen av lösenordshanterare |
| MFA | Med MFA kan användarens identitet dubbelkontrolleras via authenticators eller SMS |
| Rate Limiting | För att reglera antalet begäranden som en användare kan göra under en viss tid och på så sätt skyddas mot [Brute Force Attacks](#2113-brute-force-attack) (t.ex om de vanligaste lösenord testas med) |
| Hantering av session-identifiers | dessa bör inte vara i URL:n utan sparas i säkrade cookies. De bör invalideras vid utloggning och inaktivitet. |
| Standard inloggningsuppgifter | Applikationen bör inte byggas med standardiserade inloggningsuppgifter särskilt för administratörer som t.ex username:'admin and password:'admin'. |
| Behåll säkra lösenord | Om lösenordet inte har råkat ut för ett dataintrång bör inte användaren uppmanas till att byta. Om ett dataintrång misstänks skall lösenordet bytas **omedelbart**. |

Att försöka ta sig igenom inloggningen till en tjänst är ett av de lättare intrång en hacker kan försöka sig på. Genom att göra inloggningsprocessen säker kan en utvecklare hoppas på att trötta ut de mindre ambitösa aktörer och få applikationen att framstå som säkerhetsorienterad vilket kan även få mer avancerade hackers att ge upp.
EFBox implementerar kryptering med BCrypt med en lättare lösenordvalidering som sparar hashade lösenord. Detta kommer att granskas och uppdateras till en säkrare standard. MFA är önskvärt men kommer inte att implementeras pga tidsbrist.

##### 2.1.9.8 Software och Data Integrity Failures (Brister i mjukvaru- och dataintegritet)

För att undvika Brister i mjukvaru- och dataintegritet måste man säkerställa att den mjukvaran eller dependencies som används av applikationen kommer från pålitliga källor. Det är alltså inte bara mjukvaran i sig som kan vara farlig utan själva källan. Detta kan jämföras med en privatperson som köper ett känd och säkert kameraövervakningssystem men beställer det från en kriminell organisation som gömmer spionmjukvara i systemet.
Mjukvaran **och** källan måste vara pålitliga. OWASP föreslår följande:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Signaturer och liknande mekanismer | Dessa kontrollerar att mjukvaran kommer från rätt källa och att koden inte manipulerats i efterhand |
| Säkra dependency-bibliotek | _Dependency-libraries_ som Maven, NPM eller i EFBoxs fall Gradle bör kontrolleras för att säkerställa att de hämtar data från rätt källor |

EFBox är byggd på Gradle som beskriver en [process för att kontrollera dependencies](https://docs.gradle.org/current/userguide/dependency_verification.html) som löser många fallgropar nämnda ovan. Detta kommer att implementeras för applikationen. Källorna för andra verktyg (IDE, SonarQube mm) är också kontrollerade före projektets början.
OWASP nämner också olika steg för att skydda CI/CD-kedjan men detta anses vara _out of scope_ för denna studie.

##### 2.1.9.9 Security Logging and Alerting Failures (Brister i säkerhetsloggning och larmhantering)

Om inga varningar skickas eller ingen historik sparas, hur kan tjänsteleverantören då veta att systemet har utsatts för hot? Denna fråga summerar OWASP A09 Security Logging and Alerting Failures, då det fokuseras på behovet att informera ansvariga att något obehörigt har skett.
Om inte misstänksam aktivitet övervakas och sparas (genom logging) kan inte dessa upptäcktas. Om inga varningar skickas kan inte en snabb respons utföras.

Andra brister inom Brister i Säkerhetsloggning och Larmhantering kan vara att loggarna är tillgängliga för den som utför attacken eller att känslig information sparas i loggarna (se även [Broken Access Control](#2171-broken-access-control-bristfällig-åtkomstkontroll)). I detta fall kan loggarna manipuleras för att dölja spåren av attacken så att den förblir hemlig.
Att designa sådana system är svårt. För mycket information är opraktiskt att läsa igenom och om för många varningar skickas riskerar de att ignoreras.
Följande åtgärd föreslås av OWASP:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Övervaka känsliga system | alla fel vid login, åtkomstkontroll och inputvalidering måste loggas. Loggarna bör även sparas tillräckligt länge för att möjliggöra forensiska undersökningar om en attack sker |
| Läsbar logging format | Loggarna måste vara lätta att läsa |
| Säkra loggarna | Loggarna i sig bör vara skyddade mot injektionsattacker så att en hacker inte får loggarna att köra olovlig kod för att t.ex förfalska loggarna |
| Utveckla procedur för övervakning och varningar | När en mistänksam aktivitet upptäcks och en varning skickas måste en respons vara designad för att hantera hotet (se även [Hotmodellering](#216-hotmodellering-med-stride)) |

EFBox har ingen logging alls utöver det som erbjuds inbyggt i Spring Boot i terminalen. Därför kommer ett logging och alerting system behöva utvecklas och implementeras enligt OWASP rekommendationer.

##### 2.1.9.10 Mishandling Of exceptional Conditions (Felhantering av undantagstillstånd)

Med Felhantering av undantagstillstånd menas att applikationen misslyckas att förebygga eller upptäcka och hantera ovanliga eller oförutsebara tillstånd. Detta leder i sin tur till krascher, oönskat beteende och ibland sårbarhet.

Varje gång applikationen inte vet hur den ska hantera nästa instruktion har ett undantagstillstånd felhanterats. En hacker kan använda dessa sårbarheter för att få applikationen att bete sig på ett oönskat sätt.

Eftersom utvecklare behöver förebygga för det oförutsebara kan undantagstillståndshantering vara komplicerad. OWASP rekommenderar följande:
| Åtgärd | Förklaring |
| ------ | ---------- |
| Undantag fångas tidigt | Nästlade undantagshantering bör undvikas dels för att säkra beteende men också för att underlätta felsökning |
| Rollback | När ett undantag upptäcks ska processen som orsakade den avbrytas och eventuellt köras om. Man pratar om att applikationen felar stängt (_failing closed_) |
| Undvika undantag | För att undvika undantag ska applikationen konfigureras för att undvika dessa. Hastighetsbegränsning (_rate-limiting_), strypning och andra begränsningar förebygger undantagstillstånd. |
| Logging | Vissa undantagstillstånd bör loggas om de förekommer repetitivt över en fördefinierad tidsram. Detta för att undvika [Brute Force Attacker](#2113-brute-force-attack) och Denial of Service (DoS) attacker (dvs anfallet går ut på att överbelasta systemet) . Logging bör ske enligt föreslagna åtgärd i [Security Logging and Alerting Failures](#2179-security-logging-and-alerting-failures-brister-i-säkerhetsloggning-och-larmhantering).|

OWASP understryker också behovet av följande:

- en strikt inputsvalidering med sanitering av tecken som accepteras
- ett centraliserat och globalt felhanteringssystem för att undvika att undantag hanteras på olika sätt genom applikationen.

Dessa åtgärder får tas under beaktning under [hotmodelleringen](#216-hotmodellering-med-stride). Ett återkommande tema är behovet för bra logging av incidenter och EFBox-API:et på granskas. Ett globalt felhanteringssystem behöver skapas.

### 2.2 Befintlig Forskning och Lösningar

Det hittades ingen studie som direkt uppgraderar ett API till OWASP Top 10 standard. Det finns dock studier där OWASP används som grund för att skapa nya REST API baserat på OWASP Top 10 med varierande programmeringsspråk och omfattning.

Till exempel beskriver Silvia Llorente Viejo i sin tes "[Securing a REST API Server](https://upcommons.upc.edu/server/api/core/bitstreams/7376cf6b-eeb4-49ef-819b-281b8ad7a272/content)" (Polytechnic University of Catalonia, 2022) hur ett REST API byggs med OWASP Top 10 2021 som grund. Programmeringsspråket är NodeJS och omfattar bredare aspekter som Docker, NGINX, CI/CD mm. Studien innehåller några intressanta diskussioner och lösningar relevanta till säkringen av EFBox, nämligen valet av Argon2 för kryptering istället för BCrypt (avsnitt 1.2 Fixing A02:2021 – Cryptographic Failures). Viejos studie är relaterad till säkringen av EFBox men den är inte riktigt jämförbar. Den är dock en bra grund och inspirationskälla för arbetsprocessen.

Eftersom varje API som behandlas är unika kommer det inte finnas några direkta lösningar för de problem studien stöter på. Kanske kan studien vara en grund för att fylla denna kunskapslucka.

OWASP är dock en av de ledande oraganisationer (andra är NIST och ISO) som främjar applikationers säkerhet och tillhandahåller lösningar och i vissa fall även exempel projekt (som tidigare nämnt i [Filvalidering](#216-filvalidering)).

Webbsäkerhet är tyvärr ett ofullständigt kunskapsområde då nya exploateringar upptäcks regelbundet på system som ansågs säkra. Attackerna mot system förnyas regelbundet och det är svårt att gardera sig mot det oförutsebara.

### 2.3 Teknisk/Teoretisk Jämförelse

Websäkerhets är ett område med många olika problem som har många olika lösningar. Under förberedande forskning för studien studerades olika möjlighet för att förbättra säkerheten i EFBox och olika bedömningar gjordes. Lösningarna skulle vara relativt enkla och snabba att implementera och någorlunda lättförstådda. T.ex:

- Studien fokuserar på OWASP Top 10 för att information är lättare att ta in jämfört med t.ex NIST (som fokuserar dessutom mer på USA än EU).
- Skiftet till Argon2 är lätt att implementera och enligt alla källor är en klar förbättring. OWASP föreslår dock åtgärder för att kunna behålla _legacy system_ som BCrrypt.
- ZAP används men manuella tester, med mänsklig kännedom av sammanhang och potentiella luckor i koden, kommer att genomföras.
- Rate limiting implementeras mer effektivt i REDIS men för denna studie implementeras det i en HandlerInterceptor klass för enkelhetens skull

---

## 3. Metod och Genomförande

### 3.1 Övergripande Arbetsgång

Källorna för arbetsgången är huvudsakligen OWASP eftersom studien använder deras analys av de mest förekommande hot. Annan referensmaterial kommer att användas, i synnerhet dem som rekommenderas av OWASP som anses vara en pålitlig referens.

Arbetsprocessen för denna studie följer en lättviktig iterativ metod inspirerad av agila principer. Planeringsverktyget [Git Hub-projekt](https://github.com/users/eckofox1981/projects/2) med roadmap används för att kartlägga och kontrollera milstolparna i studien.

För det praktiska arbetet ter planeringen sig enligt följande:

1. Teoretiska studier inkl. grundläggande förståelse runt OWASP Zed Attack Proxy (ZAP).
2. Analys och test av EFbox i dess nuvarande konfiguration
3. Implementera de nödvändiga ändringarna till EFBox
4. Ny analys och testning av EFBox efter implementationen ovan

### 3.2 Verktyg och Tekniker

Resurser använda för studien:

- OWASP:s hemsida
- Spring Boots hemsida
- Google Scholar
- Google för övriga referenser

I utvecklingsmiljön används följande verktyg:

- IntelliJ Community Edtion (IDE) av JetBrains
- IntelliJ-plugin SonarQube för analys av kodkvaliten
- Docker som container för POSTGRESQL-databasen
- Postman, ett API testing verktyg
- OWASP ZAP, ett servertestverktyg
- Programmeringen sker i Java version 23 med ramverket SpringBoot
- Claude AI för code reviews (prompt och historik kommer att bifogas för transparens)
- Git för versionshantering
- Git Hub för repositories

### 3.3 Datainsamling och Analys

#### 3.3.1 Kartläggning av EFbox, struktur och end-points

##### 3.3.1.1 Översikt

EFbox är ett Javabaserat API som använder ramverket Spring Boot. Spring Boot är ett verktyg med öppen källkod som gör det enklare att skapa mikrotjänster och webbappar med Java-baserade ramverk [^34]. Spring Boot förenklar utveckling av webb-applikationer genom att, till exempel, inte behöva skapa XML-konfigurationer vilket äldre ramverk krävde (ex: Apache Turbine).

##### 3.3.1.2 Struktur

EFbox fördelas i tre paket:

- user: innehåller objekt, controller, service och repository för användarobjekten
- fileobjects: innehåller följande paket relaterade till mappar och filer:
  - efboxfolder: innehåller objekt, controller, service och repository för mappobjekten
  - efboxfile: innehåller objekt, controller, service och repository för filobjekten
- security: innehåller säkerhetskonfigurationer som JWT, JWT-filtrering, lösenordskonfigurering mm

Utöver själva koden finns konfigurationsfiler. Ingen CI/CD har implementerats.

#### 3.3.1.3 End-points

_Se Bilaga A - End-points_

#### 3.3.2 Hotmodellering av EFbox

Följande hotmodellering utgår från EFbox systemkomponenter och dataflöden
identifierade i [kartläggningen](#3311-översikt). STRIDE-ramverket används
för att kategorisera hoten.

**Systemelement:**

- Klient (Postman / webbläsare / frontend)
- REST API (Spring Boot)
- Säkerhetslager (Spring Security, JWT-filter)
- Databas (PostgreSQL via JPA/Hibernate)
- Fillagring (databas som blob)

**Dataflöden:**

- Klient till API: HTTP-förfrågningar med JWT i Authorization-header (se [end-points](#3313-end-points))
- API till Databas: JPA-frågor för användare, mappar och filer
- API till Klient: JSON-svar och filinnehåll

| Hot (STRIDE)           | Komponent                              | Beskrivning                                                                                                                                                   | Risknivå | Åtgärd                                                                                                                                                 |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Spoofing               | JWT-filter                             | Anfallaren använder [ett stulet eller förfalskat JWT](#2171-broken-access-control-bristfällig-åtkomstkontroll) för att identifiera sig som en annan användare | Hög      | [Kortlivade JWT](#212-autentisering-och-auktorisering-jwt), [säker signering med externaliserad nyckel](#213-asymmetrisk-kryptografi)                  |
| Spoofing               | /user/login                            | [Brute force-attack](#2113-brute-force-attack) mot inloggningsendpointen                                                                                      | Hög      | [Rate limiting](#2171-broken-access-control-bristfällig-åtkomstkontroll), [stark lösenordspolicy](#2177-authentication-failures-autentiseringsbrister) |
| Tampering              | /file/upload                           | Uppladdning av [skadlig fil med manipulerad Content-Type](#2112-code-injection)                                                                               | Hög      | [Filtypsvalidering](#216-filvalidering)                                                                                                                |
| Tampering              | /folder/change-name, /file/change-name | Anfallaren ändrar namn på en annans resurser                                                                                                                  | Medel    | [Åtkomstkontroll före CRUD-operationer](#2171-broken-access-control-bristfällig-åtkomstkontroll)                                                       |
| Repudiation            | Alla endpoints                         | [Inga säkerhetsloggar](#2179-security-logging-and-alerting-failures-brister-i-säkerhetsloggning-och-larmhantering) – obehöriga åtkomstförsök spåras inte      | Hög      | Implementera säkerhetsloggning                                                                                                                         |
| Information Disclosure | /user/login (felmeddelanden)           | [Vaga felmeddelanden](#21710-mishandling-of-exceptional-conditions-felhantering-av-undantagstillstånd) skyddar mot user enumeration                           | Låg      | Felhantering ska returnera vaga meddelande                                                                                                             |
| Information Disclosure | application.properties                 | [JWT-nyckel och databasuppgifter i klartext](#2177-authentication-failures-autentiseringsbrister)                                                             | Hög      | Externalisera via miljövariablar                                                                                                                       |
| Denial of Service      | /file/upload                           | Ingen lagringskvot – [en användare kan fylla databasen](#2171-broken-access-control-bristfällig-åtkomstkontroll)                                              | Hög      | Lagringskvot per användare                                                                                                                             |
| Denial of Service      | /user/login                            | Upprepade inloggningsförsök utan begränsning                                                                                                                  | Hög      | [Rate limiting](#2179-security-logging-and-alerting-failures-brister-i-säkerhetsloggning-och-larmhantering)                                            |
| Elevation of Privilege | JWT-filter                             | [Manipulerat JWT-payload för att höja användarroll](#2177-authentication-failures-autentiseringsbrister)                                                      | Hög      | Validera JWT-signatur, undvik rollbaserad kontroll                                                                                                     |
| Elevation of Privilege | /folder/search                         | Söksträng direkt i URL-sökväg – potentiellt injektionsmål                                                                                                     | Medel    | [Inputvalidering på sökparameter](#2175-injection-injektionsattacker)                                                                                  |

Hotmodelleringen är förenklad och fokuserar på de mest kritiska hoten
kopplade till OWASP Top 10:2025. En fullskalig hotmodellering med
formella DFD-diagram är utanför studiens scope.

#### 3.3.3 Automatisk och Manuell kodgranskning av EFbox ur ett säkerhetsperspektiv

Kodgranskningen avser bara säkersaspekterna och inte övriga detaljer som vissa designmål (ex: valet att bara skicka tillbaka mappnamnen i _user/info_ istället för deras ID vilket hade varit med användbart för en client).

##### 3.3.3.1 Analys med SonarQube

[SonarQube](https://www.sonarsource.com/) är ett analysverktyg som mäter olika aspekt kodkvaliten och -komplexitet. SonarQube har även ett användarvänligt webverktyg för att analysera projekt på GitHub, rapportformatet är då lättare att läsa. Vid varje pull request analyserar verktyget den nya koden och rapporterar fynden med en _Security Rating_.
**Resultat av analysen med SonarQube**
SonarQube analyserar projekt olika mätpunkter som _maintanability_ (underhållsmässighet), _security_ (säkerhet), _reliability_ (tillförlitlighet), _duplications_ (dubletter), _size_ (storlek), _complexity_ (komplexitet) och _issues_ (problem).
Varje mätpunk får ett betyg från A till E beroende på allvaret i problemen.
Överlag får EFBox följande granskning:
| Mätpunkt | Betyg |
|-|-|
| Security | E |
| Reliability | A |
| Maintanability | A |
| Security Review | E |

_se Bilaga B för mer information_

**Säkerhet**:
Överlag analyserade SonarQube två stora problem med EFBox
| Problem | Hot (OWASP Top 10) | Fil | Åtgärd |
|-|-|-|-|
| 1. Hårdkodade lösenord | Broken Access Control (A01), Security Misconfiguration (A02) | application.properties| Användning av miljövariablar. |
| 2. Acceptans för stora filer (100MB) | Denial of Service Attack, Security Misconfiguration (A02) | application.properties (2 st) | Anses vara acceptabelt för studien |

##### 3.3.3.2 Manuel kodgranskning ur ett säkerhetsperspektiv

All kod i EFBox granskas fil-för-fil och fynder dokumenteras i _Bilaga C - Manuel kodgranskning av EFbox-API:et ur ett säkerhetsperspektiv_.
**Sammanfattning av den manuella säkerhetsgranskningen**

Den manuella kodgranskningen identifierade ett antal återkommande brister
genom hela kodbasen. Nedan sammanfattas de viktigaste fynden per OWASP-kategori:

| OWASP-kategori                             | Fynd                                                                                                                                                                          |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A01 Broken Access Control                  | Avsaknad av GrantedAuthorities och rollhantering.                                                                                                                             |
| A02 Security Misconfiguration              | Felaktig CORS-konfiguration, ingen HTTPS-konfiguration.                                                                                                                       |
| A04 Cryptographic Failures                 | JWT-secret i klartext (.txt-fil), lösenordshash exponeras i SecurityContext, BCrypt bör uppgraderas till Argon2id.                                                            |
| A05 Injection                              | Bristande inputvalidering i samtliga paket. Egenskriven databasfråga i sök-funktionen utgör en reell injektionsrisk. Ingen filvalidering.                                     |
| A07 Authentication Failures                | Lösenordspolicy för svag (min 5 tecken, inga specialtecken). Ingen kontroll mot komprometterade lösenord. Ingen rate limiting på inloggning. JWT-giltighetstid på 60 minuter. |
| A09 Security Logging and Alerting Failures | Ingen säkerhetsloggning eller varningssystem implementerat.                                                                                                                   |
| A10 Mishandling of Exceptional Conditions  | Automatiska felmeddelanden avslöjar känslig information i samtliga controllers. Inget centraliserat undantagshanteringssystem.                                                |

##### 3.3.3.3 Säkerhetstestning av EFBox API:et med OWASP ZAP

OWASP ZAP (för _Zed Attack Proxy_) är ett öppenkällkodsverktyg för att testa API [^36]. Verktyget kan användas för att analysera dataflödet mellan klienten och servern. Efter initial analysen kan funktionen _Active Scan_ generera olika attack scenario och tillhörande rapport.
För att kunna fånga upp dataflödet mellan klienten och servern används ZAP som proxy (mellanhand) med en port mot servern och en mot klienten, i detta fall Postman.

**NOTERING**: under förberedande forskning för ZAP lyckades inte författaren att testa ZAP med vanliga HTTP-förfrågor då ZAP översatte dem till HTTPS, vilket logiskt inte accepterades av Postman. Eftersom HTTPS, enligt Bilaga C, behöver implementeras, konfigurerades ett privat certifikat i resursmappen med tillhörande konfigurationer i application.properties enligt [Spring Boots hemsida](https://docs.spring.io/spring-boot/how-to/webserver.html#howto.webserver.configure-ssl.pem-files).

Test protokoll:

- Tom databas i Docker (nämnd "efbox")
- Starta EFbox-API:et
- Etablera kommunikation mellan Postman - ZAP - EFBox
- Anropa alla end-points med tre olika användar konto med fokus på hanteringen av:
  - minst en mapp i root
  - minst en undermapp till den ovan
  - hantering av både text- och bildfiler
- Aktivera _Active Scan_
- Generera rapport
- Analys

Rapporten finns att tillgå i Bilaga D - ZAP-säkerhetsrapport (före åtgärder).

##### 3.3.3.3.1 Analys av ZAP-säkerhetsrapporten

_se Bilaga D - ZAP-säkerhetsrapport (före åtgärder)_

Följande är en kort sammantfattning av ZAPs genererade rapport.

##### 3.3.3.3.1.1 SQL Injection (High Risk, Medium confidence)

- A05 Injection: ZAP lyckades manipulera inloggningsendpointen /user/login via `firstname`-parametern med AND '1'='1' och fick tillbaka ett giltigt JWT. Varken `firstname` eller `lastname` används i logiken och vi kan utgå från `JpaRepository`skyddet fungerar. Under studien testades även att manuellt angripa servern via Postman genom att söka på `AND '1'= 1` och `OR '1'= 1` (se Bilaga C) men fick bara en vanlig `searchResult` tillbaka (tom). Trots att angreppet inte fungerade bör inte detta vara tillåtet och inputsvalidering bör implementeras.

##### 3.3.3.3.1.2 Buffer Overflow (Medium Risk, Medium confidence) :

- A02 Security Misconfiguration: ZAP skickade en extremt lång sträng och fick svaret 500 med felmeddelande "UUID string too large". Buffer Overflow kan ha berott på den lokala testmiljön och IDE:s begränsade minne men det ett argument för hastighetsbegränsning och inputsvalidering.
- A10 Mishandling of Exceptional Conditions: Svaret var inte fördefinierat utan texten från felet (dvs `e.getMessage()`). Det är ett tecken på att undantagshanteringen bör ses över.

##### 3.3.3.3.1.3 Application Error Disclosure (Low Risk, Medium confidence):

- A10 Mishandling of Exceptional Conditions: ZAP anses att detta är en [Security Misconfiguration](#2172-security-misconfiguration-felaktig-säkerhetskonfiguration) men kommer att hanteras som Mishandling of Exceptional Conditions då det är närmare relaterat till felhantering.

##### 3.3.3.3.1.4 Insights (information som kan vara relevant):

- 69% av svaren ansågs vara långsamma (se [Buffer Overflow](#333312-buffer-overflow-medium-risk-medium-confidence-))
- 22% av svaren returnerade statuskod 5xx vilket tyder på dålig felhantering

##### 3.3.3.3.2 Sammanfattning

Testning med ZAP har delvis bekräftat antaganden från kodgranskningen men det noteras att injektion-försöken, som inte borde tillåtas, inte lyckats.

### 3.4 Planering av åtgärder

Planeringen kommer att läggas upp i förenklad form i ett [Git hub Project](https://github.com/users/eckofox1981/projects/2/views/1).

Baserad på [datainsamling och analys](#33-datainsamling-och-analys) kan vi planera enligt följande:

| Fix                         | Beskrivning                                                                                                                                        | Branch                                         |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| Konfiguration och gitIgnore | Användaruppgifter, secret mm delas i klartext, `.gitignore`ej konfigurerad. GlobalCorsConfig skall ersättas. Cookies implementeras                 | `config-fixes`                                 |
| Infrastrukturen             | Felhanteringen är bristfällig och logging existerar inte och behöver implementeras så att vi kan använda dem senare                                | `new-exception-handling` & `logging-exception` |
| Autentisering               | BCrypt skall ersätttas med Argon2, lösenordspolicy förstärkas, rate-limiting implementeras och JWT TTL justeras med förnyelse                      | `authentication`                               |
| Lösenordsåterställing       | Implementera lösenordsåterställning med email                                                                                                      | `pass-recovery`                                |
| Granted Authorities         | En användare med tillgång åt loggarna skall implementeras med hjälp av inbyggda GrantedAuthorities                                                 | `log-access`                                   |
| Varningssystem              | Repetitiva misslyckade inloggningsförsök skall meddelas till användaren. Likaså repetitiva fel till administratörer (basera på grantedAuthorities) | `warning-system`                               |
| Inputsvalidering            | Ej befintlig ska implementeras på de identifierade områden                                                                                         | `input-validation`                             |
| Filvalidering               | Ej befintlig ska implementeras på de identifierade områden                                                                                         | `file-validation`                              |

### 3.5 Kvalitetssäkring

#### 3.5.1 Metodkvalitet och tillförlitlighet

Studien baseras på OWASP Top 10 2025 vilket strukturerar metoden och implementerar en kvalitetsstandard. Studien försöker att vara så metodiskt som självgående arbete tillåter, en ensam utvecklare kan inte identifiera och åtgärda lika många fel som ett säkerhetsteam. Tidsbristen är också faktor som påverkar både analys och kodkvalitet.

#### 3.5.2 Validering av resultat

Verktyg som ZAP, SonarQube och kodgranskning av ClaudeAI skall hjälpa att motverka [ovannämnda problemen](#351-metodkvalitet-och-tillförlitlighet). Om resultaten från dessa verktyg avviker, undersöks det och diskuteras.

#### 3.5.3 Hantering av bias eller fel

Eftersom författaren granskar sin egen kod finns en risk att problem förbises. Genom att vara metodisk försöker författaren att inte studien blir lidande av partiskhet. Genom dokumentation (hotmodellering, rapport, bilagor, versionshantering mm) försöker studien att vara så oberoende som möjligt.

---

## 4. Resultat

### 4.1 Huvudresultat

#### 4.1.1 Det nya EFBox-API:et

_se även GitHub-repository [EFBox-main brach](https://github.com/eckofox1981/EFbox)._

Projektet gick igenom 10 faser (i branscher) för att försöka säkra applikationen:

**1. Config-fixes:**

- alla säkerhetsdetaljerna som fanns i klar text (användare, lösenord mm) ingår nu i miljövariablar som inte delas på github
- SSL implementerades och cookies används för JWT
- JWT kontrolleras för format

**2. New-Exception-Handling:**

- Exceptions hanteras globalt och inkluderar specifika Exceptions för EFBox

**3. Logging-Exception :**

- Exceptions och andra events loggas nu centralt och sparas i databasen
- det hade varit säkrare att spara loggarna i en separat databas men nuvarande set-up räcker för denna studie.

**4. Log-access:**

- Användare har nu både roller och tillstånd beroende på vilka de kan ha tillgång tillgång till loggarna,
- en process via mail till ägaren (ROLE_OWNER) gör det möjligt att be om adminstatus och tillgång till loggarna (i olika förfrågor)
- Ägare definieras enbart direkt i databasen

**5. Authentication:**

- BCrypt har ersatts av Argon2id,
- lösenordspolicyn har förstärkts med krav på versaler, gemena tecken, siffror och specialtecken,
- lösenord kontrolleras även med HaveIBeenPwned-databasen
- rate-limiting baserad på både IP-address och användar-ID har implementerats i en egen klass, en produktion miljö bör dock implementera detta via REDIS eftersom servern fortfarande förfrågas. Detta märktes under ZAP-testet (se nedan) då min dator frös ett litet tag. Denial-of-service verkar därför fortfarande vara ett problem men rate-limiting försvårar i alla fall brute-force-attacker.
- JWT har en livslängd på 5 minuter, likaså cookien den leveras med och förnyas när en användare utför en auktoriserad förfråga och enbart 3 min återstår av TTL:n.

**6. Pass-Recovery:**

- lösenordsåterställning via mail, en kod genererad med SecureRandom skickas till användaren
- Koden för studien är bara fyrsiffrig men skulle lätt ändras till en längre sekvens

**7. Warning-System:**

- Repetitiva misslyckade inloggningsförsök (fem på femton minuter) meddelas till användaren och admin,
- repetitiva exception (fem på fem minuter) meddelas likaså till admins

**8. Input-validation**

- implementeras med en REGEX av godkända tecken,
- om ett injektionsförsök äger rum sparas längden av strängen i loggarna (JpaHibernate hindrade redan SQL-injektion som nämnt tidigare)

**9. File-validation:**

- Filer som kommer in inspekteras innan de sparas och bilder saneras (manipuleras för att sen sparas på nytt för att strippa metadata och potentiellt skadlig kod),
- de enda filtyper som godkänns är Word-, Excel-, PowerPoint-dokument, Pdf:er och bilder av olika format,
- fler typer av filer skulle kunna accepteras på ett relativt enkelt sätt med nuvarande arkitektur.
- Bildsaneringen orsakar en liten men tydlig kvalitetsförlust, detta kan åtgärdas men det är utanför ramen för denna studie

### 4.1.2 OSV-rapport

Efter att sista branchen mergeades, upptäcktes att anropen till OSV-API:et under [Datainsamling och analys](#33-datainsamling-och-analys) utfördes på fel sätt. Detta berodde på en felaktig tolkning av instruktionerna och att API:et returnerar: `No issues found` om det inte hittar dependency filen den ska analysera. Ett nytt anrop, korrekt formaterat, avslöjade 61 sårbarhet, varav 8 bedömdes som kritiska (_se Bilaga O_). Därför skapades en ny branch för att försöka uppdatera så många dependencyversioner som möjligt.

**EXTRA 10. Dependency-fixes**

- uppdatera Spring Boot-version till 3.4.6 vilket löste en större del av problemen, detta krävde dock att Gradle-version uppdaterades till Gradle 9.5.1,
- oanvända dependencies togs bort (OWASP rekommendation) men test dependencies lämnades dock kvar för framtida utveckling (Unit testing avaktiverades i samband med detta).
- Vissa dependencies kunde tvingas till uppgradering genom kod som `extra["spring-security.version"] = "6.4.10"` men andra fick lämnades utan åtgärd då de orsakade problem vid `build`
- Bilaga P - OSV-rapport efter åtgärd visar slutkompromissen, 12 sårbarhet varav enbart en kritisk. Den kritiska är en `spring-security-web` dependency som inte berör projektet då EFBox har egenkonfigurerad autentisering med JWT (referens i bilaga).

#### 4.1.3 ZAP-säkerhetsrapport (efter åtgärder)

Efter implementerade åtgärder genomfördes en ny ZAP Active Scan mot EFbox API:et enligt samma testprotokoll som i [den initiala skanningen](#3333-säkerhetstestning-av-efbox-apiet-med-owasp-zap). Rapporten finns att tillgå i Bilaga D - ZAP-säkerhetsrapport (efter åtgärder).

| Fynd                              | Risk   | Confidence | Bedömning                                                                                                                                                                                                                              |
| --------------------------------- | ------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SQL Injection                     | High   | Medium     | ZAP sparade ett lösenord med SQL-kommandon `[(...)AND '1'='1' -- ]` och `[(...) OR '1'='1' -- ]`. Detta ett **falskt positivt** då lösenorden sparas i hashade former i databasen och den råa strängen hanteras enbart för att hashas. |
| Spring4Shell (CVE-2022-22965)     | High   | Medium     | **Falskt positivt** det åtgärdats i Spring Boot 2.6.6, EFbox använder 3.4.1 och körs som executable jar [^37]                                                                                                                          |
| CSP Header Not Set                | Medium | High       | **Accepterat** då CSP är primärt relevant för webbläsarbaserade applikationer med frontend, vilket faller utanför studiens scope                                                                                                       |
| Cookie without SameSite Attribute | Low    | Medium     | **Kvarstående brist:** SameSite-attributet är inte konfigurerat på JWT-cookien. **Detta är en översikt.**                                                                                                                              |

Jämfört med den initiala skanningen noteras att Buffer Overflow och Application Error Disclosure inte längre förekommer, vilket indikerar att den centraliserade felhanteringen fungerar som avsett.

#### 4.1.4 Manuellt testande via Postman

Med postman testades alla säkerhetsspärrar som implementerades, från otillåten resursåtkomst till filinjektioner, genom att kalla de olika end-points.

Tre användare skapades, alla med olika filer och mappar och olika otillåtna åtkomstförsöken utfördes utan framgång.

För filvalideringstestet försökte författaren att skicka ett exceldokument med macros (ej tillåtet) och EFBox identifierade korrekt att filen inte var acceptabel, vilket förväntades av filvalideringen.

### 4.2 Detaljerade fynd

#### 4.2.1 Detaljerade fynd per OWASP-kategori

| OWASP-kategori                     | Identifierad brist                                                                                                              | Åtgärd                                                                                                                                                                                       | Verifiering                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| A01 Broken Access Control          | Ingen rollhantering, ingen lagringskvot                                                                                         | GrantedAuthorities, roller och tillstånd implementerade, ostandardiserade URI används som ett extra lager – en anfallare som söker efter "/admin"-endpoints hittar inte loggtjänsten direkt. | Manuell testning                                                                 |
| A02 Security Misconfiguration      | Felaktig CORS, ingen HTTPS, debug-logging                                                                                       | Miljövariabler, SSL, CORS i SecurityConfig                                                                                                                                                   | ZAP + manuell                                                                    |
| A03 Software Supply Chain Failures | Felaktig OSV-skanning, oanvända dependencies raderades, sårbara dependencies identifierades sent under studiens gång (Bilaga O) | Uppdatering av Spring Boot till 3.4.6, Gradle till 9.5.1, tvingade versionsuppgraderingar, borttagna oanvända dependencies, identifiering och accepterande av säkerhetskompromisser          | OSV-rapport (Bilaga P)                                                           |
| A04 Cryptographic Failures         | BCrypt, secret i klartext, lösenord bortaget från SecurityContext                                                               | Argon2id, miljövariabel, debugg lines                                                                                                                                                        | Manuell testning                                                                 |
| A05 Injection                      | Ingen inputvalidering, råa databasfrågor, ingen filvalidering                                                                   | Global REGEX-validering, loggning av försök, bildåtergivning, vanliga injektionskod triggar en Exception och loggas                                                                          | ZAP + manuell                                                                    |
| A07 Authentication Failures        | Svag lösenordspolicy (REGEX & HavIBeenPwned), ingen rate limiting                                                               | Förstärkt policy, rate limiting baserad på IP och ID, JWT TTL 5 min med förnyelse, lösenordsåterställningssystem                                                                             | ZAP + manuell                                                                    |
| A08 Software/Data Integrity        | Ingen kontroll av leveranskedjan                                                                                                | Kontroll med OSV, användning av betrodda källor, sistnämnda gjordes redan och fortsatte med under studien                                                                                    | OSV-rapport för dependencies och manuell kontroll för utvecklingsmiljöns verktyg |
| A09 Logging and Alerting           | Ingen loggning                                                                                                                  | Centraliserad loggning i databas, varningssystem via mail, logging i läsbarformat (HTML med färgkoder)                                                                                       | Manuell testning (ZAP-testning triggade alla varningar)                          |
| A10 Exceptional Conditions         | Lokala felhanterare, e.getMessage() exponerat                                                                                   | GlobalExceptionHandler, generiska meddelanden, Exceptions fångas tidigt                                                                                                                      | ZAP (Buffer Overflow borta)                                                      |

#### 4.2.2 Kontroll med hotmodelleringen

_se [Hotmodellering av EFBox](#332-hotmodellering-av-efbox)_

Hotmodelleringen visade sig vara ett användbart verktyg för att förutse de brister som senare identifierades i kodgranskningen. Samtliga hot med risknivå Hög och Låg åtgärdades, med ett undantag: lagringskvoten per användare förbisågs under planeringsfasen och implementerades aldrig (se [5.3 Begränsningar](#53-begränsningar-och-kritisk-granskning)).

De två hoten med risknivå Medel åtgärdades – inputvalidering implementerades på samtliga användarinputs (med undantag för lösenord som hashas direkt) och ägarskapskontrollen skyddar mot obehörig namnändring av andras resurser.

Hotmodelleringen förutsåg inte OSV-sårbarheter i leveranskedjan, vilket uppdagades sent i projektet. Det understryker värdet av att kombinera flera analysmetoder snarare än att förlita sig på en enskild.

Det sistnämnda avslöjar en svaghet av STRIDE-hotmodelleringen: den mänskliga faktorn nämns inte i STRIDE men bör räknas in. Detta görs sen länge i andra riskutsatta branscher, mest påtagligt flygbranschen som implementerar kontroller och procedurer för alla tänkbara riskscenario.

### 4.3 Oväntade Resultat

Ett oväntat fel som inte hann lösas var att visa undantag inte hanterades av GlobalExceptionHandler. Undantag utan specifik error handling borde hanteras som `UndefinedException`. Problemet går säkert att lösa men inte inom tidsramen för studien. Trots detta skickas inga automatiska felmeddelande tillbaka (e.getMessages()) till användaren vilket är det viktigaste.
Den svåraste aspekten för detta projekt var att acceptera att författaren inte kunde få en "ren" OSV-rapport utan _issues_ utan fick kompromissa för att EFBox underhållet skulle vara hållbart. Studien förväntade sig att kunna lösa just detta problem utan att kompromissa på detta vis. Dock finns kompromis som en del av hotmodellingens effektivitetskontroll (se [Hotmodellering med Stride, punkt 3: Acceptera](#216-hotmodellering-med-stride)).

---

## 5. Diskussion

### 5.1 Analys av Resultat

#### 5.1.1. Vilka säkerhetsbrister identifieras i EFbox REST API utifrån OWASP Top 10:2025?

OWASP dokumentation är väldigt detaljerad och dess _cheat sheet_ serie är väldigt användbar för att implementera åtgärder och tankegångar. Listan av säkerhetsbrister listad i [Sammanfattning](#33332-sammanfattning) får antagligen med de flesta brister som fanns med i projektet när studien började.
Tyvärr är det svårt som ensam utvecklare att diskutera runt de olika säkerhetsaspekterna, potentiella hot och lösningar. Kunskap är också begränsad i ett team av en person. AI skulle helt klart bredda ut ramen för studien och driva fram olika lösningar men den underliggande tanken för denna studie var att få lära sig de olika säkerhetskoncepterna på egen hand.

#### 5.1.2. Hur kan de identifierade bristerna åtgärdas inom ramen för det befintliga systemets arkitektur?

Denna fråga är svår att svara eftersom det enda sättet att åtgärda bristerna var att **utöka arkitekturen**. Jämför man originalprojektet (sparad i branchen `originalForEssay`) med slutprodukten (branch `main`) kan vi konstatera att 4.193 tillägg gjordes mot 847 borttagningar. **Elva nya _packages_ lades till med totalt 49 nya klasser** och sex nya _end-points_ skapades för att hantera det olika roller och tillstånd.

Den grundlägande arkitekturen finns kvar med `fileobject` och `user` packages. Men för att åstadkomma OWASP rekommendationerna fick en parallell säkerhetsarkitektur skapas där all data trafik filtreras och fördelas mellan de olika services.

Därmed är den största lärdom av studien:

> **För att säkra ett existerande API enligt OWASP Top 10 måste man analysera, testa, planera, åtgärda och analysera igen. Repetera.**

Tidsramen för säkringen av EFBox är för kort för att repetera hela processen, men ska ett litet team utvecklare utan specialiserat säkerhetsteam anta en sådan omstrukturering är det ett effektivt tillvägagångssätt.

#### 5.1.3. Hur verifieras att implementerade åtgärder är effektiva?

Eftersom studien riktar in sig speciellt på EFBox är det svårt att kvalitetssäkra åtgärderna specifikt som ensamstående utvecklare. Det hittades tyvärr ingen jämförbar studie för att kontrollera resultaten med och hjälpverktygen har alla säregna brister.
SonarQube inriktar sig speciellt på kodkvalitet men är också mån om att varna om potentiella säkerhetshot. Dock saknar SonarQube förmågan att analysera sammanhang och gav några falskt positiva fel, som t.ex denna kodrad i UserService.class:

```java
private boolean isUsernameValid(String username) {
        final String PASSWORD_REGEX = "^[a-zA-Z0-9]{5,20}$";
        return username.matches(PASSWORD_REGEX);
    }
```

Vid varje pull request flaggades `PASSWORD_REGEX` som ett säkerhets hot eftersom SonarQube antog att ett lösenord hade hårdkodas, vilket sänkte förtroendet för verktyget.

Pull requests analyserades även av Claude AI för kodkvalitet (se Bilagor F till M). Detta har varit hjälpsamt för att hålla kodkvalitet och rätta enkla misstag som studien, med en ensam utvecklare, hade missat. Claude AI kunde ibland lida av bristande sammanhangshålning och även hallucinationer. Flera rapporter påpekar att `.env.example` har två st `OWNER_EMAIL` miljövariablar vilka inte fanns. Överlag dock har Claude AI:s kodgranskning varit hjälpsam. Utan att information om branchens syftet kunde verktyget analysera syftet med den och beskriva det på ett korrekt sätt i rapporterna. Det ansågs vara ett tecken på kodtydlighet och att målen uppnåtts.

ZAP är ett fantastiskt verktyg för en smidig analys. Alla registrerade end-points skannas och testas och antalet förfrågan på kort tid testa serverns kapacitet. T.ex kunde studien konstatera att Rate-limiting åtgärderna, baserade på IP för anonyma användare alternativt på användarID, skulle inte räcka mot en _Denial of Service_-attack då testmiljön "frös" i ett par minuter. Alla registrerade exception finns i Bilaga Q - EFBox_event_log efter ZAP test (EFbox skapa en HTML-verion av loggarna). Det konstateras att de implementerade skydden fyller sina syften.
Rapporten fyllde sitt syfte men även här fanns ett falskt positiv fynd om en SQL-injektion eftersom lösenord har ingen inputvalidering för att de hashas i databasen. ZAP skickade inte heller några filer för att testa injektionsangrep utan ett excelblad med macro skickades manuellt för att testa filvalideringen. Andra manuella tester har varit att ljuga om filtypen, skicka förbjudna filer mm.

Sist har en del av kvalitetssäkring varit att arbeta metodiskt enligt de planerade åtgärder med hjälp av verktyget GitHub Project med utrymme för anpassning till de olika problem som uppstod. Om inga fel hittades i samband med en granskning, en logik eller av test verktygen så dubbelkollades noga. Det var så OSV granskningen av dependencies uppdagades och kunde åtgärdas. Det hade dock varit bättre, och mer givande, att arbeta i team och diskutera resultat.

> Utan en bred erfarenhetsbas är det orimligt att täcka alla hotscenario som kan förekomma hos specifika applikationer.

### 5.2 Reflektion över Metod

Metoden som används i studien anser författaren vara metodiskt och anpassad. Forskning på ämnet ägde rum i en förberedande fas och en initial plan las fram för studien. Efter att hotmodelleringen skapades användes den som bas för analys och planering. Varje kväll gjordes en analys över vad som fungerat och vad som behövde förbättras och/eller behövas läggas till. Innan en pull-requests testades den nya koden och SonarQube-plugginen granskades för allvariga problem.

Överlag har metoden varit korrekt, dess brister diskuteras i nästa avsnitt.

### 5.3 Begränsningar och Kritisk Granskning

Begränsingarna har huvudsakligen grundat sig i att arbeta själv. Ett team kan fördela resurser och stämma av med projektdeltagarna. Kunskapsbasen är inte heller så bred med bara en person som ska läsa sig på alla tekniska möjligheter att lösa ett problem. Kanske har enkla lösningar förbisetts och tid förlorats pga detta. De lösningar som har implementerats är kanske bristfälliga på ett sätt studien inte förutsett.

Ett misstag som gjordes pga tidsbristen under hotmodelleringen var att en lagringskvot för användaren förbisågs och planerades aldrig. Det resulterade i sin tur att det inte planerades och följaktigen inte implementerades.

Att arbeta ensam ökar också risken för partiskhet när man analysera sitt eget arbete: har rätt verktyg, metodologi och koncept används och implementerats på rätt sätt eller har mindre anpassade sätt tillämpats för att de var enklare att använda?

Skulle studien göras om, skulle följande rekommenderas:

- tillräckligt med tid för de involverade (team eller ensam utvecklare),
- arbeta metodisk
- planera noga
- förbli anpassningsbar

Trots dessa funderingar är EFBox API:et mycket säkrare än det var innan och författaren tror att den skulle åtminstone klara en enklare OWASP-granskning.

### 5.4 Bredare Perspektiv

Denna studie är riktad specifikt mot EFBox men det finns lärdomar för andra utvecklare som ska säkra andra API. Att säkra ett befintligt system är ett vanligare scenario i industrin än att bygga nytt från grunden, vilket gör studiens tillvägagångssätt relevant. Man kan basera sin egen forskning på den tillämpade metodologin som beskrivs i detta dokument. Man skulle även kunna tillämpa arbetssättet till andra språk (OWASP-koncepten är inte språkberoende) och ramverk. Dokumenteras det kan en bredare kunskapsbas byggas för framtiden.
Inga andra studier av denna typ hittades under förberedande forskning och projektet kan komma till användning för att etablera bättre metodologi och standard lösningar för säkring av befintliga REST API.

---

## 6. Slutsatser

### 6.1 Huvudslutsatser

Websäkerhet i allmänhet är inte väldefinierad i den mening att kompromiss måste uppnås beroende på de kommersiella intressen. En väderapplikation utsätts inte för samma hot som ett socialt nätverk. Därför var frågeställningen svår att både definiera och svara på. Säkerhetsbristerna identifierades systematiskt med OWASP Top 10 och hotmodellering som referensram. Åtgärderna krävde en utökning av arkitekturen snarare än enbart modifieringar, vilket i sig är en viktig lärdom.
Det är dock tydligt att det enda sätt att verkligen granska de implementerade lösningarna är effektiva vore att med riktig _penetration testing_ hitta problemen att granska EFBox.
Studien avser ett småskaligt projekt men understryker tydligt hur stort och invecklat säkerhetsområdet är. Små start-ups drivna av kodentusiaster bör ta detta i beaktning, särskilt med tanke på att studien inte ta upp alla regler runt GDPR och dess krav på lagring av personlig data. Det sistnämnda blev ännu viktigare då användaremail las till som krav för att skapa EFBox konto.

### 6.2 Bidrag och Betydelse

Under förberedande forskning hittades många olika lösningar online på specifika problem men ingen övergripande logik eller metodologi. OWASP, NIST och andra organisationer har detaljerade lösningar men inte metodologi. Betydelsen av denna studie är att visa ett sätt att implementera säkerhet. Utvecklare kan dra lärdomar från de misstag som begicks här och få inspiration för metodologi, verktyg mm.

### 6.3 Framtida Arbete

Studien täcker säkringen av EFBox API:et men det framgår tydligt av slutsatserna att vidare forskning bör läggas på bl.a:

- Att etablera en metodologi för säkringen av applikationer, möjligen utvecklingen av en checklista
- Att undersöka säkerhetspåverkan av att arbeta som ensamutvecklare
- Att ta fram en metod för mäta förhållandet mellan säkerheten / kommersiella behoven / arbetsbelastningen av en applikation
- Att bedöma påverkan av GDPR på säkerhetsarkitekturen av en applikation

---

## Bilagor

- Bilaga A - End-points
- Bilaga B -SonarCloud-analys (före åtgärder)
- Bilaga C - Manuel kodgranskning av EFbox-API:et ur ett säkerhetsperspektiv
- Bilaga D - ZAP-säkerhetsrapport (före åtgärder)
- Bilaga E - Claude Exchange: EFBox-PR1-Code-Review
- Bilaga F - EFBox_PR2_CodeQualityReport
- Bilaga G - EFBox_PR3_CodeQualityReport
- Bilaga H - EFBox_PR4_CodeQualityReport
- Bilaga I - EFBox_PR5_CodeQualityReport
- Bilaga J - EFBox_PR6_CodeQualityReport
- Bilaga K - EFBox_PR7_CodeQualityReport
- Bilaga L - EFBox_PR8_CodeQualityReport
- Bilaga M - EFBox_PR9_CodeQualityReport
- Bilaga N - ZAP Scanning Report efter åtgärd
- Bilaga O - OSV-rapport
- Bilaga P - OSV-rapport efter åtgärd
- Bilaga Q - EFBox event_log efter ZAP test

## Referenser

[^1]: Bandar Alotaibi, “Cybersecurity Attacks and Detection Methods in Web 3.0 Technology: A Review”, Sensors, Januari 2025. Available: https://www.mdpi.com/1424-8220/25/2/342

[^2]: “Antitrust cases against Google by the European Union”, Wikipedia, Accessed Maj 2026. Available: https://en.wikipedia.org/wiki/Antitrust_cases_against_Google_by_the_European_Union

[^3]: Vatsala Gaur, “How the EU is taking on Big Tech: Meta, Apple, Google, face heightened scrutiny, penalties”, Invezz, December 2025. Available: https://invezz.com/news/2025/12/04/how-the-eu-is-taking-on-big-tech-meta-apple-google-face-heightened-scrutiny-penalties/

[^4]: “Digital Sovereignty in Tension: U.S. Pushback Against the EU’s Digital Services Act”, The Cyber Institute, Augusti 2025. Available: https://www.cyber-institute.org/post/digital-sovereignty-in-tension-u-s-pushback-against-the-eu-s-digital-services-act

[^5]: Clare Duffy, “Trump administration’s vision of US tech dominance is colliding with Europe ”, CNN, Januari 2026. Available: https://edition.cnn.com/2026/01/12/tech/us-eu-tech-regulation-fight-explained

[^6]: Marius Laffont, “Pour son indépendance numérique, l'État français souhaite passer à Linux”, RFI, April 2026. Available: https://www.rfi.fr/fr/france/20260410-pour-son-ind%C3%A9pendance-num%C3%A9rique-l-%C3%A9tat-fran%C3%A7ais-souhaite-passer-%C3%A0-linux

[^7]: OWASP about page, Accessed: Maj 2026. Available: https://owasp.org/about/

[^8]: OWASP Top 10 threats, Accessed: Januri -Juni 2026. Available: https://owasp.org/www-project-top-ten/

[^9]: Lorikeet Security Team, "PCI DSS Requirement 6: Secure Development Practices Your QSA Will Scrutinize", Accessed: Maj 2026. Available: https://lorikeetsecurity.com/blog/pci-dss-requirement-6-secure-development

[^10]: NordVPN, Accessed May 2026. Available: https://nordvpn.com/sv/blog/mitm-attack/

[^11]: Wikipedia, Accessed May 2026. Available: https://en.wikipedia.org/wiki/Man-in-the-middle_attack

[^12]: JWT HANDBOOK, Sebastiàn Peyrott, Publisher; Auth0 by Okta. Available: https://www.jwt.io

[^13]: ItSecurityDemand, Accessed: May 2026. Available: https://www.itsecuritydemand.com/insights/security/code-injection-attacks-a-guide-to-security-prevention/

[^14]: Wikipedia, Accessed May 2026. Available: https://sv.wikipedia.org/wiki/Brute_force

[^15]: Tom Krantz & Alexandra Jonker, IBM, Accessed: May 2026. Accessible: https://www.ibm.com/think/topics/brute-force-attack

[^16]: Mozilla, Accessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/CSRF

[^17]: Asymmetrisk kryptografi: Vad är det och varför använda det inom cybersäkerhet?, HMS, Ewon Ewon, översatt till svenska, Accessed: May 2026. Available: https://www.hms-networks.com/sv/industrial-iot-blog/blogpost/hms-blog/2024/01/08/asymmetric-cryptography-what-is-it-and-why-use-it-in-cyber-security

[^18]: Encryption vs Hashing vs Salting, Geeks For Geeks, July 2025. Available: https://www.geeksforgeeks.org/computer-networks/encryption-vs-hashing-vs-salting/

[^19]: Password Storage Cheat Sheet, OWASP, Accessed: May 2026. Available: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

[^20]: Understanding Rainbow Table Attack, Geeks For Geeks, Feb 2023. Available: https://www.geeksforgeeks.org/ethical-hacking/understanding-rainbow-table-attack/

[^21]: Input Validation Cheat Sheet, OWASP, Accessed: May 2026. Available: https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html

[^22]: File Content Validation in Java and Spring, Null Gaming India, YouTube, Published March 2024. Available: https://youtu.be/A_reBQO6n30?si=8n8LE3mfoDAxzmOI

[^23]: File Upload Cheat Sheet, OWASP, Accessed: May 2026. Available: https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html

[^24]: HTTP, Mozilla, Acessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Glossary/HTTP

[^25]: Overview of HTTP, Mozilla, Accessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview

[^26]: Using HTTP cookies, Mozilla, Acessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies

[^27]: Cross-Origin Resource Sharing (CORS), Mozilla, Acessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS

[^28]: Hypertext Transfer Protocol Secure, Wikipedia, Accessed: May 2026. Available: https://sv.wikipedia.org/wiki/Hypertext_Transfer_Protocol_Secure

[^29]: Threat modeling, Mozilla, Acessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/Security/Threat_modeling

[^30]: Threat Modeling Cheat Sheet, OWASP, Accessed: May 2026. Available: https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html

[^31]: Threat Modelling Manifesto, Zoe Braiterman, Adam Shostack, Jonathan Marcil, Stephen de Vries, Irene Michlin, Kim Wuyts, Robert Hurlbut, Brook S.E. Schoenfield, Fraser Scott, Matthew Coles, Chris Romeo, Alyssa Miller, Izar Tarandach, Avi Douglen, Marc French, Threat Modelling Manifesto, Accessed: May 2026. Available: https://www.threatmodelingmanifesto.org/

[^32]: Threat modeling frameworks and tools, Mozilla, Acessed: May 2026. Available: https://developer.mozilla.org/en-US/docs/Web/Security/Threat_modeling/Frameworks#stride

[^33]: Security HTTP Response Headers, Spring, Accessed: May 2026. Available: https://docs.spring.io/spring-security/reference/features/exploits/headers.html

[^34]: Why is quantum cryptography important?, Josh Schneider, IBM, Published: December 2023. Available: https://www.ibm.com/think/topics/quantum-cryptography#Why+is+quantum+cryptography+important?

[^35]: Vad är Java Spring Boot?, Azure Microsoft, Accessed: May 2026. Available: https://azure.microsoft.com/sv-se/resources/cloud-computing-dictionary/what-is-java-spring-boot/

[^36]: Zed Attack Proxy (ZAP), Accessed: May 2026. Available: https://www.zaproxy.org/

[^37]: Spring4Shell: New info and fixes (CVE-2022-22965), Accessed: May 2026. Available: https://www.helpnetsecurity.com/2022/04/01/cve-2022-22965/
