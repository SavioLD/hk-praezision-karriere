# HK‑Präzisionstechnik – Karriereseite (Beispiel)

Recruiting-Landingpage für die **HK-Präzisionstechnik GmbH**, Oberndorf am Neckar.
Ausgeschriebene Stellen: **Konstrukteur (m/w/d)** und **Servicetechniker (m/w/d)**,
und ausschließlich diese zwei. Keine Initiativbewerbung, kein Bereich „weitere
Stellen“, keine Verlinkung auf andere Vakanzen.

Aufbau 1:1 wie die ALWA-Seite (`SavioLD/alwa-gmbh`): Topbar → Hero → Vertrauensleiste →
Stellen → Warum HK → Ablauf → CTA → Bewerbungsformular → FAQ → Footer.

> **Beispielseite für das Angebot.** Aufgaben, Voraussetzungen und Benefits sind
> typische Beispiele für die Stellen und noch **nicht mit HK abgestimmt**. Deshalb gilt:
> Leiste „Beispielseite“ oben, `noindex`, keine JobPosting-Daten für Google,
> kein Versand ohne Webhook. Livegang siehe unten.

## Inhalt

- `index.html` – die komplette Seite (self-contained, kein Build-Schritt)
- `fonts/` – Barlow und Barlow Condensed lokal (keine Verbindung zu Google)
- `bilder/` – Logo und Fotos (siehe `bilder/HIER-BILDER-ABLEGEN.txt`)
- `.nojekyll` – GitHub Pages liefert die Dateien 1:1 aus

## Live schalten (GitHub Pages)

1. Repo-Settings → **Pages** → Source: **Deploy from a branch**, Branch `main` / `(root)`
2. Nach ein paar Minuten unter `https://saviold.github.io/hk-praezision-karriere/`

## CI

Farben und Schriften stehen **ausschließlich** im `:root`-Block oben in `index.html`.
hk-prt.de war aus der Arbeitsumgebung nicht erreichbar. Die Farben sind deshalb aus dem
**Logo in der E-Mail-Signatur** ausgelesen, identisch mit dem Website-Entwurf
(`SavioLD/hk-praezision`):

```css
--brand:#586860;   /* Schiefergrün (Logo)            */
--accent:#8fb1a0;  /* Salbeigrün (Firmenname Signatur) */
--panel-2:#e7e2dd; /* Steingrau (Logo)               */
--f-display:"Barlow Condensed"; --f-body:"Barlow";
```

Liegen Styleguide oder Screenshots der aktuellen Website vor, nur diesen Block anpassen.

## Stellen pflegen

Beide Stellen stehen in **einer** Config (`var JOBS` im `<script>` am Seitenende).
Sie speist Stellenkarten, Stellenauswahl, **Fragen im Formular**, Hero-Text bei
`?stelle=…`, FAQ „Voraussetzungen“ und (nach Freigabe) die JobPosting-Daten.

| | Konstrukteur (m/w/d) | Servicetechniker (m/w/d) |
|---|---|---|
| **Pflicht (K.-o.)** | Studium Maschinenbau o. Ä. oder staatl. gepr. Techniker · mind. 2 Jahre Konstruktion · sicher in 3D-CAD | Technische Berufsausbildung · Bereitschaft zu mehrtägigen Einsätzen im In- und Ausland · Führerschein Klasse B |
| **Wünschenswert** | Vorrichtungs-/Sondermaschinen-/Werkzeugbau · gutes Englisch · OEM-/Automotive-Projekte | Montage/Inbetriebnahme/Service · gutes Englisch · Steuerungstechnik (SPS) |
| **Benefits** | Kernteam, kurze Wege · flexible Arbeitszeiten · unbefristet | Auslöse und Reisezulagen · Firmenwagen · unbefristet |

## Stellen-Deeplinks für die Anzeige

Die Seite textet dann den Hero auf die Stelle, wählt sie vor und **überspringt den
Auswahl-Schritt**: Aus 7 Schritten werden 6.

- `…/?stelle=konstrukteur` (auch `konstruktion`, `cad`)
- `…/?stelle=servicetechniker` (auch `service`, `monteur`, `inbetriebnahme`)

## Vorfilterung (Screening)

**Eine Frage pro Schritt**: davor die Stellenauswahl (entfällt beim Deeplink),
danach die Kontaktdaten. Die Fragen sind **je Stelle unterschiedlich**: 3 Pflicht + 2 optional.

| # | Konstrukteur | Servicetechniker | Kategorie |
|---|---|---|---|
| 1 | Qualifikation (Studium / Techniker) | Ausbildung (technisch) | Pflicht |
| 2 | Erfahrung Konstruktion (mind. 2 Jahre) | Einsätze im In- und Ausland | Pflicht |
| 3 | 3D-CAD (sicher / gut) | Führerschein Klasse B | Pflicht |
| 4 | Vorrichtungs-/Sondermaschinenbau | Montage/Inbetriebnahme/Service | optional |
| 5 | Englischkenntnisse | Englischkenntnisse | optional |

Das dritte Wunschkriterium jeder Stelle (OEM-Projekte bzw. SPS) steht nur in der
Anzeige, damit es bei höchstens 5 Fragen bleibt.

**Logik**

- **Pflichtfrage nicht erfüllt** → Bewerbung endet sofort mit einem freundlichen
  Abschlusshinweis. Keine weiteren Schritte, **kein Lead an die Lead Table**, keine
  anderen Stellen angeboten.
- **Optionale Frage nicht erfüllt** → Bewerber kommt normal weiter. Die Antwort wird
  übertragen und im Feld `nicht_erfuellt` markiert.
- Welche Antwort ein Kriterium erfüllt, steht in der Config als `ok:true`.
- Ein Konsistenz-Check meldet in der Konsole, wenn Pflichtfragen und
  „Das setzen wir voraus“ nicht zusammenpassen.

**Kein Lebenslauf-Upload, keine Dateianhänge.** Abgefragt werden nur
berufsbezogene Kriterien: nichts zu Alter, Herkunft, Gesundheit, Religion oder
Familienstand (AGG).

## Mobile Laufruhe

Beim Schrittwechsel ändert die Seite nur CSS-Klassen. Es gibt **kein** `window.scrollTo`,
**kein** `scrollIntoView`, **kein** `focus()`, keinen Reload und keinen Hash-Sprung.
(Das einzige `scrollIntoView` sitzt auf den „Jetzt bewerben“-Buttons der
Stellenkarten. Das ist gewollte Navigation zum Formular, kein Schrittwechsel.)
Der Formularcontainer hat eine feste Mindesthöhe (`--form-min` 520 px, `--form-min-mobile`
496 px). Sie entspricht dem höchsten Schritt, deshalb sind alle Schritte gleich hoch
und der Weiter-Button steht immer an derselben Stelle.

Nachgemessen im Browser (Chromium, Touch-Emulation), jeweils über alle Schritte bis zur
Bestätigung, für **Stellenauswahl → Konstrukteur**, **Deeplink Konstrukteur** und
**Deeplink Servicetechniker**:

| Viewport | scrollY über alle Schritte | Höhe Schritte | Weiter-Button | Karte + Topbar |
|----------|----------------------------|---------------|---------------|----------------|
| 360 × 640 | konstant | 496 px | fix | 637 px |
| 375 × 667 | konstant | 496 px | fix | 637 px |
| 390 × 844 | konstant | 496 px | fix | 637 px |
| 414 × 896 | konstant | 496 px | fix | 637 px |

Jeder Schritt passt damit auf jedem Standard-Handy ohne Scrollen ins Bild.
Auch beim Absenden bis zur Bestätigung springt nichts.

## Lead Table

**Noch kein Webhook hinterlegt** (Beispielseite). Solange `WEBHOOK_URL` und
`JOBS[].webhook` leer sind, verlässt keine Bewerbung die Seite. Sie wird nur in der
Browser-Konsole ausgegeben.

Anbinden: Generic-Webhook der Kachel in `index.html` eintragen. Entweder je Stelle
unter `webhook` (eigene Kachel pro Stelle) oder einmal in `WEBHOOK_URL` (gemeinsame Kachel).

Gesendet wird **nur** bei vollständiger, qualifizierter Bewerbung. Payload: **jedes Feld
genau einmal**, Vor- und Nachname getrennt, **kein** kombiniertes
`name`/`fullname`/`vollstaendiger_name`. Feldnamen nach der Konvention der bestehenden
Anbindungen (ALWA, Schanbacher): `vorname`, `nachname`, `telefon`, `email` als
Standardfelder, alles Weitere als Zusatzfelder.

```json
{
  "vorname": "Max",
  "nachname": "Mustermann",
  "telefon": "0151 23456789",
  "email": "max.mustermann@beispiel.de",
  "stelle": "Konstrukteur (m/w/d)",
  "qualifikation": "Studium Maschinenbau o. Ä. (Bachelor, Master oder Diplom)",
  "konstruktionserfahrung": "2 bis 5 Jahre",
  "cad_kenntnisse": "Sicher – ich konstruiere täglich in 3D-CAD",
  "erfahrung_vorrichtungsbau": "Nein, bisher in einem anderen Bereich",
  "englischkenntnisse": "Gut – ich komme im Beruf sicher zurecht (B2)",
  "nicht_erfuellt": "Erfahrung Vorrichtungs-/Sondermaschinenbau",
  "datum": "12.10.2026",
  "datenschutz": "Ja (Einwilligung mit Absenden, Art. 6 Abs. 1 lit. a DSGVO)",
  "quelle": "Karriere-Landingpage HK-Präzisionstechnik",
  "seite": "https://…/?stelle=konstrukteur"
}
```

Servicetechniker sendet statt der Konstrukteur-Fragen `qualifikation`,
`reisebereitschaft`, `fuehrerschein`, `erfahrung_service`, `englischkenntnisse`.

**Testeintrag (12.10.2026):** Ohne echte Kachel wurde der Webhook im Browser
abgefangen. Ergebnis je Stelle: genau 1 Request; „Max“, „Mustermann“, Telefon und
E-Mail jeweils genau einmal im richtigen Feld; kein Sammelfeld.
K.-o.-Abbrüche: 0 Requests. **Sobald die Kachel steht, den Test einmal mit echtem
Webhook wiederholen** und dabei die erwarteten Feldnamen der Kachel prüfen.

## Bildmaterial

Dateien mit genau diesem Namen in `bilder/` legen, dann ersetzt die Seite automatisch Verlauf bzw. Logo:

| Datei | verwendet für |
|-------|---------------|
| `hk-logo.svg` / `hk-logo.png` | Original-Logo: Topbar, Hero, Footer (bis dahin: `hk-logo-aus-signatur.png`) |
| `hero.jpg` | Hero allgemein |
| `hero-konstrukteur.jpg` | Hero bei `?stelle=konstrukteur` |
| `hero-servicetechniker.jpg` | Hero bei `?stelle=servicetechniker` |

Querformat, mind. ~1600 px breit, Motiv rechts (links liegt die Textfläche).
Weitere Fotos für die Creatives (Schritt 2) einfach dazulegen.

## Qualität

- Lighthouse mobil: Performance 97, Barrierefreiheit 100. SEO steht bei 63, weil die
  Beispielseite absichtlich auf `noindex` steht.
- axe-core (WCAG 2.2 AA + Best Practices): 0 Verstöße, Handy und Desktop
- Kein horizontaler Überlauf von 360 bis 1440 px

## Livegang

1. Inhalte mit HK abstimmen: Aufgaben, Voraussetzungen, Benefits (`JOBS`)
2. Beispiel-Leiste im `<header>` löschen und `--demo-h` auf `0px` setzen
3. `var BEISPIEL = false;` (aktiviert die JobPosting-Daten für Google)
4. `<meta name="robots">` auf `index, follow`
5. Webhook(s) eintragen und Testeintrag wiederholen

## Noch zu prüfen

- Kontakt-E-Mail: aktuell `stefan.kruck@hk-prt.de` aus der Signatur. Eine Bewerbungsadresse wäre besser.
- Links zu Impressum und Datenschutz (`hk-prt.de/impressum`, `/datenschutz`) sind angenommen
- Analytics-Snippet (`analytics.laendle-digital.com`, `data-website-id="hk_praezisionstechnik"`):
  ID anlegen oder Snippet entfernen
