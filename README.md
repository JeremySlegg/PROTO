Hier ist ein passendes README für deinen Node-RED Flow:

---

# Wetteranzeige mit LCD-Display (Node-RED)
Dieses Node-RED-Projekt ruft aktuelle Wetterdaten über die OpenWeatherMap API ab und zeigt sie auf einem externen LCD-Display an. Die Anzeige wechselt automatisch zwischen verschiedenen Wetterinformationen wie Temperatur, Luftfeuchtigkeit, Windgeschwindigkeit und Wetterbeschreibung.

## Funktionen
* **Automatischer Wetterabruf** alle 5 Minuten.
* **Fehlererkennung & Wiederherstellung** bei unterbrochener Datenaktualisierung (jede Stunde).
* **Anzeige auf LCD** via Python-Skript (`init.py`, `write.py`).
* **Wechselnde Anzeigemodi** je nach Konfiguration (Temperatur, Wind, etc.).
* **Datenverarbeitung & -formatierung** für kompakte LCD-Darstellung (max. 16 Zeichen).

## Ablauf
1. **Wetter abrufen**
   * Alle 5 Minuten wird ein HTTP-Request an die OpenWeatherMap API geschickt.
2. **Datenverarbeitung**
   * Temperatur, Luftfeuchtigkeit, Wind, Wetterbeschreibung, Zeitstempel etc. werden extrahiert und in der Flow-Variable gespeichert.
3. **Anzeige aktualisieren**
   * Je nach eingestelltem Modus wird der entsprechende Wert auf dem LCD angezeigt.
4. **Fehlerprüfung**
   * Ein separater Zeitgeber prüft stündlich, ob die Wetterdaten innerhalb der letzten 20 Minuten aktualisiert wurden. Wenn nicht, wird eine neue Abfrage erzwungen.
5. **Initialisierung**
   * Beim Start wird das Display zurückgesetzt und mit einem Begrüßungstext beschrieben.

## Voraussetzungen
* Node-RED installiert
* Python installiert mit Zugriffsrechten auf:
  * `init.py` – Initialisiert und löscht das Display
  * `write.py` – Schreibt Textzeilen auf das Display
* OpenWeatherMap API Key (`appid`) korrekt konfiguriert
* LCD-Hardware korrekt verbunden und über Python-Skripte ansprechbar

## Hinweis zur Sicherheit
Der API-Key ist aktuell **im Klartext** im HTTP-Knoten hinterlegt. Für produktive Umgebungen sollte dieser sicherer gespeichert werden, z. B. in Umgebungsvariablen oder Node-RED Secrets.

##Verzeichnisstruktur (Beispiel)
```bash
/home/jeremy/Documents/digilab/lcd/
├── init.py   # Initialisierung des LCD
└── write.py  # Schreiben von Text auf das LCD
```

## Anzeigemodi
Die Anzeige rotiert zwischen folgenden Modi (einzeln einstellbar im Flow):
* Temperatur (`Temperatur`)
* Luftfeuchtigkeit (`Luftfeuchtigkeit`)
* Windgeschwindigkeit (`Windgeschwindigk.`)
* Wetterbeschreibung (`Wetterlage`)
* Standort & Uhrzeit (`Standort`)

## Debugging
Im Flow sind mehrere Debug-Knoten eingebaut, um:
* API-Antworten zu prüfen
* Verarbeitete Wetterdaten anzuzeigen
* LCD-Schreibbefehle zu kontrollieren

## Hinweise
* Das LCD unterstützt 2 Zeilen mit je max. 16 Zeichen – längere Texte werden abgeschnitten.
* HTML-Sonderzeichen und Unicode-Zeichen werden entfernt oder ersetzt.
* Es wird nur ASCII-kompatibler Text angezeigt.
