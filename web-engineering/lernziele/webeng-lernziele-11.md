---
course: web-engineering
type: lernziele
lecture: 11
date: 2026-05-18
tags:
  - web-engineering
  - lernziele
  - tls
  - ssl
  - https
  - zertifikate
  - zertifikatskette
  - pki
  - tls-terminierung
  - buffer-overflow
  - heartbleed
  - hsts
  - sni
  - oauth2
  - openid-connect
  - bearer-token
  - security
  - flashcards/web-engineering
---

# Lernziele — Vorlesung 11 (TLS, PKI & Authentifizierung)

## Fragen

Wie funktioniert TLS und welchen Zweck erfüllt es?::TLS (Transport Layer Security, Nachfolger von SSL) sichert die Verbindung zwischen Client und Server mit drei Zielen: **Vertraulichkeit** (Verschlüsselung), **Integrität** (MAC) und **Authentizität** (Server-, optional Client-Zertifikat). Ablauf: Im **Handshake** einigen sich beide auf Cipher-Suite und Version, der Server weist sich per **Zertifikat** aus, per **asymmetrischer Kryptografie** (bzw. Diffie-Hellman) wird ein gemeinsames **symmetrisches Sitzungsschlüssel** ausgehandelt; die eigentlichen Nutzdaten werden dann **symmetrisch** (schneller) verschlüsselt.

Was ist eine Zertifikatskette und wie wird sie geprüft?::Eine Vertrauenskette vom **Server-Zertifikat** über ein oder mehrere **Intermediate-CA-Zertifikate** bis zum **Root-CA-Zertifikat**. Jedes Zertifikat ist von der jeweils übergeordneten CA **signiert**; das Root-Zertifikat ist **selbstsigniert** und liegt im **Trust Store** von Browser/OS. Der Client validiert die Kette, indem er jede Signatur bis zu einer vertrauenswürdigen Root prüft (sowie Gültigkeitszeitraum, Hostname/CN/SAN und Sperrstatus via CRL/OCSP).

Was ist TLS-Terminierung und wie ordnet sich TLS in HTTP ein?::**TLS-Terminierung** bedeutet, dass die verschlüsselte Verbindung an einem vorgelagerten Punkt (z.B. **Reverse Proxy / Load Balancer**) entschlüsselt wird; dahinter läuft der Verkehr oft unverschlüsselt im internen Netz. TLS sitzt **zwischen Transport- (TCP) und Anwendungsschicht** — **HTTPS = HTTP über TLS**: HTTP selbst bleibt unverändert, die Nachrichten werden nur durch die TLS-Schicht geschleust.

Warum braucht man eine PKI und welche Schwächen hat TLS?::Eine **PKI (Public Key Infrastructure)** bindet öffentliche Schlüssel verlässlich an Identitäten — über **CAs**, die Zertifikate ausstellen/signieren, plus Registrierung, Verteilung und **Sperrung** (CRL/OCSP). Ohne PKI wäre ein öffentlicher Schlüssel nicht vertrauenswürdig einer Gegenstelle zuzuordnen (MitM). **Schwächen von TLS:** kompromittierte oder bösartige CAs (Fehl-Ausstellung), Downgrade-/Protokollangriffe, schwache/veraltete Cipher-Suites, Implementierungsfehler (z.B. Heartbleed) und mangelhafte Zertifikatsprüfung auf Clientseite.

Was ist die Buffer-Overflow-Problematik — erklärt am Beispiel von Heartbleed?::Ein **Buffer Overflow** entsteht, wenn über die Grenzen eines Puffers hinaus geschrieben/gelesen wird, weil Längenangaben nicht geprüft werden. **Heartbleed** (CVE-2014-0160) war ein **Buffer-Over-Read** in der OpenSSL-Heartbeat-Erweiterung: Der Client gab eine Payload-**Länge** an, die größer war als die tatsächlich gesendeten Daten; der Server kopierte blind so viele Bytes zurück und gab dadurch **angrenzenden Speicher** preis — darunter private Schlüssel, Sessions und Passwörter. Ursache: fehlende **Längenvalidierung**.

Welche Bedeutung haben HSTS und SNI?::**HSTS (HTTP Strict Transport Security)** ist ein Response-Header (`Strict-Transport-Security`), der dem Browser vorschreibt, eine Domain **nur noch über HTTPS** anzusprechen — schützt vor SSL-Stripping/Downgrade und Klartext-Erstaufrufen. **SNI (Server Name Indication)** ist eine TLS-Erweiterung, bei der der Client den **gewünschten Hostnamen schon im Handshake** mitsendet, damit ein Server mit **mehreren Zertifikaten/Domains auf einer IP** das passende Zertifikat ausliefern kann.

Was sind die Grundlagen von OAuth2 und OpenID Connect?::**OAuth2** ist ein **Autorisierungs**-Framework: Ein Client erhält nach Zustimmung des Nutzers vom Authorization Server ein **Access Token**, mit dem er im Namen des Nutzers auf geschützte Ressourcen zugreift (Rollen: Resource Owner, Client, Authorization Server, Resource Server; gängig der Authorization-Code-Flow). **OpenID Connect (OIDC)** ist eine **Authentifizierungs**-Schicht **auf OAuth2**: Zusätzlich zum Access Token wird ein signiertes **ID Token (JWT)** ausgestellt, das die Identität des Nutzers belegt — OAuth2 klärt „darf", OIDC klärt „wer".

Wozu dienen Bearer-Token?::Ein **Bearer Token** ist ein Token, bei dem allein der **Besitz** zum Zugriff berechtigt („wer es vorzeigt, darf") — ohne weiteren Identitätsnachweis. Es wird im HTTP-Header `Authorization: Bearer <token>` mitgeschickt (typisch das OAuth2-Access-Token, oft ein JWT). **Konsequenz:** Wird das Token abgefangen, kann es missbraucht werden → zwingend **nur über TLS** übertragen, kurze Gültigkeit und sichere Speicherung.
