# Wealth Pilot - Der Entnahmerechner 2.0

Eine Privacy-First Web-Anwendung zur Ruhestandsplanung, speziell für den deutschen Markt entwickelt.

## 🎯 Vision

Im Gegensatz zu simplen Zinsrechnern simuliert Wealth Pilot den gesamten Lebenszyklus der Vermögensplanung (Ansparen & Entspanren) unter Berücksichtigung von:

- **Realer Kaufkraft** (Inflationsbereinigung)
- **Deutschen Steuern** (Abgeltungssteuer, Sparerpauschbetrag)
- **Krankenversicherungsstatus** (KVdR, GKV, PKV)
- **Marktrisiken** (Monte-Carlo-Simulation mit 500+ Läufen)
- **Sequence of Returns Risk** (Reihenfolgerisiko der Renditen)

## 🎨 Features

### ✅ Vollständig implementiert

#### Section A: Ansparphase ("Hero")
- **Live-Berechnung**: Alle Eingaben aktualisieren sich in Echtzeit
- **Inflations-Toggle**: Umschalten zwischen nominalen und realen Werten (Kaufkraft heute)
- **Interaktiver Chart**: Visualisierung der Vermögensentwicklung bis zum Renteneintritt
- **Key Metrics**: Vermögen bei Rentenbeginn, Gesamteinzahlungen, Erzielte Gewinne

#### Section B: Entnahmestrategie ("Strategy Deck")
Drei wissenschaftlich fundierte Strategien mit jeweils eigener Monte-Carlo-Simulation:

1. **Ewige Rente** (Kapitalerhalt)
   - Nur Entnahme der Gewinne
   - Kapital bleibt vollständig erhalten
   - 🟢 Sicherheit: 100%
   - Ideal für: Erbe sichern, maximale Sicherheit

2. **Die 4%-Regel** (Standard)
   - Feste 4% Entnahme vom Startwert, inflationsangepasst
   - Wissenschaftlich erprobt (Trinity Study)
   - 🟡 Sicherheit: ~85-95% (abhängig von Asset Allocation)
   - Ideal für: Ausgewogenes Risiko-Rendite-Profil

3. **Kapitalverzehr** (Dynamisch)
   - Maximales Einkommen durch gezielten Kapitalabbau
   - Endet beim gewünschten Erbe (oder 0)
   - 🔴 Sicherheit: variabel
   - Ideal für: Maximales Einkommen, flexibel

#### Section C: Cockpit (Experten-Einstellungen)

**Markt & Inflation**
- Inflationsrate (Standard: 2%)
- Asset Allocation Slider: 0-100% Aktienquote
  - 0% = Sicher (Anleihen/Cash): 2% Volatilität
  - 100% = Chancen (Aktien): 18% Volatilität
  - Standard: 70/30 = 13,2% Volatilität

**Nachlass & Sicherheit**
- Gewünschtes Erbe (€)
- Lebenserwartung (Standard: 95 Jahre)

**Steuern & Sozialabgaben** (Der "German Factor")
- Abgeltungssteuer: 26,375% (25% + 5,5% Soli)
- Sparerpauschbetrag: 1.000 € (ab 2024)
- Teilfreistellung für Aktien-ETFs: 30% (konfigurierbar) - Steuerbefreiung auf Kursgewinne
- Krankenversicherungsstatus:
  - **KVdR** (Pflichtversichert): Keine Abzüge auf Kapitalerträge
  - **GKV** (Freiwillig): ~19% Abzug auf Gewinnanteil
  - **PKV** (Privat): Keine variablen Abzüge

**Monte Carlo**
- Anzahl Simulationen (Standard: 500)

**Rendite-Annahmen** (Neu!)
- Erwartete Rendite Sichere Anlagen: 2,5% p.a. (konfigurierbar)
- Erwartete Rendite Aktien: 7,5% p.a. (konfigurierbar)
- Erlaubt individuelle Anpassung der Renditeerwartungen basierend auf persönlichen Markteinschätzungen

#### Section D: Visualisierungen & Charts (Neu!)

**Entnahme-Verteilung (Kapitalverzehr-Strategie)**
- **Ersetzt** die irreführende Erfolgswahrscheinlichkeit für Kapitalverzehr
- Zeigt die **Bandbreite möglicher Entnahmen** über die Zeit
- Perzentilbänder:
  - 5. Perzentil: Worst-Case-Szenario (nur 5% der Simulationen schlechter)
  - 25.-75. Perzentil: Mittlere 50% der Ergebnisse (schattierte Fläche)
  - Median (50.): Erwartete typische Entnahme (hervorgehoben)
  - 95. Perzentil: Best-Case-Szenario (nur 5% der Simulationen besser)
- **Praktischer Nutzen**: "Ihr erwartetes Einkommen ist €2.500/Monat (Median), aber in einem schlechten Marktszenario (5. Perzentil) könnte es auf €1.200/Monat sinken"
- Hilft bei der Planung für Worst-Case-Szenarien

**Vermögensverlauf mit Unsicherheitsbändern (Alle Strategien)**
- Erweiterte Darstellung der Vermögensentwicklung während der Entnahmephase
- Zeigt **Perzentilbänder** für mögliche Vermögenstrajektorien:
  - 5. Perzentil: Worst-Case-Vermögen (gestrichelte rote Linie)
  - 25.-75. Perzentil: Mittlere 50% der Vermögensentwicklungen (schattierte Fläche)
  - Median: Typische Vermögensentwicklung (hervorgehoben, immer sichtbar)
  - 95. Perzentil: Best-Case-Vermögen (gestrichelte grüne Linie)
- **Toggle-Kontrolle**: "Bänder anzeigen" Checkbox zum Ein-/Ausblenden der Perzentile
- Visualisiert Unsicherheit und Marktrisiko

**Erfolgswahrscheinlichkeit über Zeit (Ewige Rente & 4%-Regel)**
- Zeigt den Prozentsatz der Simulationen, die das Vererbungsziel erreichen
- Bleibt erhalten für Strategien 1 & 2 (sinnvolle Metrik bei Kapitalerhalt)

## 🔬 Mathematische Grundlagen

### 1. Steuerstundung (Tax Deferral)
Anders als Bank-Rechner werden in der Ansparphase keine Steuern abgezogen (Annahme: Thesaurierender ETF). Der Zinseszins wirkt auf den Bruttobetrag.

```
VermögenNeu = VermögenAlt × (1 + BruttoRendite) + Sparrate
```

Erst bei der Entnahme wird der Gewinnanteil ermittelt und versteuert:
```
Gewinnanteil = (AktuellesVermögen - Einstandskurs) / AktuellesVermögen
```

### 2. Die "Freiwillig Versichert" Falle
Viele Rentner unterschätzen die Abzüge bei freiwilliger GKV-Versicherung:

```
NettoEntnahme = BruttoEntnahme - Steuer - (Gewinnanteil × 19%)
```

Die ~19% setzen sich zusammen aus:
- 14,6% KV-Beitrag
- ~1,6% Zusatzbeitrag (Durchschnitt)
- 3,05% Pflegeversicherung

### 3. Dynamische Volatilität (Asset Allocation)
Die Standardabweichung für Monte Carlo wird linear interpoliert:

```
Vola_Safe = 2% (Cash/Anleihen)
Vola_Risky = 18% (Aktien)

StdDev = Vola_Safe + (Slider_Percent × (Vola_Risky - Vola_Safe))
```

**Beispiel**: 70/30 Portfolio
```
StdDev = 0.02 + (0.70 × 0.16) = 0.132 → 13,2% Volatilität
```

### 4. Monte Carlo Simulation
Für jede Entnahmestrategie werden 500+ Simulationsläufe durchgeführt:

1. Jedes Jahr wird eine zufällige Rendite generiert (Normalverteilung)
2. Rendite = Mittelwert ± Volatilität (Box-Muller-Transform)
3. Entnahme wird berechnet (inkl. Steuern & KV)
4. Vermögen wird aktualisiert
5. Erfolgsrate = Anzahl erfolgreicher Läufe / Gesamtläufe

**Erfolg** = Vermögen am Ende ≥ Gewünschtes Erbe

### 5. Erbe-Berechnung
Für dynamische Entnahme (Kapitalverzehr) wird die Annuitätenformel angepasst:

```
PMT = (PV - FV × fvFactor) / pvFactor

wobei:
pvFactor = (1 + r)^n - 1 / (r × (1 + r)^n)
fvFactor = 1 / (1 + r)^n
```

Dies reduziert die Entnahme, damit das gewünschte Erbe übrig bleibt.

## 🔒 Privacy & Datenschutz

### Keine Cloud, keine Server
- **100% lokal**: Alle Berechnungen erfolgen in Ihrem Browser
- **Keine Datenübertragung**: Keine Verbindung zu Servern (außer CDN für Libraries)
- **localStorage**: Automatisches Speichern im Browser
- **Single File**: Komplette Anwendung in einer HTML-Datei

### Daten löschen
Browserdaten löschen entfernt alle gespeicherten Einstellungen:
- Chrome/Edge: `Strg + Shift + Entf` → "Cookies und andere Websitedaten"
- Firefox: `Strg + Shift + Entf` → "Cookies"
- Safari: Einstellungen → Datenschutz → Website-Daten verwalten

## 🚀 Installation & Nutzung

### Einfachste Methode
1. Datei `wealth-pilot.html` herunterladen
2. Doppelklick → Öffnet sich im Browser
3. Fertig!

### Empfohlene Browser
- ✅ Chrome/Edge (beste Performance)
- ✅ Firefox
- ✅ Safari
- ⚠️ Internet Explorer nicht unterstützt

### Offline-Nutzung
Die Anwendung kann komplett offline genutzt werden, wenn die CDN-Bibliotheken einmal geladen wurden. Für vollständige Offline-Nutzung können die Bibliotheken lokal eingebunden werden.

## 📱 Mobile Optimierung

Vollständig responsiv und touch-optimiert:
- Große Touch-Targets für alle Buttons und Slider
- Stacking Layout auf kleinen Bildschirmen
- Optimierte Charts für mobile Ansicht

## 📊 Verwendete Technologien

- **Tailwind CSS** (via CDN): Modernes, utility-first CSS Framework
- **Chart.js v4**: Interaktive, responsive Charts
- **jsPDF**: PDF-Export-Funktionalität
- **Vanilla JavaScript**: Keine weiteren Abhängigkeiten

## 🎓 Wissenschaftliche Grundlagen

### Trinity Study (4%-Regel)
Die 4%-Regel basiert auf der "Trinity Study" von 1998:
- Analyse historischer Marktdaten von 1926-1995
- 30 Jahre Entnahmezeitraum
- 95% Erfolgsrate bei 50/50 Portfolio

**Quellen**:
- Bengen, William P. (1994): "Determining Withdrawal Rates Using Historical Data"
- Trinity Study: "Retirement Savings: Choosing a Withdrawal Rate That Is Sustainable"

### Sequence of Returns Risk
Das Reihenfolgerisiko beschreibt die Gefahr schlechter Renditen zu Beginn der Entnahmephase:
- Frühe Verluste sind gefährlicher als späte
- Monte Carlo simuliert verschiedene Renditereihenfolgen
- Zeigt realistische Worst-Case-Szenarien

### Modern Portfolio Theory (MPT)
Asset Allocation basiert auf Markowitz's Portfolio-Theorie:
- Risiko-Rendite-Optimierung
- Diversifikation senkt Risiko ohne Renditeverlust
- Volatilität als Risikomaß

## 📈 Beispiel-Szenarien

### Szenario 1: Der konservative Sparer
**Eingaben**:
- Alter: 30 → 67
- Vermögen: 50.000 €
- Sparrate: 800 €/Monat
- Rendite: 5% (konservativ)
- Asset Allocation: 40/60 (Aktien/Sicher)

**Ergebnis**:
- Vermögen mit 67: ~550.000 €
- Strategie "4%-Regel": ~1.800 €/Monat
- Sicherheit: 94%

### Szenario 2: Der FIRE-Anhänger
**Eingaben**:
- Alter: 25 → 45 (Early Retirement)
- Vermögen: 30.000 €
- Sparrate: 2.500 €/Monat
- Rendite: 7% (MSCI World Durchschnitt)
- Asset Allocation: 90/10 (Aggressive)

**Ergebnis**:
- Vermögen mit 45: ~950.000 €
- Strategie "4%-Regel": ~3.100 €/Monat
- Sicherheit: 89% (höheres Risiko durch längeren Entnahmezeitraum)

### Szenario 3: Der Erbe-Planer
**Eingaben**:
- Alter: 50 → 67
- Vermögen: 300.000 €
- Sparrate: 1.500 €/Monat
- Rendite: 6%
- Gewünschtes Erbe: 200.000 €
- Asset Allocation: 60/40

**Ergebnis**:
- Vermögen mit 67: ~770.000 €
- Strategie "Kapitalverzehr": ~2.800 €/Monat
- Vermögen mit 95: ~200.000 € (Erbe gesichert)

## ⚠️ Wichtige Hinweise & Disclaimer

### Keine Finanzberatung
Diese Anwendung dient ausschließlich zu Informations- und Bildungszwecken. Sie stellt keine Finanzberatung dar.

### Vereinfachungen
Die folgenden Vereinfachungen wurden getroffen:
1. **Vorabpauschale**: Nicht explizit modelliert (bei thesaurierenden ETFs relevant)
2. **Kirchensteuer**: Nicht berücksichtigt (8-9% auf Abgeltungssteuer)
3. **GKV-Beiträge**: Vereinfacht als 19% pauschal
4. **Renditeverteilung**: Normalverteilung (in Realität sind Märkte nicht perfekt normalverteilt)

### Rendite-Annahmen
Historische Durchschnittswerte (vor Inflation):
- MSCI World: ~7-8% p.a. (seit 1975)
- 60/40 Portfolio: ~6-7% p.a.
- Anleihen: ~3-5% p.a.

**Wichtig**: Vergangene Performance ist keine Garantie für zukünftige Ergebnisse.

### Inflation
Standard-Inflationsannahme: 2% p.a. (EZB-Ziel)
Historisch schwankte die Inflation in Deutschland zwischen -0,5% und 4% (letzte 30 Jahre).

## 🐛 Bekannte Limitierungen

1. **Browser-Kompatibilität**: Funktioniert nicht in Internet Explorer
2. **Performance**: Bei >1000 Monte Carlo Läufen kann die Berechnung verzögert sein
3. **Mobile**: Charts können auf sehr kleinen Bildschirmen schwer lesbar sein
4. **PDF-Export**: Beinhaltet keine Charts (nur Zahlen)

## 🔄 Zukünftige Erweiterungen (Optional)

Mögliche Features für zukünftige Versionen:
- [ ] Chart-Export als Bild
- [ ] Detaillierte Monte-Carlo-Visualisierung (Perzentile)
- [ ] Vorabpauschalen-Berechnung
- [ ] Mehrere Szenarien vergleichen
- [ ] Import/Export von Einstellungen (JSON)
- [ ] Dark Mode
- [ ] Erweiterte Steuersimulation (Kirchensteuer)
- [ ] Renten-Integration (gesetzliche/private Rente)
- [ ] Mehrere Währungen

## 📝 Changelog

### Version 1.2 - Januar 2026
**Neue Features:**
- ✅ **Entnahme-Verteilungs-Chart**: Neue Visualisierung für Kapitalverzehr-Strategie zeigt Bandbreite möglicher monatlicher Entnahmen
  - Ersetzt die irreführende Erfolgswahrscheinlichkeit für diese Strategie
  - Zeigt 5., 25., 50. (Median), 75. und 95. Perzentil der Entnahmen über Zeit
  - Gibt praktische Einblicke: "Erwartete €2.500/Monat, aber Worst-Case €1.200/Monat"
- ✅ **Vermögens-Perzentilbänder**: Alle Strategien zeigen nun Unsicherheitsbänder im Vermögensverlauf-Chart
  - Visualisiert die Bandbreite möglicher Vermögensentwicklungen (5., 25., 50., 75., 95. Perzentil)
  - Toggle-Kontrolle zum Ein-/Ausblenden der Perzentilbänder
  - Median-Linie immer sichtbar und hervorgehoben
- ✅ **Strategie-spezifische Charts**: Kapitalverzehr zeigt Entnahme-Verteilung, andere Strategien zeigen Erfolgswahrscheinlichkeit
- ✅ **Verbesserte Monte-Carlo-Datenstruktur**: Tracking von Entnahmen und Vermögen für alle Simulationsläufe

### Version 1.1 - Januar 2026
**Neue Features:**
- ✅ **Teilfreistellung implementiert**: 30% Steuerbefreiung für Aktien-ETFs (konfigurierbar)
- ✅ **Konfigurierbare Rendite-Annahmen**: Erwartete Renditen für sichere und risikoreiche Anlagen können individuell angepasst werden
- ✅ **Ewige Rente Chart-Fix**: Success Rate Chart funktioniert jetzt auch für die "Ewige Rente" Strategie
- ✅ **Performance-Optimierung**: Caching von Monte-Carlo-Simulationen im Binary Search reduziert Berechnungszeit

## 📚 Weitere Ressourcen

### Empfohlene Lektüre
- **"Souverän investieren mit Indexfonds und ETFs"** - Gerd Kommer
- **"The Simple Path to Wealth"** - JL Collins
- **"Your Money or Your Life"** - Vicki Robin

### Community & Austausch
- **r/Finanzen** (Reddit): Deutsche FIRE-Community
- **Finanzfluss** (YouTube): ETF & Altersvorsorge
- **Finanztip**: Verbrauchertipps zu Geldanlage

### Tools & Calculators
- **Portfolio Performance**: Kostenlose Portfolio-Software
- **cFIREsim**: Historische FIRE-Simulationen (USA-fokussiert)
- **FireCalc**: Retirement Calculator mit historischen Daten

## 📄 Lizenz

MIT License - Frei verwendbar für private und kommerzielle Zwecke.

## 🤝 Beitragen

Verbesserungsvorschläge und Bug-Reports sind willkommen!

### Häufige Verbesserungen
1. Genauere Steuermodelle (z.B. Vorabpauschale)
2. Zusätzliche Entnahmestrategien (z.B. Guyton-Klinger)
3. Verbesserte Mobile-UX
4. Mehrsprachigkeit (EN, FR, ES)

---

**Entwickelt mit 💙 für die deutsche FIRE- und ETF-Community**

*Version 1.2 - Januar 2026*
