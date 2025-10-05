# Programmierprojekt: LSB-Steganographie in C++

Dieses Projekt ist eine ideale, praktische Vorbereitung auf die Inhalte der Vorlesung "Multimedia Security". Es berührt alle geforderten Kernkompetenzen: C++-Programmierung, grundlegende Bildverarbeitung und algorithmisches Denken.

---

## Die Idee: Versteckte Nachrichten in Bildern 🖼️

Steganographie ist die Kunst, eine Nachricht in einem Trägermedium (in unserem Fall ein Bild) so zu verstecken, dass ihre Existenz für Dritte nicht offensichtlich ist.

Wir verwenden die einfachste und bekannteste Methode: **Least Significant Bit (LSB) Steganography**. Die Idee ist simpel: Jedes Pixel eines Bildes besteht aus Farbwerten, z.B. für Rot, Grün und Blau (RGB). Jeder dieser Farbwerte ist ein Byte (8 Bits). Wir verändern nur das **allerletzte, unwichtigste Bit** (das LSB) jedes Farbwertes und ersetzen es durch ein Bit unserer geheimen Nachricht. Diese Änderung ist für das menschliche Auge praktisch unsichtbar, erlaubt uns aber, Daten im Bild zu "schmuggeln".



---

## Warum dieses Projekt ideal zur Vorbereitung ist

* **Direkter Bezug:** Steganographie ist ein Kernthema der Vorlesung.
* **C++ Übung:** Du trainierst den Umgang mit Klassen, Datei-I/O (File-I/O) und Bit-Operationen – allesamt wichtige Programmiergrundlagen.
* **Bildverarbeitung:** Du lernst, Pixel für Pixel durch ein Bild zu iterieren und deren Daten auf unterster Ebene zu manipulieren.
* **Machbarkeit:** Das Projekt ist so konzipiert, dass es in einer Woche gut zu bewältigen ist.

---

## Projektplan für eine Woche 🚀

### Tag 1: Setup und Grundlagen

Das Ziel des ersten Tages ist es, deine Arbeitsumgebung einzurichten und eine grundlegende Bildoperation durchzuführen.

1.  **Entwicklungsumgebung einrichten:** Installiere und konfiguriere deine C++-Umgebung (z.B. Visual Studio Code mit g++ Compiler, CLion oder Visual Studio).
2.  **Bildbibliothek integrieren:** Lade die Header-Only-Bibliotheken **`stb_image.h`** und **`stb_image_write.h`** herunter. Diese sind extrem einfach zu nutzen, da du sie nur in dein Projekt einbinden musst, ohne komplexe Kompilierungs- oder Linker-Schritte.
3.  **Erstes Programm schreiben:** Implementiere ein einfaches C++-Programm, das:
    * Eine Bilddatei (z.B. `input.png`) von der Festplatte lädt.
    * Die Bilddimensionen (Breite und Höhe) auf der Konsole ausgibt.
    * Das geladene Bild unverändert unter einem neuen Namen (z.B. `copy.png`) wieder abspeichert.

### Tag 2-3: Implementierung des Encoders

Jetzt implementieren wir die Kernlogik zum Verstecken der Nachricht.

* **Funktion erstellen:** Schreibe eine Funktion mit der Signatur `void encode(const char* imagePath, const std::string& message, const char* outputPath);`.
* **Ablauf in der Funktion:**
    1.  Lade das Bild unter `imagePath`.
    2.  **Wichtig:** Kodiere zuerst die Länge der Nachricht in die ersten Bits des Bildes. So weiß der Decoder später, wie viele Bits er lesen muss.
    3.  Iteriere durch jedes Bit deiner `message`.
    4.  Für jedes Nachrichten-Bit, nimm den nächsten verfügbaren Farbkanal (R, G, B, R, G, B, ...) des Bildes.
    5.  Setze das LSB dieses Farbwerts zuerst auf 0 mittels einer bitweisen UND-Verknüpfung: `color_byte = color_byte & 0xFE;` (0xFE ist binär `11111110`).
    6.  Setze das LSB dann auf den Wert deines Nachrichten-Bits (0 oder 1) mittels einer bitweisen ODER-Verknüpfung: `color_byte = color_byte | message_bit;`.
    7.  Stelle sicher, dass die Nachricht nicht länger ist als die Kapazität des Bildes (Breite × Höhe × 3 Bits).
    8.  Speichere das modifizierte Bild unter `outputPath`.

### Tag 4-5: Implementierung des Decoders

Nun schreiben wir die Logik, um die versteckte Nachricht wieder auszulesen.

* **Funktion erstellen:** Schreibe eine Funktion mit der Signatur `std::string decode(const char* imagePath);`.
* **Ablauf in der Funktion:**
    1.  Lade das steganographisch veränderte Bild (`imagePath`).
    2.  Lies zuerst die Länge der versteckten Nachricht aus den ersten Bits des Bildes aus.
    3.  Iteriere nun so oft, wie die ausgelesene Länge es vorgibt, durch die Farbkanäle der Pixel.
    4.  Extrahiere bei jedem Farbkanal nur das LSB durch eine bitweise UND-Verknüpfung: `lsb = color_byte & 1;`.
    5.  Sammle diese einzelnen Bits und setze sie nach und nach wieder zu Bytes (und damit zu Zeichen) zusammen.
    6.  Gib die vollständig rekonstruierte Nachricht als `std::string` zurück.

### Tag 6-7: Testen, Aufräumen & Kommandozeile

Im letzten Schritt machen wir das Programm benutzerfreundlich und robust.

1.  **`main`-Funktion erstellen:** Implementiere eine `main`-Funktion, die Kommandozeilenargumente verarbeitet, um entweder den Encoder oder den Decoder aufzurufen.
2.  **Beispielaufrufe implementieren:**
    ```bash
    # Nachricht verstecken
    ./steganography encode input.png "Meine streng geheime Nachricht!" output.png

    # Nachricht auslesen
    ./steganography decode output.png
    ```
3.  **Testen:** Überprüfe dein Programm mit verschiedenen Bildern (PNG, BMP) und Nachrichten (kurze und lange Texte, Sonderzeichen). Die dekodierte Nachricht muss **exakt** dem Original entsprechen.
4.  **Visueller Check:** Öffne `input.png` und `output.png` und vergleiche sie. Fällt dir mit bloßem Auge ein Unterschied auf? (Die Antwort sollte "Nein" lauten).
