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

![](https://cdn.discordapp.com/attachments/667797054474420238/987747554710138940/20220614_234617.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987748058852888628/20220428_235124.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749165222199336/20220614_231527.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749227461476352/20220615_164647.jpg)
![](https://cdn.discordapp.com/attachments/667797054474420238/987749277394669648/20220615_192457.jpg)


---

## Motivation (für eure Aufmerksamkeit)

| Kurze Übersicht |                                   |
| --------------- | --------------------------------- |
| Hintergründe    | Geschichte und Einordnung         |
| Aufbau          | Elektrische Funktionsweise        |
| Datenübertragung | Datenpakete, Prioritäten, Synchronisation, Priorisierung |
| Anwendung IRL   | CAN im Golf                       |
| CAN auf Microcontrollern | CAN RX/TX, CAN-Transceiver |            
| Quallen         |                                   |
| Anwendung TFY   | Überraschung :)                   |

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

## no
