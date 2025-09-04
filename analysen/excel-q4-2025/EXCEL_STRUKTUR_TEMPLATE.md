# 📝 EXCEL-STRUKTUR TEMPLATE - BLITZSCHUTZ ABRECHNUNGEN

## 📊 DATEISTRUKTUR

### Dateiname-Konvention:
```
Termine_Q[Quartal]_Abrechnung[Monat]_[Jahr].xlsx
Beispiel: Termine_Q4_Abrechnung09_2025.xlsx
```

---

## 📑 ARBEITSBLÄTTER

### Blatt 1: HAUPTDATEN
**Spalten-Struktur:**

| Spalte | Feldname | Datentyp | Beispiel | Pflichtfeld |
|--------|----------|----------|----------|-------------|
| A | Projektnummer | Text | K001461 | ✅ |
| B | Kunde | Text | Betty Barclay Group | ✅ |
| C | Objektbezeichnung | Text | Hochregallager | ✅ |
| D | Straße | Text | Im Bruch 6 | ✅ |
| E | PLZ | Zahl | 76646 | ✅ |
| F | Ort | Text | Bruchsal | ✅ |
| G | Ansprechpartner | Text | Herr Römmer | ⭕ |
| H | Telefon | Text | 06224 900-538 | ⭕ |
| I | E-Mail | E-Mail | christian.roemmer@bettybarclay.com | ⭕ |
| J | Prüfungsart | Dropdown | Umfassende Prüfung | ✅ |
| K | Prüfungsstatus | Dropdown | Erstprüfung | ✅ |
| L | Anzahl Mess-Stellen | Zahl | 25 | ✅ |
| M | Prüfdatum | Datum | 15.09.2025 | ✅ |
| N | Prüfer | Dropdown | Armin | ✅ |
| O | Bemerkungen | Text | Leistungsnachweis unterschreiben lassen | ⭕ |

### Blatt 2: ABRECHNUNG
**Spalten-Struktur:**

| Spalte | Feldname | Datentyp | Beispiel | Berechnung |
|--------|----------|----------|----------|------------|
| A | Projektnummer | Text | K001461 | =Hauptdaten!A2 |
| B | Prüfgebühr netto | Währung | 450,00 € | Basisbetrag |
| C | Je Nebenerdung | Währung | 25,00 € | Anzahl × Preis |
| D | Je EW Messung | Währung | 15,00 € | Anzahl × Preis |
| E | Je ÜSE | Währung | 35,00 € | Anzahl × Preis |
| F | Je PAS | Währung | 20,00 € | Anzahl × Preis |
| G | Erstellung Bericht | Währung | 150,00 € | Festbetrag |
| H | Fahrtkosten | Währung | 85,00 € | km × 0,35 € |
| I | 6% KfZ/Messgeräte | Währung | 48,90 € | =(B:H)*0,06 |
| J | Zwischensumme | Währung | 863,90 € | =SUMME(B:I) |
| K | MwSt (19%) | Währung | 164,14 € | =J*0,19 |
| L | Gesamtsumme | Währung | 1028,04 € | =J+K |

### Blatt 3: ROUTENPLANUNG
**Spalten-Struktur:**

| Spalte | Feldname | Datentyp | Beispiel | Hinweis |
|--------|----------|----------|----------|---------|
| A | Tour-ID | Text | Q4.1 | Quartal.Woche |
| B | Projektnummer | Text | K001461 | Referenz |
| C | Reihenfolge | Zahl | 1 | Sortierung |
| D | Breitengrad | Dezimal | 49.0865 | GPS-Koordinate |
| E | Längengrad | Dezimal | 8.4419 | GPS-Koordinate |
| F | Geplante Ankunft | Zeit | 08:30 | HH:MM |
| G | Verweildauer (Min) | Zahl | 120 | Minuten |
| H | Fahrzeit zum nächsten | Zahl | 25 | Minuten |
| I | Status | Dropdown | Geplant | Geplant/Erledigt/Verschoben |

---

## 🔧 FORMELN UND VALIDIERUNGEN

### Dropdown-Listen:

**Prüfungsart:**
- Umfassende Prüfung
- Vollständige Prüfung
- Sichtprüfung
- Wartung

**Prüfungsstatus:**
- Erstprüfung
- Wiederholungsprüfung
- Nachprüfung
- Sonderprüfung

**Prüfer:**
- Armin
- Ruben
- Seibert Wolfgang

### Bedingte Formatierung:

1. **Kritische Felder (ROT):**
   - Bemerkungen enthalten "Leistungsnachweis"
   - Prüfdatum < HEUTE()

2. **Warnung (GELB):**
   - Mess-Stellen > 50
   - Fahrtkosten > 100€

3. **Erledigt (GRÜN):**
   - Status = "Erledigt"

---

## 📈 PIVOT-TABELLEN

### Übersicht 1: Prüfungen pro Prüfer
```
Zeilen: Prüfer
Werte: Anzahl Prüfungen, Summe Gesamtsumme
```

### Übersicht 2: Umsatz pro Kunde
```
Zeilen: Kunde
Werte: Anzahl Aufträge, Summe Gesamtsumme
```

### Übersicht 3: Prüfungen pro PLZ-Bereich
```
Zeilen: PLZ (gruppiert)
Werte: Anzahl, Durchschnitt Fahrtkosten
```

---

## 🔐 DATENVALIDIERUNG

### Pflichtfelder prüfen:
```Excel VBA
=WENN(ODER(A2="";B2="";C2="");"FEHLER: Pflichtfeld leer";"OK")
```

### Projektnummer-Format:
```Excel VBA
=WENN(LINKS(A2;1)="K";"OK";"FEHLER: K-Nummer erwartet")
```

### E-Mail-Validierung:
```Excel VBA
=WENN(ISTFEHLER(FINDEN("@";I2));"Keine E-Mail";"OK")
```

---

## 💾 IMPORT/EXPORT

### CSV-Export Format:
```
Projektnummer;Kunde;Objekt;PLZ;Ort;Prüfdatum;Gesamtsumme
K001461;Betty Barclay;Hochregallager;76646;Bruchsal;15.09.2025;1028.04
```

### JSON-Struktur für API:
```json
{
  "projektnummer": "K001461",
  "kunde": {
    "name": "Betty Barclay Group",
    "objekt": "Hochregallager",
    "adresse": {
      "strasse": "Im Bruch 6",
      "plz": "76646",
      "ort": "Bruchsal"
    }
  },
  "pruefung": {
    "art": "Umfassende Prüfung",
    "status": "Erstprüfung",
    "messstellen": 25,
    "datum": "2025-09-15"
  },
  "abrechnung": {
    "netto": 863.90,
    "mwst": 164.14,
    "brutto": 1028.04
  }
}
```

---

## 🚀 AUTOMATISIERUNGS-READINESS

### Bereit für Automatisierung:
- ✅ Strukturierte Projektnummern
- ✅ Standardisierte Dropdown-Felder
- ✅ GPS-Koordinaten für Routing
- ✅ Klare Preis-Kalkulationen

### Verbesserungspotenzial:
- ⚠️ Einheitliche Datumsformate
- ⚠️ Vollständige E-Mail-Adressen
- ⚠️ Standardisierte Zeitangaben
- ⚠️ Konsistente Statusverfolgung

---

*Template erstellt für Blitzschutz Adams Workflow Projekt*
*Version 1.0 - September 2025*