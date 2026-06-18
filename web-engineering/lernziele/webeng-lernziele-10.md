---
course: web-engineering
type: lernziele
lecture: 10
date: 2026-05-12
tags:
  - web-engineering
  - lernziele
  - java
  - servlets
  - jsp
  - container
  - lebenszyklus
  - annotationen
  - tag-libraries
  - mvc
  - flashcards/web-engineering
source: 15-Java.pdf
---

# Lernziele — Vorlesung 10 (Java: Servlets & JSP)

**Quelle:** [[web-engineering/resources/15-Java.pdf|15-Java.pdf]]

## Fragen

Warum kommt ein Servlet ohne main-Methode aus?::Der gesamte Lebenszyklus wird vom **Container** (Servlet-Engine, z.B. Tomcat) gesteuert. Der Container lädt und instanziiert das Servlet und ruft die Lifecycle-Methoden `init`, `service` und `destroy` auf — es gibt keinen eigenen Einstiegspunkt wie `main`.

Wie realisiert man eigene Logik über ein Servlet?::Klasse von `HttpServlet` ableiten und `doGet`/`doPost` (oder generisch `service`) überschreiben. Eingaben über `HttpServletRequest` lesen (`getParameter("…")`), Antwort über `HttpServletResponse` erzeugen (Content-Type setzen, `PrintWriter` holen, HTML schreiben, ggf. Cookies/Header setzen).

Wie hängen Container und Lebenszyklus eines Servlets zusammen?::Der **Container** verwaltet das Servlet über seinen gesamten **Lebenszyklus**: Laden/Instanziieren (on demand oder bei `<load-on-startup>1</load-on-startup>` beim Start), `init()` für globale Initialisierung, mehrfaches `service()`/`doGet`/`doPost` (ein Thread pro Anfrage, dieselbe Instanz → thread-safe!), schließlich `destroy()` und Entladen der Klasse.

Wieso spielen Annotationen im Zusammenhang mit dem Servlet-Container eine Rolle?::Seit der **Servlets-3.0-API** helfen Annotationen dem Container, ein Servlet passend einzubinden und Funktionalitäten zu erkennen — z.B. `@WebServlet("/route")` bindet die Klasse an eine URL, `@WebFilter` realisiert eine Vorverarbeitung (Middleware-ähnlich). Sie ersetzen die Konfiguration in der `web.xml`.

Was ist die Idee von Java Server Pages (JSP)?::**Umkehrung des Servlet-Prinzips**: statt „Java-Code erzeugt HTML" sind JSP **HTML-Seiten mit eingebettetem Java-Code** (spezielle Tags, analog zum PHP-/Template-Prinzip). Der Server erkennt JSP, **kompiliert sie automatisch (einmalig) in ein Servlet** und nutzt danach direkt die übersetzte Klasse.

Was sind Tag-Libraries und welche Vorteile bieten sie bei der Entwicklung?::Sammlungen wiederverwendbarer, benannter **Custom Tags**, die Java-Logik hinter HTML-ähnlichen Tags kapseln. Vorteil: **möglichst wenig Java-Code in der View**, saubere Trennung von Darstellung und Logik, bessere Lesbarkeit/Wartbarkeit und Wiederverwendbarkeit der Bausteine.

Wie lässt sich das MVC-Modell mit Servlets in Beziehung setzen?::**Controller = Servlet** (liest/prüft Parameter, ruft die Logik auf, wählt die View), **Model = Java Beans** (Daten + Geschäftslogik, unabhängig von der Webschnittstelle), **View = JSP** (Anzeige des Ergebnisses, möglichst wenig Java-Code). Der Servlet-Container hostet alle drei; Frameworks wie **Struts** (`ActionServlet`) oder **Spring** liefern einen generischen Controller.
