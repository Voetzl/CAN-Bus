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
| **Hochschule:**          | `Technische Universität Freiberg`                                                                                                                                                    |
| **Inhalte:**             | `Einführung in Komponenten und Funktionsweise des CAN-Bus`                                                                                            |
| **Link auf den GitHub:** | [https://github.com/Voetzl/CAN-Bus](https://github.com/Voetzl/CAN-Bus) |
| **Autoren**              | @author                                                                                                                                                                              |

![](https://cdn.discordapp.com/attachments/667797054474420238/985561027032793178/unknown.png)

---

## Motivation (für eure Aufmerksamkeit)

| Kurze Übersicht |                                   |
| --------------- | --------------------------------- |
| Hintergründe    | Geschichte und Einordnung         |
| Aufbau          | Elektrische Funktionsweise        |
| Datenübertragung | Datenpakete, Prioritäten, Getriggerte Komponenten |
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

Das "Can-Bus-Kabel" besteht aus zwei ineinander verdrehten Drähten. Eine gute Abschirmung ist nicht zwingend notwendig.#

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

Aufbau eines Datenübertragungspakets

Prioriätetensetzung

Wiederholung bei höherer Priorität / bei Fehlerhafter Übertragung

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

