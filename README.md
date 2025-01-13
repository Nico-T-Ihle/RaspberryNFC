# Raspberry-NFC-Documentation

Hier ist eine Anleitung, wie du einen RFID-Reader an einen Raspberry Pi anschließen kannst und ein einfaches Python-Skript zum Lesen und Schreiben auf RFID-Tags erstellst. In diesem Fall verwenden wir den **RC522 RFID-Reader**, der oft mit einem Raspberry Pi verwendet wird.

### Voraussetzungen:
- Raspberry Pi (mit installiertem Raspberry Pi OS)
- RC522 RFID-Reader-Modul (z.B. MFRC522)
- Jumper-Kabel
- RFID-Tags oder -Karten

### 1. Verkabelung

Zuerst musst du den RFID-Reader mit deinem Raspberry Pi verbinden. Hier ist die Belegung:

| RFID-Pin  | Raspberry Pi Pin  |
|-----------|-------------------|
| VCC       | Pin 1 (3.3V)      |
| GND       | Pin 6 (Ground)    |
| MISO      | Pin 21 (GPIO 9)   |
| MOSI      | Pin 19 (GPIO 10)  |
| SCK       | Pin 23 (GPIO 11)  |
| SDA       | Pin 24 (GPIO 8, CE0) |
| RST       | Pin 22 (GPIO 25)  |

**Hinweis:** Verwende den **3.3V-Pin** für die Stromversorgung des RC522, da der Reader für 3.3V ausgelegt ist und 5V beschädigen kann.

### 2. SPI-Schnittstelle aktivieren

Bevor du den RFID-Reader mit Python programmieren kannst, musst du die SPI-Schnittstelle auf dem Raspberry Pi aktivieren:

1. Öffne ein Terminal auf deinem Raspberry Pi.
2. Gib folgenden Befehl ein, um die Raspberry Pi-Konfiguration zu öffnen:
   ```bash
   sudo raspi-config
   ```
3. Gehe zu **Interfacing Options** und aktiviere **SPI**.
4. Starte den Raspberry Pi neu.

### 3. Benötigte Python-Bibliotheken installieren

Installiere die notwendigen Bibliotheken:

```bash
sudo apt-get update
sudo apt-get install python3-dev python3-pip
sudo pip3 install spidev
sudo pip3 install mfrc522
```

### 4. Einfaches Python-Skript zum Lesen eines RFID-Tags

Erstelle ein neues Python-Skript, um einen RFID-Tag zu lesen.

```bash
nano rfid_read.py
```

Füge folgenden Code ein:

```python
#!/usr/bin/env python

import RPi.GPIO as GPIO
from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()

try:
    print("Bitte halte deine RFID-Karte an den Leser...")
    id, text = reader.read()
    print(f"ID: {id}")
    print(f"Text: {text}")
finally:
    GPIO.cleanup()
```

Speichere das Skript und führe es aus:

```bash
python3 rfid_read.py
```

Wenn du deine RFID-Karte oder deinen Tag an den Leser hältst, sollte der Code und der Text auf dem Tag angezeigt werden.

### 5. Einfaches Python-Skript zum Schreiben auf einen RFID-Tag

Erstelle ein neues Python-Skript, um auf einen RFID-Tag zu schreiben:

```bash
nano rfid_write.py
```

Füge folgenden Code ein:

```python
#!/usr/bin/env python

import RPi.GPIO as GPIO
from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()

try:
    text = input("Gib den Text ein, den du auf die Karte schreiben möchtest: ")
    print("Bitte halte deine RFID-Karte an den Leser...")
    reader.write(text)
    print("Text geschrieben.")
finally:
    GPIO.cleanup()
```

Speichere das Skript und führe es aus:

```bash
python3 rfid_write.py
```

Gib den Text ein, der auf die Karte geschrieben werden soll, und halte dann den RFID-Tag an den Leser. Der Text wird auf den Tag geschrieben.

### 6. Testen

1. Führe das **Lesen-Skript** (`rfid_read.py`) aus, um den Inhalt des Tags zu überprüfen.
2. Führe das **Schreiben-Skript** (`rfid_write.py`) aus, um Text auf das Tag zu schreiben.
3. Halte den Tag an den Leser, um die Aktionen zu testen.

### 7. Fehlerbehebung

Falls der Reader nicht funktioniert, überprüfe:
- Ob die SPI-Schnittstelle korrekt aktiviert ist.
- Ob die Verkabelung korrekt und fest verbunden ist.
- Ob du den RFID-Tag richtig an den Leser hältst.

Damit solltest du in der Lage sein, einen RC522 RFID-Reader an einen Raspberry Pi anzuschließen und einfache Lese- und Schreiboperationen durchzuführen!
