---
Author: "kevin.ohsystems"
Title: "Fernwirkprotokolle"
SourceRef: "" 
TargetRef: ""
BibResource: "bibliography.bib"
---

# Fernwirkprotokolle 

## IEC 60870-5-104

Die IEC 60870-5-104 ist ein allgemeines Übertragungsprotokoll zwischen (Netz-)Leitsystemen und Unterstationen.

Die Telegramme werden per Internetprotokoll TCP/IP übertragen. Das Protokoll besitzt allgemeine Fähigkeiten im Rahmen von SCADA-Anwendungen.

Im Gegensatz zur IEC 60870-5-101, die Verbindungen über serielle Schnittstellen aufbaut, ermöglicht die IEC 60870-5-104 Schnittstelle die Kommunikation über Netzwerke (LAN und WAN). Dabei können handelsübliche Netzwerkkomponenten (Router) verwendet werden (in Schaltanlagen sollten Industriekomponenten verwendet werden). Als Standard wird hierbei der TCP-Port 2404 verwendet.

Mit der Norm IEC 60870-5-104 wurde erreicht, dass Geräte und Anlagen der Fernwirk- und Leittechnik verschiedener Hersteller ohne grundsätzliche Anpassungsentwicklungen miteinander kommunizieren können. Die Freiheitsgrade der Norm erlauben verschiedene lieferantenspezifische Profile (z. B. der verwendeten Telegrammtypen und Funktionen). Mit Hilfe einer Interoperabilitätsliste können die Profile aufeinander abgestimmt werden. Diese Norm hat vor allem im europäischen und asiatischen Raum weite Verbreitung gefunden.

Das Fernwirkprotokoll IEC 60870-5-104 eignet sich auch als Feld- oder Stationsbus. Der Einsatz als Stationsbus ermöglicht dabei auch eine direkte Kommunikation zwischen einzelnen Geräten.

Als Stations- oder Feldbus, auf Basis TCP/IP, wird heute ebenfalls die IEC 61850, die auf einem objektorientierten Datenmodell aufbaut, eingesetzt. 

## Modbus 

Das Modbus-Protokoll ist ein Kommunikationsprotokoll, das auf einer Client/Server-Architektur basiert. Es wurde 1979 von Gould-Modicon für die Kommunikation mit seinen speicherprogrammierbaren Steuerungen ins Leben gerufen.[1] In der Industrie hat sich der Modbus zu einem De-facto-Standard entwickelt, da es sich um ein offenes Protokoll handelt. Seit 2007 ist die Version Modbus/TCP Teil der Norm IEC 61158.
Inhaltsverzeichnis
Grundlagen

Mittels Modbus können ein Client (z. B. ein PC) und mehrere Server (z. B. Mess- und Regelsysteme) verbunden werden. Es gibt zwei Versionen: Eine für die serielle Schnittstelle (EIA-232 und EIA-485)[2] und eine für Ethernet.

Bei der Datenübertragung werden drei verschiedene Betriebsarten unterschieden:

    Modbus/RTU
    Modbus/ASCII
    Modbus/TCP

Jeder Busteilnehmer muss eine eindeutige Adresse besitzen. Die Adresse 0 ist dabei für einen Broadcast reserviert. Jeder Teilnehmer darf Nachrichten über den Bus senden. In der Regel wird dies jedoch durch den Client initiiert und ein adressierter Server antwortet.

Lese- und Schreibzugriffe sind auf folgende Objekttypen möglich:
Objekttyp   Zugriff     Größe   Funktionscode
Einzelner Ein-/Ausgang „Coil“   Lesen & Schreiben   1-bit   01 / 05 / 15
Einzelner Eingang „Discrete Input“  nur Lesen   1-bit   02
(analoge) Eingänge „Input Register“     nur Lesen   16-bits     04
(analoge) Ein-/Ausgänge „Holding Register“  Lesen & Schreiben   16-bits     03 / 06 / 16

## Modbus/RTU

Modbus RTU (RTU: Remote Terminal Unit, Fernbedienungsterminal) überträgt die Daten in binärer Form. Dies sorgt für einen guten Datendurchsatz, allerdings können die Daten nicht direkt vom Menschen ausgewertet werden, sondern müssen zuvor in ein lesbares Format umgesetzt werden.

Protokollaufbau[2]

Im RTU-Modus wird der Sendebeginn durch eine Sendepause von mindestens der 3,5-fachen Zeichenlänge markiert. Ein Zeichen hat eine Länge von 11 Bit. Die Länge der Sendepause hängt somit von der Übertragungsgeschwindigkeit ab. Dies muss bei niedrigen Datenraten exakt eingehalten werden. Bei einer Bitrate von mehr als 19200 bit/s kann eine feste Pausenzeit von 1,75 ms verwendet werden. Das Adressfeld besteht aus acht Bit, die die Empfängeradresse darstellen. Der Server sendet bei seiner Antwort an den Client ebendiese Adresse zurück, damit der Client die Antwort zuordnen kann. Das Funktionsfeld besteht aus 8 Bit. Hat der Server die Anfrage des Client korrekt empfangen, so antwortet er mit demselben Funktionscode. Ist ein Fehler aufgetreten, so verändert er den Funktionscode, indem er das höchstwertige Bit des Funktionsfeldes auf 1 setzt. Das Datenfeld enthält Hinweise, welche Register der Server auslesen soll, und ab welcher Adresse diese beginnen. Der Server setzt dort die ausgelesenen Daten (z. B. Messwerte) ein, um sie an den Client zu senden. Im Fehlerfall wird dort ein Fehlercode übertragen. Das Feld für die Prüfsumme, die mittels CRC ermittelt wird, beträgt 16 Bit. Das gesamte Telegramm muss in einem kontinuierlichen Datenstrom übertragen werden. Tritt zwischen zwei Zeichen eine Sendeunterbrechung auf, die länger als 1,5 Zeichen ist, so ist das Telegramm als unvollständig zu bewerten und sollte vom Empfänger verworfen werden.
Start   Adresse     Funktion    Daten   CR-Check    Ende
Wartezeit (min. 3,5 Zeichen)    1 Byte  1 Byte  n Byte  2 Byte  Wartezeit (min 3,5 Zeichen)

## Modbus/ASCII

Im Modbus ASCII wird keine Binärfolge, sondern ASCII-Code übertragen. Dadurch ist es direkt für den Menschen lesbar, allerdings ist der Datendurchsatz im Vergleich zu RTU geringer.

Protokollaufbau

Im ASCII-Modus beginnen Nachrichten mit einem vorangestellten Doppelpunkt, das Ende der Nachricht wird durch die Zeichenfolge Carriage return – Line feed (CRLF) markiert.

Die ersten zwei Bytes enthalten zwei ASCII-Zeichen, die die Adresse des Empfängers darstellen. Der auszuführende Befehl ist auf den nächsten zwei Bytes codiert. Über weitere n Zeichen folgen die Daten. Über das gesamte Telegramm (ohne Start- und Ende-Markierung) wird zur Fehlerprüfung ein LRC ausgeführt, dessen Paritätsdatenwort in den abschließenden zwei Zeichen untergebracht wird. Tritt während der Übertragung eines Frames eine Pause von > 1 s auf, wird der Frame als Fehlerfall bewertet. Der Benutzer kann ein längeres Timeout konfigurieren.
Start   Adresse     Funktion    Daten   LR-Check    Ende
1 Zeichen (:)   2 Zeichen   2 Zeichen   n Zeichen   2 Zeichen   2 Zeichen (CRLF)

## Modbus/TCP

Modbus/TCP ist RTU sehr ähnlich, allerdings werden TCP/IP-Pakete verwendet, um die Daten zu übermitteln.[3][4] Der TCP-Port 502 ist für Modbus/TCP reserviert. Modbus/TCP ist seit 2007 in der Norm IEC 61158 festgelegt und wird in IEC 61784-2 als CPF 15/1 referenziert.

Protokollaufbau
Transaktionsnummer  Protokollkennzeichen    Zahl der noch folgenden Bytes   Adresse     Funktion    Daten
2 Byte  2 Byte (immer 0x0000)   2 Byte (n + 2)  1 Byte  1 Byte  n Byte

Dadurch, dass hier keine CRC-Prüfsummenbytes zu berechnen sind, ist die Implementierung eines Treibers für die TCP-Schnittstelle einfacher als für die serielle Schnittstelle, sofern man auf eine vorhandene TCP-Implementierung aufsetzen kann.

Modbus/TCP Security Protocol
Im Oktober 2018 wurde eine sichere Variante Modbus/TCP Protokoll auf Basis Transport Layer Security (TLS) veröffentlicht[5]. Diese nutzt X.509v3 digitale Zertifikate zur Authentifizierung von Server und Client. Damit sollen Angriffe auf vernetzte Modbus/TCP-Komponenten (z. B. Man-in-the-Middle-Angriffe) verhindert werden. Das sichere Modbus/TCP bietet auch eine rollenbasierte Zugriffssteuerung. Es nutzt den TCP-Port 802. Das Protokoll wird in der MODBUS/TCP Security Protocol Specification[6] beschrieben. 

Quelle: Wikipedia

SourceRef [*.md](*.md)
TargeRef [*.md](*.md)
