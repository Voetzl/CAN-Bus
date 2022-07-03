<!--

author:   Bastian Zötzl
email:    bastian.zoetzl@outlook.com

version:  0.1.2
language: de
narrator: Deutsch Female

import:  https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
         https://github.com/LiaTemplates/AVR8js/main/README.md


icon: https://upload.wikimedia.org/wikipedia/commons/d/de/Logo_TU_Bergakademie_Freiberg.svg

@ttc: <a title='@0' href="#" class="lia-link"><sup><button aria-describedby='footnote-@1' class="lia-btn lia-btn--transparent text-highlight" id='footnote-key-@1' style="padding: 2px;">[@1]</button></sup></a>
@tt: @ttc(@0, ? )

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://github.com/Voetzl/CAN-Bus/blob/main/README.md)

# CAN-Bus

| Parameter               | Kursinformationen                                                    |
| ----------------------- | -------------------------------------------------------------------- |
| **Veranstaltung**       | `Praktikum Digitale Systeme`                                         |
| **Hochschule**          | `Technische Universität Bergakademie Freiberg`                       |
| **Inhalte**             | `Einführung in die Funktionsweise des CAN-Bus`                       |
| **Link auf GitHub**     | https://github.com/Voetzl/CAN-Bus                                    |
| **Autoren**             | @author                                                              |

![](https://cdn.discordapp.com/attachments/667797054474420238/987747554710138940/20220614_234617.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987748058852888628/20220428_235124.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749165222199336/20220614_231527.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749227461476352/20220615_164647.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749277394669648/20220615_192457.jpg)
![](https://cdn.discordapp.com/attachments/466942400267419648/992125116362264696/20220615_192457.jpg)

## Motivation (für Eure Aufmerksamkeit)

| Kurze Übersicht                   |                                                            |
| --------------------------------- | ---------------------------------------------------------- |
| Hintergründe                      | Geschichte und Einordnung                                  |
| Aufbau                            | Elektrische Funktionsweise                                 |
| Datenübertragung                  | Datenpakete, Prioritäten, Synchronisation, Priorisierung   |
| Anwendung IRL@tt(`in Real-Life`)  | CAN im Golf                                                |
| CAN auf Microcontrollern          | CAN RX/TX, CAN-Transceiver                                 |
| Quallen                           |                                                            |
| Anwendung TFY@tt(`x for you`)     | Überraschung :)                                            |

![](https://cdn.discordapp.com/attachments/667797054474420238/985561027032793178/unknown.png)


## Geschichte und Einordnung

    {{0-1}}
********************************************************************************

**1987 stellt Bosch das Controller-Area-Network vor**

* Entwicklung begann 1981 (Fertigungstechnik), 1983 begann die Weiterentwicklung für Kraftfahrzeuge
* Daimler nutzt erste Serienanwendung in 1991
* Jahrtausendwende bringt Weiterentwicklungen: Subsystem LIN (Local Interconnect Network)
* Große Weiterentwicklung in 2012 mit vergrößertem Datenfeld auf 64 Byte (urspr. 8)

![](https://cdn.discordapp.com/attachments/667797054474420238/985571641411129404/unknown.png)

********************************************************************************

    {{1-2}}
********************************************************************************

**Der CAN-Bus gehört zu den Feldbussen**

```text @plantUML.png
@startuml
ditaa
+------------------------+   +---------------------+   +-----------+
|cF88                    |   |cFF4                 |   |c8F8       |
| Serielle Kommunikation |-->| Parallelverdrahtung |-->| Feldbusse |
|                        |   |                     |   |           |
+------------------------+   +---------------------+   +-----------+
@enduml
```

Feldbusse sind die Weiterentwicklung der Parallelverdrahtung um den Kabelbedarf zu mindern. Die Notwendigkeit besteht vor allem in der Industrie.
Dabei werden mit "Feldbus" alle Bussysteme angesprochen, die 'Sensoren und Aktoren zum Informationsaustausch mit einem Steuerungsrechner verbinden'. Jeder Hersteller hat dazu seine eigene*n

* Topologie (Stern, Linie, Baum)
* Fehlererkennung / -vermeidung
* Protokollstandards

entworfen. Zusätzlich unterscheiden sich die Systeme in ihrer/n 

* maximalen Kabellänge
* maximalen Datenbytes
* Übertragungsmedien

Beim CAN-Bus ist kein Steuerungsrechner notwendig, er zählt daher auch nicht zu den "klassischen" Feldbussen.

![](https://cdn.discordapp.com/attachments/667797054474420238/985578194444894268/unknown.png)

********************************************************************************

## Elektrische Funktionsweise

Das "Can-Bus-Kabel" besteht aus zwei ineinander verdrehten Drähten. Eine gute Abschirmung ist nicht zwingend notwendig. Die Impedanz des Kabels muss laut ISO im Bereich zwischen 100–130 Ohm liegen.

![](https://cdn.discordapp.com/attachments/667797054474420238/986336641914396773/unknown.png)

Zusätzlich müssen an den Enden Abschlusswiderstände von 120 Ohm angebracht sein. Solche "line terminations" sind keine Besonderheit des CAN-Bus und lassen sich über elektrotechnische Phänomene erklären.

{{1}} Bei der CAN-Übertragung heißt die logische 1 "rezessiv", die logische 0 "dominant", wobei der rezessive Zustand der Ruhezustand ist. Wird eine logische 0 gesendet, wechselt CANH aus ein höheres Spannungsniveau, CANL auf ein niedrigeres. 

{{1}} ![](https://cdn.discordapp.com/attachments/667797054474420238/986333441253588992/unknown.png)

{{2}} Beide Stränge müssen dabei perfekt gespiegelt laufen!

    {{2-3}}
********************************************************************************

![](https://cdn.discordapp.com/attachments/667797054474420238/986334487904387162/unknown.png)

********************************************************************************

    {{3-4}}
********************************************************************************

![](https://cdn.discordapp.com/attachments/667797054474420238/986342156736278578/unknown.png)

> Ein dominantes Bit wird von Transceivern üblicherweise ab ΔV > 2V erkannt, wobei freilich CANH den höheren Wert besitzen muss.

********************************************************************************

## Datenübertragung

> Verbreitete CAN-Bus-Geschwindigkeiten sind 125kbit/s (Lowspeed), 250kbit/s, 500kbit/s und 1Mbit/s (Highspeed). Dabei wird eine Kabellänge von bis 40m betrachtet.

**Aufbau eines Datenübertragungspakets**

{{0}} ![](https://cdn.discordapp.com/attachments/667797054474420238/986730062294437928/unknown.png)

    {{1-2}}
********************************************************************************

![](https://cdn.discordapp.com/attachments/667797054474420238/986741135638659072/unknown.png)

********************************************************************************

    {{2-3}}
********************************************************************************

**CAN 2.0A / Standard-CAN**

| Feld            | Größe        | Bedeutung                             |
| --------------- | ------------ | ------------------------------------- |
| Start           | 1 bit        | dominant, zur Syncronisation          |
| Identifier      | 11 bits      | Prioritätenübermittlung               |
| RTR             | 1 bit        | Anforderung (dominant) / Senden       |
| IDE             | 1 bit        | Identifier Extension (CAN2.0 A / B)   |
| r0              | 1 bit        | "reserviert", in KFZ ungenutzt        |            
| DLC             | 4 bits       | data length code                      |
| DATA            | 64 bit (max) | 1 - 8 Byte                            |
| CRC             | 15 bits      | 14 Fehlererkennungsbits, 1 Delimiter  |
| ACK             | 2 bits       | 1 Acknowledge, 1 Delimiter            |
| EOF             | 7 bits       | end of field, rezessiv                |
| IFS             | 3 / 7 bits   | rezessiv, Puffer-Feld!                | 

********************************************************************************

    {{3-4}}
********************************************************************************

**CAN 2.0B / Extended CAN**

| Feld            | Größe        | Bedeutung                                   |
| --------------- | ------------ | ------------------------------------------- |
| Start           | 1 bit        | dominant, zur Syncronisation                |
| Identifier      | 11 bits      | Prioritätenübermittlung                     |
| **SRR**         | **1 bit**    | **Ersetzt RTR, Platzhalterfunktion**        |
| **IDE**         | **1 bit**    | **Rezessiv weißt auf mehr Identifier hin**  |
| **Identifier**  | **18 bit**   | **18 weitere Identifier-bits**              |
| RTR             | 1 bit        | Anforderung (rezessiv) / Senden             |
| **r1**          | **1 bit**    | **"reserviert", in KFZ ungenutzt**          |
| r0              | 1 bit        | "reserviert", in KFZ ungenutzt              |            
| DLC             | 4 bits       | data length code                            |
| DATA            | 64 bit (max) | 1 - 8 Byte                                  |
| CRC             | 15 bits      | 14 Fehlererkennungsbits, 1 Delimiter        |
| ACK             | 2 bits       | 1 Acknowledge, 1 Delimiter                  |
| EOF             | 7 bits       | end of field, rezessiv                      |
| IFS             | 3 / 7 bits   | rezessiv, Puffer-Feld!                      | 

> Für genauere Infos zu den CRC-Bits: https://www.mikrocontroller.net/articles/CAN_CRC_Berechnung und https://www.can-cia.org/can-knowledge/can/crc/

> [Bit-Stuffing](https://www.youtube.com/watch?v=gglX5zJSdJk)

********************************************************************************

    {{4-5}}
********************************************************************************

**Typen von Nachrichten**

 *The Data Frame*

--> Normale Übertragung

 *The Remote Frame*

--> RTR rezessiv, kein Datenfeld 

 *The Error Frame*

--> Spezieller Frame, wird bei Fehler ausgelöst

 *The Overload Frame*

--> Buffer-Wirkung: Wird bei Überlastung zur Verzögerung genutzt

********************************************************************************

### Synchronisation

> Ein CAN-Bit ist nicht einfach nur ein Bit!

![](https://cdn.discordapp.com/attachments/667797054474420238/987729793216823366/unknown.png)
 
| Segment         | Länge                             |
| --------------- | --------------------------------- |
| SYNC-SEG        | 1 Time Quantum lang, Synchronisation |
| PROP_SEG        | Bis 8 tq einstellbar              |
| PHASE_SEG 1     | Bis 8 tq einstellbar (Länger bei Resynchronisation) |
| PHASE_SEG 2     | Maximal so lang wie PHASE_SEG 1 und Verarbeitungszeit |

> Für einen CAN-Bus mit 1000 kbps muss der Tranceiver-Chip mit mindestens 8 MHz getaktet werden!

    {{1-2}}
********************************************************************************

**Arten der Reynchronisation**

- *Hard Synchronisation:* Neustart der Transceiver-Clock-Zeit durch diese Flanke
- *Resynchronisation*: Kleinere Anpassungen durch SJW (Synchronisation-Jump-Width) Paramter des Transceivers

********************************************************************************

### Fehlererkennung 

    {{0}}
********************************************************************************

Das CAN-Protokoll beinhaltet 5 Fehlerüberprüfungsmethoden. Drei davon sind aus "Nachrichten-Level", zwei davon auf "Bit-Level". Sobald einer der Fehler auftritt wird ein ***Error-Frame*** generiert. 

Da immer alle Teilnehmer (auch der Sender selbst) mitlesen wird eine Fehlererkennung sicher gewährleistet.

********************************************************************************

    {{1}}
********************************************************************************

**Fehler auf Nachrichten-Level**

- 15 + 1 CRC Bits
- 1 + 1 ACK Bits
- Form check: SOF, EOF, ACK delimiter und CRC delimiter müssen rezessiv sein

> **Die Delimiter-Bits sind die Bits, die die CRC und ACK bits verifizieren, da diese sich logischerweise nicht bereits selbst kontrollieren können**

********************************************************************************

    {{2}}
********************************************************************************

**Fehler auf Bit-Level**

- Sender schaut direkt, ob er das gesandte auch lesen kann. Ausnahme sind die Identifier-Bits und das ACK-Bit
- Bit stuffing-Regel: Nach fünf folgenden Bits gleichen logischen Levels **muss** ein anderes folgen! 

> Bei einem erkannten Fehler wird der erwähnte ***Error-Frame*** erzeugt. Dieser besteht aus aus 6 dominanten Bits (<-> Bit-stuffing) und löst bei allen weiteren Teilnehmern einen Fehler aus. Ein ***Error-Frame*** kann also aus 6-12 dominanten Bits bestehen. Daraufhin folgen 8 rezessive Bits und eine Neusendung der selben Nachricht wird garantiert! 

********************************************************************************

### Priorsierung

Normalerweise gilt: Wer zuerst kommt, der kommt dran. Stauen sich während einer Übermittlung jedoch Nachrichten an, starten diese danach zum selben Zeitpunkt.

![](https://cdn.discordapp.com/attachments/667797054474420238/987725955927773204/unknown.png)

Die Sender, der dabei den niedrigsten Identifier sendet, darf weitersenden.

> Extended-CAN-Identifier haben immer eine niedrigere Prioriät!

> **Bei Überlastung des CANs kann es schnell passieren, dass manche Sender nie "zu Wort" kommen!**


## Beispiel Lichtanlage GOLF V

Schalten der Lichtanlage ändert "Data" der Nachricht

{{0-1}} ![](https://cdn.discordapp.com/attachments/667797054474420238/987744190601527336/unknown.png)

{{1-2}} ![](https://cdn.discordapp.com/attachments/667797054474420238/987741110577295370/unknown.png)

{{2-3}} ![](https://cdn.discordapp.com/attachments/667797054474420238/987742290380460043/unknown.png)

{{3-4}} !?[](https://www.youtube.com/watch?v=KIJFfHlRWNg)

## Anwendung des CAN-Protokolls auf dem Microcontroller

> Die uns bekannten Microcontroller können alle ein "digitales CAN-Signal" erzeugen!

Tatsächlich können sowohl unser kleiner 8-bit Arduino Uno, der Arduino Uno Wifi und natürlich unsere STMs ein CAN-TX-Signal erzeugen und ein CAN-RX Signal empfangen. Jedoch muss beachtet werden: Bei 8 tq / bit und einer Übertragungsgeschwindigkeit von 1000 kbit/s wäre der kleine Arduino vollständig ausgelastet alleine durch das Mitlesen!

Nachdem das "digitale CAN-Signal" generiert wurde, muss es über einen Transceiver auf die richtigen Spannungslevel konvertiert werden.

    {{1-2}}
********************************************************************************

**Can-Transceiver**

![](https://cdn.discordapp.com/attachments/667797054474420238/987747554122956830/20220614_234545.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/988025542479581214/20220619_120953.jpg)

> Diese Transceiver halten CANH und CANL durchgehend auf bestimmten Spannungsleveln und sind dabei sehr stromhungrig, weshalb diese ICs ziemlich häufig Sleep-Modi integriert haben 

[Datenblatt SN65HVD231](https://www.ti.com/lit/ds/symlink/sn65hvd231.pdf)

********************************************************************************

## Quallen

Datensammlung für die Präsentation fand zwischen 12.06 - 19.06 statt. In diesem Zeitraum wurden die Informationen auf u.g. Websiten gefunden. Bei abweichenden Informationen wurde jeweils eine dritte Quelle zum Vergleich herangezogen.

**Geschichte und Einordnung**

Teil 1: Text: [CAN- Historie, Standards und Eigenschaften (kunbus.de)](https://www.kunbus.de/can-historie-standards-und-eigenschaften.html)

        Bild: [Zeitlinien-Bild](https://cdn.shopify.com/s/files/1/0579/8032/1980/files/can-bus-history-timeline-controller-area-network.svg?v=1633690040)

Teil 2: Text: [Feldbus Grundlagen und Erklärung – KUNBUS GmbH](https://www.kunbus.de/feldbus-grundlagen.html),
              [Industrielle Kommunikation](https://www.lappkabel.de/branchen/industrielle-kommunikation/feldbus.html)

        Bild: [Feldbus-Aufbau](https://www.itwissen.info/lex-images/Aufbau-eines-Feldbusses.png)

**Elektronische Funktionsweise**

Bild: [CAN-Bus Kabel & Leitungen nach ISO 11898-2 (sab-kabel.de)](https://www.sab-kabel.de/kabel-konfektion-temperaturmesstechnikabel-und-leitungen/buskabel-und-leitungen/can-bus-kabel-leitungen.html)

Text + restl. Bilder: [How CAN bus works \| How data transmitted on CAN bus](https://www.youtube.com/watch?v=EIVQzv6-LRo),
                      [CAN-BUS Grundlagen - Diagnose am Fahrzeug](https://www.youtube.com/watch?v=izVTPsujT8U),
                      Eigenproduktion "krasse paint.net Skills"

**Datenübertragung**

Übergreifend: [Introduction to the Controller Area Network (CAN) (Rev. B)](https://www.ti.com/lit/an/sloa101b/sloa101b.pdf)

Text: [canbus.pdf (mikrocontroller.net)](https://www.mikrocontroller.net/attachment/6819/canbus.pdf)

Bilder: [CAN Protokoll Aufbau und Funktion](https://www.youtube.com/watch?v=ak03HkdX1Cw)

*Synchronisation*

Text / Bild: [CAN Bit Timing (kvaser.com)](https://www.kvaser.com/about-can/the-can-protocol/can-bit-timing/),
             [CAN Bit Time Calculation (can-wiki.info)](http://www.bittiming.can-wiki.info/)

*Fehlererkennung*

s. Übergreifend

*Priorisierung*

s. Übergreifend,
eigenes Verständnis

**Golf-Beispiel**

Text / Bilder: [Praktikum CAN-Bus am Fahrzeug (Beispiel Beleuchtung Golf V)](https://www.youtube.com/watch?v=KIJFfHlRWNg)

**Anwendung auf dem Microcontroller**

Text: Eigene Rechnungen,
      Datenblatt des verwendeten Transceivers (s. GitHub-Ordner: [Dateien](https://github.com/Voetzl/CAN-Bus/Dateien/tree/main/Dateien)) **<- Den Ordner gibt's gar nicht!**

Bild: Eigenproduktion "krasse Handykamera Skills"     

**Ist es ein Auto?**

Videos und Artikel, die zur Hilfe herangezogen wurden:

Hauptsächlich: [How to use CAN Protocol in STM32 » ControllersTech](https://controllerstech.com/can-protocol-in-stm32/)


## Ist es ein Auto?

Code Implementierung 1

Code Implementierung 2

Code Implementierung 3


