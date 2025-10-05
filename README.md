Dieses Projekt berührt alle geforderten Kenntnisse: C++, Bildverarbeitung und algorithmisches Denken. Es ist ein klassisches Thema aus der Bildforensik.

Projekt: Ein einfacher LSB-Steganographie-Encoder/Decoder
Die Idee: Steganographie ist die Kunst, eine Nachricht in einem anderen Medium (hier ein Bild) zu verstecken. Die einfachste Methode ist Least Significant Bit (LSB) Steganography. Dabei wird das niederwertigste Bit jedes Farbkanals (Rot, Grün, Blau) eines Pixels durch ein Bit der geheimen Nachricht ersetzt. Für das menschliche Auge ist diese winzige Änderung kaum sichtbar.

Warum dieses Projekt?

Direkter Bezug: Es ist ein Kernthema der Vorlesung (Steganographie).

C++ Übung: Du benötigst Klassen, File-I/O und Bit-Operationen.

Bildverarbeitung: Du musst Pixel für Pixel durch ein Bild iterieren und deren Farbwerte manipulieren.

Machbarkeit: In einer Woche gut schaffbar.

Tagesplan für das Projekt:
Tag 1: Setup und Bild laden/speichern.

Richte deine C++ Entwicklungsumgebung ein (z.B. Visual Studio Code mit g++).

Installiere eine einfache Bildverarbeitungsbibliothek. Empfehlung: stb_image.h und stb_image_write.h. Das sind zwei Header-Only-Bibliotheken, die extrem einfach zu benutzen sind. Du musst nichts kompliziertes kompilieren oder linken.

Schreibe ein C++ Programm, das ein Bild von der Festplatte lädt (z.B. eine PNG-Datei), die Dimensionen ausgibt und es unter einem neuen Namen wieder abspeichert.

Tag 2-3: Implementierung des Encoders.

Erstelle eine Funktion void encode(const char* imagePath, const std::string& message, const char* outputPath).

Lade das Bild.

Iteriere durch die Pixel des Bildes. Für jedes Bit deiner Nachricht message:

Nimm den Bytewert eines Farbkanals (z.B. Rot).

Setze das niederwertigste Bit (LSB) dieses Bytewertes auf 0 (z.B. color_byte = color_byte & 0xFE;).

Setze das LSB dann auf das aktuelle Bit deiner Nachricht (z.B. color_byte = color_byte | message_bit;).

Achte darauf, dass du nicht mehr Bits verstecken willst, als im Bild Platz haben (Breite * Höhe * 3). Es ist auch üblich, zuerst die Länge der Nachricht zu kodieren, damit der Decoder weiß, wann er aufhören muss zu lesen.

Speichere das modifizierte Bild im outputPath.

Tag 4-5: Implementierung des Decoders.

Erstelle eine Funktion std::string decode(const char* imagePath).

Lade das steganographisch veränderte Bild.

Iteriere wieder durch die Pixel und ihre Farbkanäle.

Extrahiere aus jedem Farbkanal das LSB (lsb = color_byte & 1;).

Sammle diese Bits und setze sie zu Bytes (und dann zu Zeichen) zusammen, um die ursprüngliche Nachricht zu rekonstruieren.

Höre auf, wenn du die (zuvor mitkodierte) Länge der Nachricht erreicht hast.

Tag 6-7: Testen und Aufräumen.

Erstelle eine main-Funktion, die Kommandozeilenargumente entgegennimmt, um entweder den Encoder oder den Decoder aufzurufen.

Beispielaufruf: ./steganography encode input.png "Geheime Nachricht" output.png

Beispielaufruf: ./steganography decode output.png

Teste dein Programm mit verschiedenen Bildern und Nachrichten. Überprüfe, ob die dekodierte Nachricht exakt der Originalnachricht entspricht.

Schau dir das output.png an. Siehst du einen Unterschied zum Original? Wahrscheinlich nicht!
