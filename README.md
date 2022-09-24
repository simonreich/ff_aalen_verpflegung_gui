# Anforderungen

## Hardware

Als Hardware dienen:

- Raspberry Pi v3 oder v4
  - Stromversorgung
  - SD-Karte 16GB
- Externer Bildschirm mit Touchscreen
  - Stromversorgung
  - HDMI-Kabel
  - Anschluss für Touchscreen an Raspberry Pi
- Lesegerät für RFID-Chips

## Graphische Oberfläche

Nach dem Boot vom Raspberry Pi und dem Start von Raspbian wird die GUI automatische geöffnet und in den Vordergrund geschoben.
Die GUI wird in Python programmiert.
Sie besteht aus 3 Dialogen:

1. Main-Dialog
2. Auswahl-Dialog
3. Hilfe-Dialog.


### Main-Dialog

Hier sind alle zu kaufenden Items auf einem Grid aufgeführt.
Für jedes Item existiert

- Ein Symbolbild
- Text und Preis unter dem Symbolbild

Bei Klick auf einem Item öffnet sich der Auswahl-Dialog.

Wird das Symbolbild auf der SD-Karte nicht gefunden, wird ein Default-Bild angezeigt, z.B. ein Flaschensymbol.

In der linken oberen Ecke existiert ein Hilfe-Symbol.
Bei Klick öffnet sich der Hilfe-Dialog.

Nach 10 Minuten Inaktivität wird der externe Monitor abgeschaltet.

### Auswahl-Dialog

Der Auswahl-Dialog legt sich über den Main-Dialog. 
Der Main-Dialog wird im Hintergrund ausgegraut.

Er zeigt das gewählte Item in vergrößerter Form an und dient zur visuellen Bestätigung.
Unter dem Item ist eine Graphik, die darum bittet, den roten Feuerwehr-Dongle auf das RFID-Lesegerät zu halten.
Unter der Graphik ist ein "Abbrechen"-Button.

Wird der Feuerwehr-Dongle auf das Lesegerät gehalten, gilt das als Bestätigung.
Das gewählte Item wird in der Datenbank gespeichert.
Anschließend schließt sich der Auswahl-Dialog und kehrt zum Main-Dialog zurück.

Bei Klick auf Abbrechen schließt sich der Auswahl-Dialog und kehrt zum Main-Dialog zurück.
Bei 30 Sekunden Inaktivität schließt sich der Auswahl-Dialog und kehrt zum Main-Dialog zurück.

### Hilfe-Dialog

Listet:

- Ansprechpartner mit Mailadresse
- Softwareversion
- Datum vom letzten Softwareupdate
- Neustart-Button (Startet Raspberry Pi neu)

## Softwareupdates

Es wird unterschieden von Source-Code-Updates und Konfigurations-Updates:

### Source-Code-Updates

Die Software steht in git-Repository zur Verfügung.
Periodisch, z.B. jede Nacht um 2:00 Uhr, prüft die Software, ob eine neue Version veröffentlicht ist.
Falls das der Fall ist, wird sie automatisch installiert und die Software neu gestartet.

## Softwarekonfiguration

Alle vom Fachbereich vorzunehmenden Konfigurationen werden zentral in einer Datei abgelegt, z.B. `config-items.json`:

```json
{
  "ansprechpartner": 
    {
      "name": "Max Mustermann",
      "email": "mmustermann@example.com"
    },
  "items": [
    {
      "name": "Cola",
      "logo": "cola.jpg",
      "preis": 1.00
    },
    {
      "name": "Fanta",
      "logo": "fanta.jpg",
      "preis": 1.00
    },
    {
      "name": "Wasser",
      "logo": "wasser.jpg",
      "preis": 1.00
    },
    {
      "name": "Apfelschorle",
      "logo": "apfelschorle.png",
      "preis": 1.00
    }
  ]
}
```

Im selben Verzeichnis werden die Symbolbilder als jpg- oder png-Datei abgelegt.

TODO: Wie kommen die Softwarekonfigurationen auf den Raspberry Pi?

- Ebenfalls über ein git-Repository
  - Automatisiert, z.B. jede Nacht
- Über einen USB-Stick
  - Updatedialog, der aus Hilfe-Dialog geöffnet wird?
- Per Mail von Account Ansprechpartner.
  - Automatisiert, z.B. jede Nacht
  - GUI benötigt eigenen Mail-Account.
  - Passwort muss im Plain-Text hinterlegt werden.

## Datenfluss

1. Nutzer wählt Item (IID, IID ist Name vom Getränk) 
2. Nutzer identifiziert sich über RFID-Leser (UID, UID ist gelesener RFID-Code) 
3. UID, IID, Datum, Zeit, Preis wird in Datenbank gespeichert

## Datenspeicher

Jede Transaktion (UID, IID, Datum, Zeit, Preis) wird lokal in einer SQlite3-Datenbank abgelegt.

## Datenexport

TODO: 
- Welches Ereignis löst den Datenexport aus? Ist er periodisch, z.B. jeden Monatsanfang?
- Wohin sollen die Datenexportiert werden?
- In welchem Format?
- Sollen die lokalen Daten nach dem Export gelöscht werden? Wie lange werden Daten aufgehoben?

