# Manuell kodgranskning av EFbox-API:et ur ett säkerhetsperspektiv

## 1. Metod

GitHub-repository: [branch originalForEssay](https://github.com/eckofox1981/EFbox/tree/originalForEssay)

För den manuella granskningen kommer studien att gå igenom varje fil och leta efter logik eller kod som kan utgöra ett hot enligt OWASP Top 10.

En tabell för varje fil ritas upp nedan med fynderna.

---

## 2. Förkortningar

| Term/Förkortning | Förklaring                                                                                                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CRUD             | Create, Read, Update, Delete – syftar på datahanteringen som skapande, läsande, uppdatering och radering                                                                                          |
| CORS             | Cross-origin resource sharing, en teknik som begränsar åtkomst till websidor från specifierade domäner                                                                                            |
| DTO              | Direct Transfer Object, ett objekt som används för att förmeddla enbart vald data utan logik.                                                                                                     |
| JDE              | Java Development Environment                                                                                                                                                                      |
| JDK              | Java Development Kit                                                                                                                                                                              |
| One To Many      | En term för att beskriva förhållandet mellan objekt i en databas. One To Many syftar på att ett objekt kan relatera till flera andra objekt (en användare kan ha flera mappar)                    |
| Many To One      | En term för att beskriva förhållandet mellan objekt i en databas. Many To One syftar på att flera objekt av samma typ kan relatera till samma objekt (flera mappar kan ägas av en enda användare) |

## 3. Granskningen

### 3.1 User-paketet

#### 3.1.1 User.java

User.java är ett enkelt användarobjekt med ett OneToMany-förhållande till mappar. Granskning noterar även rad 29:

```java
@OneToMany(mappedBy = "user", fetch = FetchType.EAGER, cascade = CascadeType.ALL, orphanRemoval = true)
private final List<EFBoxFolder> RootFolder;
```

som kommer att se till att vid kontoradering kommer även användarens mapp att raderas.

| Rad | Fynd               | OWASP-referens            | Kommentar                                                                                                                                                                                                        | Åtgärd                                    |
| --- | ------------------ | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| 42  | `getAuthorities()` | A01 Broken Access Control | Denna funktion är en enkel override från GrantedAuthorities. Funktionen returnerar bara en tom lista men skulle kunna användas av administratörsliknande användare för att komma åt säkerhetsloggarna (A09/A10). | Implementera GrantedAuthorities           |
| -   | Ingen permission   | A01 Broken Access Control | I samband med ovanstående finns det ingen GrantedAuthorities för användaren.                                                                                                                                     | Skapa GrantedAuthorities och implementera |

#### 3.1.2 UserController.java

UserController.java exponerar alla endpoints relaterade till User-objektet.
Generellt skickar controllern förfrågan vidare till UserService.java och hanterar svaret till användaren.
En subklass UserDTO samt en NoPasswordUserDTO är inbakade i klassen.

| Rad | Fynd                                                             | OWASP-referens                                                                        | Kommentar                                                                                                                                                                                                                                                                                                                                                | Åtgärd                                                                                       |
| --- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| 36  | Icke-vaga felmeddelanden                                         | A10 Mishandling of Exceptional Conditions                                             | Om något fel inträffar vid skapande av användaren skickas ett informationsmeddelande med felmeddelandet från `UserService.createUser()`. Felmeddelandet är generellt vagt vilket är positivt. Däremot fångas bara Exception, vilket innebär att om ett annat fel än det i createUser fångas kan känslig information oavsiktligt skickas till användaren. | En ytterligare catch-sats bör skrivas för att definiera ett svarsmedelande.                  |
| 52  | LoginException fångas                                            | A09 Security Logging and Alerting Failures, A10 Mishandling of Exceptional Conditions | Enbart LoginException fångas, vilket har ett vagt meddelande (positivt), men inga andra undantag finns förberedda som backup.                                                                                                                                                                                                                            | En ytterligare catch-sats bör fånga generella undantag utan att avslöja känslig information. |
| 69  | Icke-vaga felmeddelanden                                         | A10 Mishandling of Exceptional Conditions                                             | Se rad 36 ovan                                                                                                                                                                                                                                                                                                                                           | Se rad 36 ovan                                                                               |
| 84  | Standard Exception skickar automatiskt genererade felmeddelanden | A10 Mishandling of Exceptional Conditions                                             | Om ett undantag inträffar i UserService.deleteUser skickas ett automatiskt felmeddelande till användaren med risk att känslig information avslöjas.                                                                                                                                                                                                      | Bör ändras till vagt felmeddelande.                                                          |

#### 3.1.3 UserService.java

UserService behandlar logiken för hanteringen av User-objekt och CRUD-operationer via UserRepository.

| Rad    | Fynd                                       | OWASP-referens                            | Kommentar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Åtgärd                                                                                                                               |
| ------ | ------------------------------------------ | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| 31     | `createUser()`                             | A07 Authentication Failures               | `createUser()` har en lösenordsvalidering som tillåter enbart mer än 5 tecken, minst en versal och gemena bokstäver och minst en siffra. OWASP refererar till [NIST-dokumentationen](https://pages.nist.gov/800-63-3/sp800-63b.html#:~:text=5.1.1%20Memorized%20Secrets) för lösenordslängd när dessa väljs av användarna själva: mellan 8 och 64 tecken. Angående komplexitet är [NIST bilaga A](https://pages.nist.gov/800-63-3/sp800-63b.html#appA) försiktig med att definiera en standard men varnar att alltför komplexa lösenord riskerar att glömmas av användaren. Introduktionen av specialtecken (@, +, $, mm) gör det svårare för anfallare att utföra brute force-attacker. | Förläng regex i passwordValidationIsOK till 8 upp till 64 tecken och introducera krav på ett specialtecken.                          |
| 31     | `createUser()`                             | A05 Injection                             | Användarnamnet valideras inte vid registreringen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Inputvalidering implementeras                                                                                                        |
| 32, 86 | `createUser()`, `passwordValidationIsOK()` | A07 Authentication Failures               | `passwordValidationIsOK()` kontrollerar inte mot vanliga eller komprometterade lösenord. OWASP föreslår en kontroll mot [Have I Been Pwned](https://haveibeenpwned.com) som tillhandahåller ett API för ändamålet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Implementera kontroll av svaga eller komprometterade lösenord, antingen via Have I Been Pwned eller med hjälp av en nedladdad lista. |
| -      | Bra undantagshantering                     | A10 Mishandling of Exceptional Conditions | UserService skickar generiska felmeddelanden utan att avslöja känslig information.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | N/A                                                                                                                                  |

#### 3.1.4 UserRepository.java

Denna fil hanterar kommunikationen mellan databasen och UserService.java. Utan anmärkning.

### 3.2 EFBox Folder-paketet

#### 3.2.1 EFBoxFolder.java

EFBoxFolder.java representerar mappobjektet. Eftersom mappar kan referera till varandra via _parentFolder_ ser man även här (rad 29) till att tillhörande filer och mappar raderas i samband med föräldramappens radering:

```java
@OneToMany(mappedBy = "user", fetch = FetchType.EAGER, cascade = CascadeType.ALL, orphanRemoval = true)
private final List<EFBoxFolder> RootFolder;
```

Inga säkerhetsavvikelser påträffades.

#### 3.2.2 EFBoxFolderDTO.java och SearchResponseDTO.java

Dessa DTO:er är ganska enkla och riskfria ur ett säkerhetsperspektiv. Designvalet att bara skicka tillbaka fil- och mappnamn behandlas ej i denna studie.
Det noteras positivt att användaruppgifter (ID eller användarnamn) inte delas ut i DTO:erna.
Utan anmärkning.

#### 3.2.3 EFBoxFolderController.java

EFBoxFolderController.java exponerar alla endpoints relaterade till EFBoxFolder-objektet.
Generellt skickar controllern förfrågan vidare till EFBoxFolderService.java och hanterar svaret till användaren.
| Rad | Fynd | OWASP-referens | Kommentar | Åtgärd |
|-|-|-|-|-|
| 34, 46, 63, 77, 90 | Automatiska felmeddelanden returneras | A10 Mishandling of Exceptional Conditions | De olika funktionerna har fördefinierade meddelanden för undantagshantering men som backup för oförutsedda undantag skickas automatiska felmeddelanden. Risk för att känslig information avslöjas. | Implementera generiska felmeddelanden. |

#### 3.2.4 EFBoxFolderService.java

EFBoxFolderService behandlar logiken för hanteringen av User-objekt och CRUD-operationer via UserRepository.
| Rad | Fynd | OWASP-referens | Kommentar | Åtgärd |
|-|-|-|-|-|
| - | Bristande inputvalidering | A05 Injection | Många funktioner accepterar stränginputs från användaren men dessa valideras aldrig mot injektioner. EFBox använder visserligen JpaRepository som agerar som mellanhand för databaskommunikation, men inputvalidering är en bra praxis att adoptera. | Implementera inputvalidering |
| 75 | Bristande inputvalidering | A05 Injection | `searchInAllFolder()` lider av inputvalideringsbristen nämnd ovan. I detta fall är det ett allvarligt problem då funktionen som anropas i EFBoxFolderRepository.java anropar databasen med en specialskriven fråga som tar in inputen obehandlad. Detta utgör en större risk för injektionsattacker. | Se ovan |
| 120 | Undantagshantering | A10 Mishandling of Exceptional Conditions | `changeFolderName()` kan kasta två olika sorters undantag men funktionen anger bara det generella Exception. | Exception bör ändras till de specificerade undantagen i funktionen |
| 139 | Åtkomstkontroll | A01 Broken Access Control | Funktionen `userIsNotFolderOwner()` kontrollerar att användaren äger mappen innan varje CRUD-operation, vilket är positivt. | N/A |

#### 3.2.5 EFBoxFolderRepository.java

EFBoxFolderRepository.java hanterar kommunikationen mellan databasen och EFBoxFolderService.

| Rad | Fynd                      | OWASP-referens | Kommentar                                                                                                                                                  | Åtgärd                                  |
| --- | ------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 20  | Bristande inputvalidering | A05 Injection  | Som nämnts tidigare i [EFBoxFolderService](#324-efboxfolderservice.java) används här ett egetskrivet databasanrop som tar in ovaliderad data från service. | Implementera inputvalidering i service. |

### 3.3 EFBoxFile package

#### 3.3.1 EFBoxFile.java

EFBoxFile.java representerar filobjekt med ett Many To One förhållande til mapparna (eftersom flera filer kan finnas i samma mapp). Objektets egenskap 'content' av typ byte[] noteras men i övrigt inga anmärkningar.

#### 3.3.2 EFBoxFileDTO

Det noteras positivt att användaruppgifter (ID eller användarnamn) inte delas ut i DTO:et men i överigt inga anmärkningar.

#### 3.3.3 EFBoxFileController

EFBoxFileController.java exponerar alla endpoints relaterade till EFBoxFile-objektet.
Generellt skickar controllern förfrågan vidare till EFBoxFileService.java och hanterar svaret till användaren.

| Rad             | Fynd                                  | OWASP-referens                             | Kommentar                                                                                                                                                                                          | Åtgärd                                 |
| --------------- | ------------------------------------- | ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| 40, 65, 85, 107 | Automatiska felmeddelanden returneras | A10 Mishandling of Exceptional Conditions  | De olika funktionerna har fördefinierade meddelanden för undantagshantering men som backup för oförutsedda undantag skickas automatiska felmeddelanden. Risk för att känslig information avslöjas. | Implementera generiska felmeddelanden. |
| 29, 98          | bristande inputsvalidering            | A05 Injection                              | Varken `uploadFile()` eller `changeFileName()` har inputs validering för filnamnen. Risk för injection                                                                                             | Inplementera inputsvalidering          |
| -               | Hyfsad undatangshantering             | A10 Mishhandling of Exceptional Conditions | Med undantag för _bad requests_-undantagshanteringen har de flesta undantag som fångas egna informationsmeddelanden från de funktioner de respektivt anropar.                                      | N/A                                    |

#### 3.3.4 EFBoxFileService.java

EFBoxFileService behandlar logiken för hanteringen av User-objekt och CRUD-operationer via UserRepository.
| Rad | Fynd | OWASP-referens | Kommentar | Åtgärd |
|-|-|-|-|-|
| 34, 96 | Bristande inputvalidering | A05 Injection | `uploadFile()` och `changeFileName()` har ingen inputsvalidering. Det är allvarligt problem då funktionen som anropas i EFBoxFileRepository.java anropar databasen med en specialskriven fråga som tar in inputen obehandlad. Detta utgör en större risk för injektionsattacker. | Implementera inputsvalidering |
| 34 | Bristande filvalidering | relaterat till A05 Injection | Filerna accepteras som de är utan att kontrolleras för innehåll. | Implementera filvalidering |
| 34, 59, 76, 96 | Automatiska felmeddelanden returneras | A10 Mishandling of Exceptional Conditions | De olika funktionerna har fördefinierade meddelanden för undantagshantering men som backup för oförutsedda undantag skickas automatiska felmeddelanden. Risk för att känslig information avslöjas. | Implementera generiska felmeddelanden. |

#### 3.3.5 EFBoxFileRepository.java

EFBoxFileRepository.java hanterar kommunikationen mellan databasen och EFBoxFileService.

| Rad | Fynd                      | OWASP-referens | Kommentar                                                                                                                                             | Åtgärd                                  |
| --- | ------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 21  | Bristande inputvalidering | A05 Injection  | Som nämnts tidigare i [EFBoxFileService](#334-efboxfileservicejava) används här ett egetskrivet databasanrop som tar in ovaliderad data från service. | Implementera inputvalidering i service. |

#### 3.4 Security package

##### 3.4.1 GlobalCorsConfig.java

CORS-konfigurationen för EFBox, enligt beskrivningskommentaren genererades den koden av AI för eventuell frontendimplementering. Säkerhet är inte konfigurerad utan möjliggör CORS från alla origin : `config.addAllowedOrigin("*");`.

CORS kommer att implementera på ett annorlunda sätt inom SecurityConfig.java i enighet med Spring Boots instruktioner [1].

#### 3.4.2 JWTFilter.java

JWTFilter.java filtrerar det mottagna JWT vid auktoriseringen och skickar förfrågan och responsen vidare till nästa filter.

| Rad   | Fynd                                         | OWASP-referens                             | Kommentar                                                                                                                                                                                                                                                    | Åtgärd                                                        |
| ----- | -------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |
| 26    | Bristande JWT-validering                     | A07 Authentication Failures                | JWTs format ska vara en lång sammanhängande sträng med två stycken punkter (.). Det finns en möjlighet här att kontrollera att JWT inte innehåller mellanrum som en enklare validering.                                                                      | Validera JWT-format innan verifiering.                        |
| 41    | Spridning av lösenord                        | A04 Cryptographic Failures                 | När _authentication_ sätts skickas `user.getPassword()` (alltså det hashade lösenordet) med i auktoriseringen. Det hashade lösenordet kommer senare att finnas i SecurityContext och kan exponeras via loggning, debug-utskrifter eller serialisering (A08). | Ange inte lösenord i `UsernamePasswordAuthenticationToken`.   |
| 34–43 | Bristfällig felhantering vid JWT-verifiering | A09 Security Logging and Alerting Failures | Om `verifyAuthentication()` kastar ett undantag kan filtret ge ett generiskt serverfel utan en tydlig hantering av ogiltiga tokens.                                                                                                                          | Fånga verifieringsfel och returnera fördefinierat meddelande. |

#### 3.4.3 JWTService.java

JWTService.java hanterar JWT.

| Rad | Fynd                | OWASP-referens              | Kommentar                                                                                                                                                                                                                  | Åtgärd                                  |
| --- | ------------------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 33  | _Secret_ i klartext | A04 Cryptographic Failures  | `setSecretString()` hämtar en textrad från en .txt-fil i resources-mappen. Värdet som returneras används för HMAC256-algoritmen på nästa rad.                                                                              | Ersätt funktionen med miljövariabel.    |
| 44  | JWT-giltighetstid   | A07 Authentication Failures | JWTs giltighetstid sätts till 60 minuter, vilket inte är fel i sig tål att förkortas. Ett säkrare alternativ skulle vara kortare giltighetstid med en funktion för att förnya JWT:n vid autentiserade anrop från klienten. | Bestäm om alternativ ska implementeras. |

#### 3.4.4 PasswordConfig.java

PasswordConfig.java returnerar bara ett BCryptPasswordEncoder-objekt och använder BCrypt. Studien kommer att följa OWASP rekommendationer och använda Argon2.

#### 3.4.5 SecurityConfig.java

SecurityConfig.java konfigurerar Spring Securitys säkerhetsfilterkedja och definierar vilka endpoints som kräver autentisering.

| Rad   | Fynd                                         | OWASP-referens                | Kommentar                                                                                                                                                                                               | Åtgärd                                                   |
| ----- | -------------------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| 33    | CSRF inaktiverat                             | A02 Security Misconfiguration | CSRF är korrekt inaktiverat för ett stateless JWT-API då CSRF-skydd är avsett för sessionsbaserade applikationer.                                                                                       | N/A                                                      |
| 34–41 | Åtkomstkontroll                              | A01 Broken Access Control     | Publika endpoints begränsas till registrering och inloggning – alla övriga kräver autentisering.                                                                                                        | N/A                                                      |
| -     | CORS saknas i SecurityConfig                 | A02 Security Misconfiguration | Ingen CORS-konfiguration finns i SecurityConfig. GlobalCorsConfig som finns i projektet täcker inte applikationens faktiska endpoints (behandlas även i [GlobalCorsConfig](#341-globalcorsconfigjava)). | Konfigurera CORS explicit via `CorsConfigurationSource`. |
| -     | Ingen rate limiting på inloggningsendpointen | A07 Authentication Failures   | `/user/login` är publik utan begränsning på antal inloggningsförsök, vilket möjliggör brute force-attacker.                                                                                             | Implementera rate limiting på inloggningsendpointen.     |

### 3.6 Konfigurationsfiler och övriga resurser

#### 3.6.1 application.properties

Självförklarande.
Problem som behandlas i _Bilaga A - SonarCloud-analys (före åtgärder)_ behandlas inte i denna sektion\*.

| Rad | Fynd                          | OWASP-referens                | Kommentar                                                                           | Åtgärd                       |
| --- | ----------------------------- | ----------------------------- | ----------------------------------------------------------------------------------- | ---------------------------- |
| -   | Ingen konfiguration för HTTPS | A02 Security Misconfiguration | För att HTTPS ska fungera lokalt bör ett lokalt certifikat skapas och konfigureras. | Skriv HTTPS-konfigurationen. |

_\* Detta problem uppdagades i samband med förberedande forskning för denna studie utan direkta anvisningar från OWASP. För skapandet av ett lokalt certifikat används JDK:s eget `keytool`-verktyg. Inställningarna finns tillgängliga på [Spring Boots hemsida](https://docs.spring.io/spring-boot/how-to/webserver.html#howto.webserver.configure-ssl.pem-files)._

#### 3.6.2 secret.txt

Filen innehåller _secret_-strängen som används i [JWTService.java](#343-jwtservicejava) och ersätts av en miljövariabel.

#### 3.6.3 .gitignore

.gitignore innehåller filer som ska ignoreras av Git. I nuvarande läge är den i standardkonfiguration och ignorerar enbart systemfiler. När miljövariabler implementeras ska dessa ignoreras.

#### 3.6.4 build.gradle.kts

Konfiguration för Gradle-dependencies. Senaste commit för projektet kördes mot [osv.dev](https://osv.dev):s API utan att några sårbarheter upptäcktes (A03 Software Supply Chain Failures och A08 Software or Data Integrity Failures).

## 4. Problem som inte syns

Vissa säkerhetsbrister framgår inte av en fil-för-fil-granskning utan handlar om funktioner som saknas helt i applikationen.

### 4.1 Centraliserad undantagshantering

EFbox saknar ett centralt undantagshanteringssystem. Undantag hanteras lokalt i varje controller med risk för inkonsekvent beteende och informationsläckage. Ett centraliserat system skulle säkerställa att alla undantag hanteras på ett enhetligt och säkert sätt (A10 Mishandling of Exceptional Conditions).

### 4.2 Centraliserad säkerhetsloggning

Ingen säkerhetsloggning finns implementerad utöver Spring Boots standardlogging i terminalen. Misslyckade inloggningar, obehöriga åtkomstförsök och ogiltiga JWT-tokens loggas inte. Ett centraliserat loggningssystem bör implementeras (A09 Security Logging and Alerting Failures).

### 4.3 Centraliserat varningssystem

I samband med loggningen saknas ett varningssystem som varnar vid misstänkt aktivitet, t.ex. upprepade misslyckade inloggningsförsök eller ovanligt många förfrågningar från samma källa. Utan varning kan en pågående attack inte upptäckas i realtid (A09 Security Logging and Alerting Failures).

### 4.4 Lösenordsåterställning

EFbox saknar funktionalitet för lösenordsåterställning. Detta innebär att en användare som glömt sitt lösenord inte kan återfå åtkomst till sitt konto utan direktingripande i databasen. En säker återställningsfunktion kräver att användarens e-postadress lagras och verifieras (A07 Authentication Failures).

### 4.5 JWT-giltighetstid och utloggning

JWT-tokens i EFbox har en giltighetstid på 60 minuter och det finns ingen mekanism för att ogiltigförklara en token före dess utgång. Detta innebär att en utloggad användares token fortfarande är giltig tills den löper ut, vilket kan missbrukas vid tokenstöld.

JWT-token är immutable och går inte att ogiligförklarars. Alternativa lösningar är att skapa en sk. _black list_ av JWT som kontrolleras vid varje förfråga [2], eller att hantera giltighetstiden. Tidsbegränsningar på studien gör det svårt att uppnå en felfri lösning.
JWT är inte menade att användas på detta sätt. Studien kommer att utgå från kortare JWT giltighetstid med tanke på följande:

- om applikationen hade haft en fungerande frontend skulle JWT med extremt kort liv kunna förnyas regelbundet.
- om användaren är inaktiv en längre period (mät av frontend) skulle hen kunna loggas ut automatisk (dvs klienten slutar be om nya JWT)

Av praktiska skäl kommer JWT ha en TTL (_Time To Live_) på 10 minuter och förnyas med varje förfrågan. //TODO: ändra om brist på tid för implementering.

## Referenser

[1]: Cors-configuration, Spring Boot, Accessed May 2026. Available: https://docs.spring.io/spring-security/reference/servlet/integrations/cors.html#page-title

[2]: JWT Blacklisting in Spring Boot for Revoked Sessions, Alexander Obregon, Medium, Published: August 2025. Avalailble: https://medium.com/@AlexanderObregon/jwt-blacklisting-in-spring-boot-for-revoked-sessions-9041592585be
