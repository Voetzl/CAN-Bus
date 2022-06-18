<!--

author:   Bastian Zötzl
email:    bastian.zoetzl@outlook.com

version:  0.0.6
language: de
narrator: Deutsch Female

import:  https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
         https://github.com/LiaTemplates/AVR8js/main/README.md


icon: https://upload.wikimedia.org/wikipedia/commons/d/de/Logo_TU_Bergakademie_Freiberg.svg

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://github.com/Voetzl/CAN-Bus/blob/main/README.md)

# CAN-Bus

| Parameter                | Kursinformationen                                                                                                                                                                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Veranstaltung:**       | `Praktikum Digitale Systeme`                                                                                                                                                      |
| **Hochschule:**          | `Technische Universität Bergakademie Freiberg`                                                                                                                                                    |
| **Inhalte:**             | `Einführung in die Funktionsweise des CAN-Bus`                                                                                            |
| **Link auf den GitHub:** | [https://github.com/Voetzl/CAN-Bus](https://github.com/Voetzl/CAN-Bus) |
| **Autoren**              | @author                                                                                                                                                                              |

![](https://cdn.discordapp.com/attachments/667797054474420238/985561027032793178/unknown.png)

---

## Motivation (für eure Aufmerksamkeit)

| Kurze Übersicht |                                   |
| --------------- | --------------------------------- |
| Hintergründe    | Geschichte und Einordnung         |
| Aufbau          | Elektrische Funktionsweise        |
| Datenübertragung | Datenpakete, Prioritäten, Synchronisation, Priorisierung |
| Anwendung IRL   | Lichtanlage Golf                  |
| CAN auf dem STM32 | CAN-Transceiver                 |            
| Quallen         |                                   |
| Anwendung TFY   | Überraschung :)                   |


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

Das "Can-Bus-Kabel" besteht aus zwei ineinander verdrehten Drähten. Eine gute Abschirmung ist nicht zwingend notwendig.

![](https://cdn.discordapp.com/attachments/667797054474420238/986336641914396773/unknown.png)

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

********************************************************************************


## Datenübertragung

**Aufbau eines Datenübertragungspakets**

{{0}} ![](https://cdn.discordapp.com/attachments/667797054474420238/986730062294437928/unknown.png)

                                  {{1-2}}
********************************************************************************

![](https://cdn.discordapp.com/attachments/667797054474420238/986741135638659072/unknown.png)

********************************************************************************

                                  {{2-3}}
********************************************************************************

**CAN 2.0A / Standard-CAN**

| Feld            | Größe   | Bedeutung                        |
| --------------- | ------- | ---------------------------------|
| Start           | 1 bit   | dominant, zur Syncronisation     |
| Identifier      | 11 bits | Prioritätenübermittlung          |
| RTR             | 1 bit   | Anforderung (dominant) / Senden  |
| IDE             | 1 bit   | Identifier Extension (CAN2.0 A / B) |
| r0              | 1 bit   | "reserviert", in KFZ ungenutzt   |            
| DLC             | 4 bits  | data length code                 |
| DATA            | 64 bit (max) | 1 - 8 Byte                  |
| CRC             | 15 bits |  14 Fehlererkennungsbits, 1 Delimiter |
| ACK             | 2 bits  | 1 Acknowledge, 1 Delimiter       |
| EOF             | 7 bits  | end of field, rezessiv           |
| IFS             | 3 / 7 bits | rezessiv, Puffer-Feld!        | 

********************************************************************************

                                  {{3-4}}
********************************************************************************

**CAN 2.0B / Extended CAN**

| Feld            | Größe   | Bedeutung                        |
| --------------- | ------- | ---------------------------------|
| Start           | 1 bit   | dominant, zur Syncronisation     |
| Identifier      | 11 bits | Prioritätenübermittlung          |
| **SRR**         | **1 bit**   | **Ersetzt RTR, Platzhalterfunktion** |
| **IDE**         | **1 bit**   | **Rezessiv weißt auf mehr Identifier hin**  |
| **Identifier**  | **18 bit**  | **18 weitere Identifier-bits**       |
| RTR             | 1 bit   | Anforderung (rezessiv) / Senden  |
| **r1**          | **1 bit**   | **"reserviert", in KFZ ungenutzt**   |
| r0              | 1 bit   | "reserviert", in KFZ ungenutzt   |            
| DLC             | 4 bits  | data length code                 |
| DATA            | 64 bit (max) | 1 - 8 Byte                  |
| CRC             | 15 bits |  14 Fehlererkennungsbits, 1 Delimiter |
| ACK             | 2 bits  | 1 Acknowledge, 1 Delimiter       |
| EOF             | 7 bits  | end of field, rezessiv           |
| IFS             | 3 / 7 bits | rezessiv, Puffer-Feld!        | 

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


{{0}} Das CAN-Protokoll beinhaltet 5 Fehlerüberprüfungsmethoden. Drei davon sind aus "Nachrichten-Level", zwei davon auf "Bit-Level". Sobald einer der Fehler auftritt wird ein ***Error-Frame*** generiert. 

{{0}} Da immer alle Teilnehmer (auch der Sender selbst) mitlesen wird eine Fehlererkennung sicher gewährleistet.

                                  {{1-2}}
********************************************************************************

**Fehler auf Nachrichten-Level**

- 15 + 1 CRC Bits
- 1 + 1 ACK Bits
- Form check: SOF, EOF, ACK delimiter und CRC delimiter müssen rezessiv sein

> **Die Delimiter-Bits sind die Bits, die die CRC und ACK bits verifizieren, da diese sich logischerweise nicht bereits selbst kontrollieren können**

********************************************************************************

                                  {{2-3}}
********************************************************************************

**Fehler auf Bit-Level**

- Sender schaut direkt, ob er das gesandte auch lesen kann. Ausnahme sind die Identifier-Bits und das ACK-Bit
- Bit stuffing-Regel: Nach fünf folgenden Bits gleichen logischen Levels **muss** ein anderes folgen! 

********************************************************************************

                                  {{3-4}}
********************************************************************************

> Bei einem erkannten Fehler wird der erwähnte ***Error-Frame*** erzeugt. Dieser besteht aus aus 6 dominanten Bits (<-> Bit-stuffing) und löst bei allen weiteren Teilnehmern einen Fehler aus. Ein ***Error-Frame*** kann also aus 6-12 dominanten Bits bestehen. Daraufhin folgen 8 rezessive Bits und eine Neusendung der selben Nachricht wird garantiert! 

********************************************************************************

### Priorsierung

Normalerweise gilt: Wer zuerst kommt, der kommt dran. Stauen sich während einer Übermittlung jedoch Nachrichten an, starten diese danach zum selben Zeitpunkt.

![](https://cdn.discordapp.com/attachments/667797054474420238/987725955927773204/unknown.png)

Die Sender, der dabei den niedrigsten Identifier sendet, darf weitersenden.

> Extended-CAN-Identifier haben immer eine niedrigere Prioriät!

> **Bei Überlastung des CANs kann es schnell passieren, dass manche Sender nie "zu Wort" kommen!**


## Beispiel GOLF


--> Auch noch Vorteil ggü. alles verdrahten
--> In kurz: Also da sendet jetzt hier die Lichtanlage einfach immer ihren Status, den kann man auch überschreiben...
--> Problem bei Überlastung
--Video am Ende mit einbetten

## Anwendung des CAN-Protokolls auf dem STM32

--> Eigentlich ist hier die Transceiver-Erklärung dann so drin...

## Mein Ultra-Tolles-Anwendungsbeispiel

--> Fotos auf Unterseite...

## Quellen

