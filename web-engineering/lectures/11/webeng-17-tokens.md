---
course: web-engineering
type: lecture
lecture: 11
date: 2026-06-16
tags:
  - web-engineering
  - lecture
  - oauth2
  - openid-connect
  - jwt
  - bearer-token
  - access-token
  - id-token
  - refresh-token
  - authorization-code-flow
  - api-key
  - authentifizierung
  - autorisierung
  - security
source: 17-Tokens.pdf
---

# 17 — Tokens (OAuth2 & OpenID Connect)

**Folien:** [[web-engineering/resources/17-Tokens.pdf|17-Tokens.pdf]]
**Lernziele:** [[web-engineering/lernziele/webeng-lernziele-11|Lernziele Vorlesung 11]]

> [!info] Hinweis
> Woche 11 umfasst zwei Foliensätze: [[web-engineering/lectures/11/webeng-16-https|16 — HTTPS (TLS, PKI & Zertifikate)]] und **17 — Tokens** (diese Notiz). Diese Notiz behandelt die Lernziele zu **OAuth2/OpenID Connect** und **Bearer-Token** (die letzten beiden Karten); die Lernziele zu TLS/PKI/Zertifikaten/Heartbleed/HSTS/SNI werden in der HTTPS-Notiz behandelt.

## Inhaltsverzeichnis

- [[#Authentifizierung vs. Autorisierung|Authentifizierung vs. Autorisierung]]
- [[#Token-basierte Authentifikation|Token-basierte Authentifikation]]
- [[#JSON Web Token (JWT)|JSON Web Token (JWT)]]
- [[#OAuth2|OAuth2]]
- [[#OpenID Connect (OIDC)|OpenID Connect (OIDC)]]
- [[#Access Token vs. ID Token|Access Token vs. ID Token]]
- [[#Bearer-Token in der Praxis|Bearer-Token in der Praxis]]
- [[#Refresh Token|Refresh Token]]
- [[#JWT mit Express (einfaches Beispiel)|JWT mit Express (einfaches Beispiel)]]
- [[#API-Keys — Identifikation von Anwendungen|API-Keys — Identifikation von Anwendungen]]
- [[#Bezug zu Lernzielen|Bezug zu Lernzielen]]

---

## Authentifizierung vs. Autorisierung

Zwei Begriffe, die im Kontext von Web-Security streng auseinandergehalten werden müssen:

| Aspekt | Authentifizierung (Authentication) | Autorisierung (Authorization) |
|---|---|---|
| Frage | **Wer bist du?** | **Was darfst du?** |
| Zweck | Identität eines Nutzers nachweisen | Zugriff auf geschützte Ressourcen erlauben |
| Beleg | ID Token (OpenID Connect) | Access Token (OAuth2) |
| Enthält | `sub`, `email`, `name` (Identitätsdaten) | `scope`, Berechtigungen |

> [!tip] Merke
> **OAuth2 klärt „darf" (Autorisierung), OpenID Connect klärt „wer" (Authentifizierung).** OAuth2 liefert eigentlich **keine** Aussage über die vorher notwendige Authentifikation — diese Lücke schließt OIDC als Schicht oben auf OAuth2.

---

## Token-basierte Authentifikation

Moderne Web-Schnittstellen verwenden gerne eine besondere Art der Autorisierung, die **zwingend mit HTTPS** verwendet werden sollte.

- Basis ist ein **Token** — eine Art Geheimnis, welches man mit jeder Anfrage mitpräsentieren muss.
- Dieses Geheimnis erhält man **nach erfolgreicher Authentifikation** (ggf. auch bei einem externen Anbieter) und speichert es zwischen.
- Ein Token hat eine **Struktur** und eine **Signatur**.
- Der Token wird als **HTTP-Authorization-Header vom Typ `Bearer`** übertragen.

> [!warning] Achtung
> Ein Token ist ein Geheimnis. Wird es über eine unverschlüsselte Verbindung übertragen, kann es abgefangen und missbraucht werden → **Token nur über TLS/HTTPS übertragen.**

---

## JSON Web Token (JWT)

Ein JWT ist **eine Möglichkeit**, ein Token umzusetzen — und zusammen mit OAuth2 die de-facto Standardlösung.

Aufbau aus drei durch Punkte getrennten, jeweils **base64-codierten** Teilen:

```
aaaaaaaaaa.bbbbbbbbbbbb.cccccccccccc
   Header  .   Payload  .  Signature
```

```mermaid
flowchart LR
    H["Header<br/>(JSON, base64)<br/>alg, typ"]
    P["Payload<br/>(JSON, base64)<br/>Claims: sub, iss, aud, exp, ..."]
    S["Signature<br/>(base64)<br/>über Header+Payload+Secret"]
    H --- P --- S
```

| Teil | Inhalt |
|---|---|
| **Header** | gibt im JSON-Format an: `typ` = JWT und den Hash-/Signatur-Algorithmus (`alg`, z.B. `RS256`, `HS256`) |
| **Payload** | wichtige Informationen als **Claims**: Subject (`sub`), Issuer (`iss`), Audience (`aud`), Expiration (`exp`), Not-valid-before (`nbf`), Ausstellzeitpunkt (`iat`) |
| **Signature** | Signatur über Header, Payload und ein zusätzliches **Secret** — sichert Integrität und Echtheit |

> [!quote] Definition (Claims)
> **Claims** sind die im Payload transportierten Aussagen über das Subjekt und das Token selbst (z.B. „wer" per `sub`, „von wem ausgestellt" per `iss`, „für wen bestimmt" per `aud`, „gültig bis" per `exp`).

> [!warning] Achtung
> Payload und Header sind nur **base64-codiert, nicht verschlüsselt** — jeder kann sie lesen. Die Signatur schützt nur vor **Veränderung**, nicht vor Einsicht. Daher gehören keine Geheimnisse in den Payload.

---

## OAuth2

> [!quote] Definition (OAuth2)
> **OAuth2** ist ein **verteiltes Autorisierungsprotokoll** mit standardisierter Programmierschnittstelle (offengelegtes Protokoll). Es realisiert einen **Token-basierten Schutz auf Ressourcen**.

**Vier Rollen:**

| Rolle | Bedeutung |
|---|---|
| **Resource Owner** | der Nutzer, dem die Ressource/Daten gehören und der die Zustimmung erteilt |
| **Resource Server** | Server, der die geschützten Ressourcen bereithält (prüft das Token) |
| **Client** | die Anwendung, die im Namen des Nutzers auf Ressourcen zugreifen will |
| **Authorization Server** | stellt nach erfolgreicher Autorisierung das **Access Token** aus |

- Zusammen mit **JSON Web Tokens (JWT)** die de-facto Standardlösung zur Autorisierung.
- Liefert jedoch eigentlich **keine Aussagen über die vorher notwendige Authentifikation** — dafür wird OIDC ergänzt.

```mermaid
sequenceDiagram
    participant RO as Resource Owner (Nutzer)
    participant C as Client (App)
    participant AS as Authorization Server
    participant RS as Resource Server
    C->>RO: Weiterleitung zum Authorization Server
    RO->>AS: Login + Zustimmung (Authentifikation)
    AS-->>C: Authorization Code
    C->>AS: Code + Client-Credentials
    AS-->>C: Access Token (JWT)
    C->>RS: Anfrage mit Authorization: Bearer <token>
    RS->>RS: Token/Signatur prüfen
    RS-->>C: geschützte Ressource
```

---

## OpenID Connect (OIDC)

> [!quote] Definition (OpenID Connect)
> **OpenID Connect** ist ein verteiltes **Authentifizierungssystem** in der Web-Landschaft — ein offengelegtes Protokoll, das **auf dem OAuth 2.0-Protokoll basiert** und dieses im Sinne der Authentifikation erweitert (Autorisierung wird genutzt, um einen **IdP** zu befragen).

Weitere Eigenschaften:

- Ausschließlich unter **Open Source** bereitgestellte Software.
- Dient der **föderierten Authentifikation** (Login über bestehende Identitäten wie Google, GitHub …).
- **Relativ einfach** für die Nutzer und daher gerne im Web-Kontext genutzt.

> [!tip] Merke
> OpenID Connect nutzt OAuth 2 als Grundlage für die Autorisierung und erweitert es um **Authentifizierung**. Es ermöglicht die **Wiederverwendung bestehender Identitäten**, sodass **keine eigene Benutzerdatenbank** erforderlich ist.

```mermaid
flowchart TB
    subgraph OIDC["OpenID Connect — Authentifizierung ('wer')"]
        subgraph OAuth["OAuth2 — Autorisierung ('darf')"]
            T["Token-basierter Ressourcenschutz"]
        end
        ID["ID Token (JWT) — Identitätsnachweis"]
    end
```

**Föderierter Ablauf (nach Mozilla-Guideline):**

```mermaid
sequenceDiagram
    participant RP as Relying Party (Web-App)
    participant B as Web Browser (Login-Form)
    participant OP as OpenID Connect Provider (OP)
    participant IdP as Identity Providers (SAML, LDAP/AD, GitHub, Google, ...)
    RP->>B: (1) Redirect zum OP-Auth-Formular (mit Request)
    B->>OP: (2) Authenticate (LDAP, Password-less, GitHub, Google, ...)
    OP->>IdP: Anfrage an Identity Provider
    IdP-->>OP: Bestätigung
    OP-->>B: (3) Redirect zur Post-Login-URL mit<br/>Authorization Code Grant, Access Token, ID Token, User-Profil
```

- Die **Relying Party (RP)** ist die eigene Web-Anwendung.
- Der **OpenID Connect Provider (OP)** ist ein Front-end zu den Identity Providern und kann auch selbst einen IdP bereitstellen.
- Als **Identity Providers (IdP)** kommen z.B. SAML, WS-Federate, LDAP/AD, GitHub OIDC, Google OIDC in Frage.

---

## Access Token vs. ID Token

Beide sind typischerweise JWTs, haben aber unterschiedliche Zwecke:

| | **ID Token** | **Access Token** |
|---|---|---|
| Aussage | *The user has been authenticated* | *The app has been authorized* |
| Spezifikation | OpenID Connect | OAuth |
| Zweck | **Authentifizierung** (wer ist der Nutzer) | **Autorisierung** (worauf darf zugegriffen werden) |
| Format | **muss** ein JWT sein | beliebige Struktur (String), oft auch ein JWT |
| Typische Claims | `sub`, `given_name`, `family_name`, `iss`, `aud`, `exp`, `iat` | `sub`, `iss`, `aud`, `scope`, `gty` (grant type, z.B. `authorization_code`) |
| Enthält | *identification information* | *authorization information* |

> [!warning] Achtung — verbreitete Praxis in der Realität
> Viele **Identity Provider (Google, Microsoft, Auth0)** geben **nur einen Access Token** zurück, der **beide Funktionen** übernimmt:
> - **Authentifizierung:** enthält `sub`, `email`, `name` (wie ein ID Token)
> - **Autorisierung:** enthält `scopes` (wie ein Access Token)
>
> Viele Entwickler nutzen damit faktisch nur **OAuth2 mit implizitem OIDC**, ohne wirklich einen ID Token zu validieren.

---

## Bearer-Token in der Praxis

> [!quote] Definition (Bearer)
> Ein **Bearer Token** ist ein Token, bei dem allein der **Besitz** zum Zugriff berechtigt — „wer es vorzeigt, darf" (engl. *bearer* = Überbringer). Es ist kein weiterer Identitätsnachweis nötig.

Übertragung im HTTP-Header:

```
GET /books HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6...
Host: localhost:3000
```

Typischer Ablauf mit einem signierten JWT als Access Token:

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    C->>S: Login Request (username/password)
    S-->>C: Signed JWT (accessToken)
    Note over C: Token wird zwischengespeichert
    C->>S: Data Request + Authorization: Bearer <JWT>
    S->>S: Signatur mit Secret verifizieren
    S-->>C: Response (geschützte Daten)
```

> [!warning] Achtung
> Da beim Bearer-Token der bloße Besitz genügt, kann es bei Abfangen missbraucht werden. Konsequenzen: **zwingend nur über TLS** übertragen, **kurze Gültigkeit** (`exp`) und **sichere Speicherung** auf dem Client.

---

## Refresh Token

Damit Access Tokens kurzlebig sein können, ohne den Nutzer ständig neu einloggen zu lassen, gibt es zusätzlich **Refresh Tokens**.

- **Authentication + API calls:** nach dem Login erhält der Client **Access Token + Refresh Token**; die API-Aufrufe erfolgen mit dem Access Token.
- **Access Token Expires:** ist das Access Token abgelaufen, sendet der Client das **Refresh Token** und erhält ein **neues Access Token (+ Refresh Token)** — ohne erneute Anmeldung.
- **Refresh + Access Token Expires:** sind **beide** abgelaufen, wird der Nutzer **ausgeloggt** und muss sich **neu authentifizieren**.

> [!tip] Merke
> Access Token = kurzlebig, wird bei jeder Anfrage mitgeschickt. Refresh Token = langlebiger, dient nur dazu, neue Access Tokens zu beschaffen.

---

## JWT mit Express (einfaches Beispiel)

Ein minimaler Authentifizierungs-Service mit dem Modul `jsonwebtoken`. Er stellt beim Login ein signiertes JWT aus und schützt danach eine Route per Middleware.

**Setup und Nutzer:**

```js
const express = require('express');
const app = express();
const jwt = require('jsonwebtoken');

const accessTokenSecret = 'mein access token secret';

app.use(express.json());

const users = [
  { username: 'volker',  password: 'password123admin',  role: 'admin'  },
  { username: 'sander',  password: 'password123member', role: 'member' }
];
```

**Login — Token ausstellen (`jwt.sign`):**

```js
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username && u.password === password);
  if (user) {
    // Access Token mit den Claims username + role signieren
    const accessToken = jwt.sign({ username: user.username, role: user.role }, accessTokenSecret);
    res.json({ accessToken });
  } else {
    res.send('Username or password incorrect');
  }
});
```

**Middleware — Token prüfen (`jwt.verify`):**

```js
const authenticateJWT = (req, res, next) => {
  const authHeader = req.headers.authorization;
  if (authHeader) {
    const token = authHeader.split(' ')[1];      // "Bearer <token>" → <token>
    jwt.verify(token, accessTokenSecret, (err, user) => {
      if (err) return res.sendStatus(403);        // Token ungültig → Forbidden
      req.user = user;
      next();
    });
  } else {
    res.sendStatus(401);                          // kein Header → Unauthorized
  }
};
```

**Geschützte Route:**

```js
const books = [
  { author: 'a', language: 'aa', title: 'Erster'  },
  { author: 'b', language: 'bb', title: 'Zweiter' },
  { author: 'c', language: 'vc', title: 'Dritter' }
];

app.get('/books', authenticateJWT, (req, res) => {
  res.json(books);
});

app.listen(3000, () => console.log('Authentication service started on port 3000'));
```

> [!warning] Achtung — Statuscodes
> - **401 Unauthorized:** gar kein `Authorization`-Header vorhanden (nicht authentifiziert).
> - **403 Forbidden:** Header vorhanden, aber Token-Signatur ungültig/abgelaufen (nicht autorisiert).

**Testablauf (z.B. mit einem REST-Client):**

1. `POST http://localhost:3000/login` mit Body `{"username":"volker","password":"password123admin"}` → liefert `{ accessToken: "eyJ..." }`.
2. `GET http://localhost:3000/books` mit Header `Authorization: Bearer eyJ...` → liefert die Bücherliste (GET hat keinen Body).

> [!warning] Achtung
> Im Beispiel steht das `accessTokenSecret` im Klartext im Code und die Passwörter sind unverschlüsselt — das ist nur zu Lehrzwecken. In Produktion: Secrets aus der Umgebung laden, Passwörter hashen, Token mit `exp` versehen und ausschließlich HTTPS verwenden.

---

## API-Keys — Identifikation von Anwendungen

Abzugrenzen von Tokens sind **API-Keys**:

> [!tip] Merke
> **Tokens** sind private, **personenbezogene** Hilfsmittel. **API-Keys** sollen dagegen eine **Anwendung oder ein Projekt** identifizieren.

| | **Token** | **API-Key** |
|---|---|---|
| Identifiziert | einen **Nutzer/Person** | eine **Anwendung/Projekt** |
| Erzeugung | **kontinuierlich** (bei jeder Sitzung neu) | **einmalig** erzeugt |
| Zweck | Authentifizierung + Autorisierung | Zuordnung, Statistik, Billing |

- Faktisch werden API-Keys gerne genutzt, um Aufrufe Anwendungen, Projekten oder gar Personen zuzuordnen. Diese Zuordnung ist jedoch **locker** und eher für **statistische Zwecke** geeignet — nicht um den Zugang wirklich bestimmten Benutzern zuzuordnen.
- **Beispiel:** Google Maps benötigt einen API-Key, den man sich erzeugen muss; er identifiziert das Projekt für **Nutzung und Abrechnung**.

---

## Bezug zu Lernzielen

Die kompakten Karteikarten finden sich unter [[web-engineering/lernziele/webeng-lernziele-11|Lernziele Vorlesung 11]]. Diese Notiz deckt die Lernziele zu **OAuth2/OpenID Connect** und **Bearer-Token** ab; die Lernziele zu **TLS, PKI, Zertifikatsketten, Heartbleed, HSTS und SNI** werden in [[web-engineering/lectures/11/webeng-16-https|16 — HTTPS (TLS, PKI & Zertifikate)]] behandelt.

**Was sind die Grundlagen von OAuth2 und OpenID Connect?**

**OAuth2** ist ein verteiltes, offengelegtes **Autorisierungs-Framework** mit standardisierter Schnittstelle und Token-basiertem Schutz auf Ressourcen. Es kennt **vier Rollen**: den **Resource Owner** (Nutzer, dem die Daten gehören), den **Client** (die zugreifende Anwendung), den **Authorization Server** (stellt das Access Token aus) und den **Resource Server** (hält die geschützten Ressourcen und prüft das Token). Nach Zustimmung des Nutzers erhält der Client vom Authorization Server ein **Access Token** (üblicherweise ein JWT) und greift damit im Namen des Nutzers auf Ressourcen zu — gängig ist der **Authorization-Code-Flow**. OAuth2 klärt also nur, **was** ein Client darf, und trifft eigentlich **keine Aussage über die vorher notwendige Authentifikation**.

**OpenID Connect (OIDC)** ist eine **Authentifizierungs-Schicht auf OAuth2**: Es basiert auf dem OAuth-2.0-Protokoll und erweitert es um die Authentifizierung. Zusätzlich zum Access Token wird ein signiertes **ID Token (JWT)** ausgestellt, das die **Identität** des Nutzers belegt (`sub`, `email`, `name`). OIDC ermöglicht **föderierte Authentifikation** — Login über bestehende Identitäten (Google, GitHub, LDAP/AD …) über einen **OpenID Connect Provider (OP)**, der als Front-end zu Identity Providern dient. Dadurch ist **keine eigene Benutzerdatenbank** nötig. Kurz: **OAuth2 klärt „darf", OIDC klärt „wer".** In der Praxis liefern viele IdPs nur einen Access Token, der beide Funktionen (Identitäts-Claims + `scopes`) übernimmt — faktisch OAuth2 mit implizitem OIDC.

**Wozu dienen Bearer-Token?**

Ein **Bearer Token** ist ein Token, bei dem allein der **Besitz** zum Zugriff berechtigt („wer es vorzeigt, darf") — ohne weiteren Identitätsnachweis (*bearer* = Überbringer). Es wird im HTTP-Header **`Authorization: Bearer <token>`** mitgeschickt; typischerweise ist das das **OAuth2-Access-Token**, oft in Form eines JWT. Der Server prüft bei jeder Anfrage die **Signatur** des Tokens gegen sein Secret (`jwt.verify`) und gewährt bei Gültigkeit Zugriff (sonst 401 ohne Header bzw. 403 bei ungültigem Token). Die **Konsequenz** des reinen Besitz-Prinzips: Wird das Token abgefangen, kann es missbraucht werden. Deshalb müssen Bearer-Token **zwingend nur über TLS/HTTPS** übertragen werden, eine **kurze Gültigkeit** (`exp`, ergänzt durch Refresh Tokens) besitzen und **sicher gespeichert** werden.
