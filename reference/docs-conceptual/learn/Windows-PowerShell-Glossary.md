---
ms.date: 06/05/2017
keywords: powershell,cmdlet
title: Windows PowerShell-Glossar
ms.assetid: b0f88cbe-cb83-4912-a301-184534cb35c7
ms.openlocfilehash: fd15667939fd9b3ea705806686b626645519588a
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62057349"
---
# <a name="windows-powershell-glossary"></a>Windows PowerShell-Glossar


|Begriff|Definition|
|--------|--------------|
|Binäres Modul|Ein Windows PowerShell-Modul, dessen Stammmodul eine binäre Moduldatei (DLL) ist. Ein binäres Modul kann ein Modulmanifest enthalten.|
|Allgemeiner Parameter|Ein Parameter, der von der Windows PowerShell-Engine allen Cmdlets, erweiterten Funktionen und Workflows hinzugefügt wird.|
|Punkt als Stammelement|Um in Windows PowerShell einen Befehl zu starten, geben Sie einen Punkt und ein Leerzeichen vor dem Befehl ein. Befehle mit Punkt als Stammelement werden im aktuellen Bereich anstatt im neuen Bereich ausgeführt. Alle Variablen, Aliase, Funktionen oder Laufwerke, die der Befehl erstellt, werden im aktuellen Bereich erstellt und sind für Benutzer verfügbar, wenn der Befehl abgeschlossen ist.|
|Dynamisches Modul|Ein Modul, das nur im Arbeitsspeicher vorhanden ist. Die Cmdlets „New-Module“ und „Import-PSSession“ erstellen dynamische Module.|
|Dynamischer Parameter|Parameter, der einem Windows PowerShell-Cmdlet, einer Funktion oder Skript einen unter bestimmten Umständen hinzugefügt wird. Cmdlets, Funktionen, Anbieter und Skripts können dynamische Parameter hinzufügen.|
|Formatierungsdatei|Eine Windows PowerShell-XML-Datei mit der Erweiterung „.format.ps1xml“, die definiert, wie Windows PowerShell ein Objekt basierend auf seinem .NET Framework-Typ angezeigt.|
|Globaler Sitzungsstatus|Der Sitzungsstatus, der die Daten enthält, auf die der Benutzer einer Windows PowerShell-Sitzung zugreifen kann.|
|Host|Die Schnittstelle, die die Windows PowerShell-Engine für die Kommunikation mit dem Benutzer verwendet. Der Host gibt beispielsweise an, wie Eingabeaufforderungen zwischen Windows PowerShell und dem Benutzer behandelt werden.|
|Hostanwendung|Programm, das die Windows PowerShell-Engine in seinen Prozess lädt und verwendet, um Vorgänge auszuführen.|
|Eingabeverarbeitungsmethode|Methode, die ein Cmdlet verwenden kann, um die Datensätze, die es empfängt, als Eingabe zu nutzen. Eingabeverarbeitungsmethoden sind z. B. „BeginProcessing“, „ProcessRecord“, „EndProcessing“und „StopProcessing“.|
|Manifestmodul|Windows PowerShell-Modul, das über ein Manifest verfügt und dessen RootModule-Schlüssel leer ist.|
|Modulmanifest|Windows PowerShell-Datendatei (.psd1), die den Inhalt eines Moduls beschreibt und steuert, wie ein Modul verarbeitet wird.|
|Modulsitzungsstatus|Sitzungsstatus mit den öffentlichen und privaten Daten eines Windows PowerShell-Moduls. Die privaten Daten im Sitzungsstatus sind für den Benutzer einer Windows PowerShell-Sitzung nicht verfügbar.|
|Fehler ohne Abbruch|Fehler, der Windows PowerShell nicht an der weiteren Verarbeitung des Befehls hindert.|
|Nomen|Wort, das in einen Windows PowerShell-Cmdlet-Namen auf den Bindestrich folgt. Das Nomen beschreibt die Ressourcen, auf die das Cmdlet angewendet wird.|
|Parametersatz|Gruppe von Parametern, die im selben Befehl verwendet werden kann, um eine bestimmte Aktion auszuführen.|
|Pipe|Dient in Windows PowerShell zum Weiterleiten der Ergebnisse des vorherigen Befehls als Eingabe an den nächsten Befehl in der Pipeline.|
|Pipeline|Reihe von Befehlen, die durch Pipelineoperatoren (&#124;) (ASCII 124) verbunden sind. Jeder Pipelineoperator leitet die Ergebnisse des vorherigen Befehls als Eingabe an den nächsten Befehl weiter.|
|PSSession|Typ von Windows PowerShell-Sitzung, die vom Benutzer erstellt, verwaltet und geschlossen wird.|
|Stammmodul|Das Modul, das im RootModule-Schlüssel in einem Modulmanifest angegeben ist.|
|Runspace|In Windows PowerShell die Betriebsumgebung, in der jeder Befehl in einer Pipeline ausgeführt wird.|
|Skriptblock|In der Windows PowerShell-Programmiersprache eine Auflistung von Anweisungen oder Ausdrücken, die als einzelne Einheit verwendet werden kann. Ein Skriptblock kann Argumente und Rückgabewerte akzeptieren.|
|Skriptmodul|Windows PowerShell-Modul, dessen Stammmodul eine Skriptmoduldatei (.psm1) ist. Ein Skriptmodul kann ein Modulmanifest enthalten.|
|Skriptmoduldatei|Datei, die ein Windows PowerShell-Skript enthält. Das Skript definiert die Elemente, die das Skriptmodul exportiert. Skriptmoduldateien haben die Erweiterung „.psm1“.|
|Shell|Befehlsinterpreter, der verwendet wird, um Befehle an das Betriebssystem zu übergeben.|
|Switch-Parameter|Parameter, der kein Argument akzeptiert.|
|Fehler mit Abbruch|Fehler, der verhindert, dass Windows PowerShell die Verarbeitung des Befehls abschließt.|
|Transaktion|Unteilbare Arbeitseinheit. Die Arbeit in einer Transaktion muss als Ganzes abgeschlossen werden. Wenn bei einem Teil der Transaktion ein Fehler auftritt, misslingt die gesamte Transaktion.|
|Typdatei|Windows PowerShell-XML-Datei mit der Erweiterung „.ps1xml“, die die Eigenschaften der Microsoft .NET Framework-Typen in Windows PowerShell erweitert.|
|Verb|Wort, das in einen Windows PowerShell-Cmdlet-Namen vor dem Bindestrich steht. Das Verb beschreibt die Aktion, die das Cmdlet ausführt.|
|Windows PowerShell|Eine Befehlszeilenshell und aufgabenbasierte Technologie zur Skripterstellung, die IT-Administratoren eine umfassende Kontrolle und Automatisierung von Systemverwaltungsaufgaben ermöglicht.|
|Windows PowerShell-Befehl|Die Elemente in einer Pipeline, die dazu führen, dass eine Aktion durchgeführt wird. Windows PowerShell-Befehle werden entweder über die Tastatur eingegeben oder programmgesteuert aufgerufen.|
|Windows PowerShell-Datendatei|Textdatei mit der Erweiterung „.psd1“. Windows PowerShell verwendet Datendateien für verschiedene Zwecke, z. B. Speichern von Modulmanifestdaten und übersetzten Zeichenfolgen für die Skriptinternationalisierung.|
|Windows PowerShell-Laufwerk|Virtuelles Laufwerk, das direkten Zugriff auf einen Datenspeicher bietet. Es kann von einem Windows PowerShell-Anbieter definiert oder über die Befehlszeile erstellt werden. Laufwerke, die über die Befehlszeile erstellt werden, sind sitzungsspezifisch und gehen verloren, sobald die Sitzung geschlossen wird.|
|Windows PowerShell Integrated Scripting Environment (ISE)|Windows PowerShell-Hostanwendung, mit der Sie in einer benutzerfreundlichen, Unicode-kompatiblen Umgebung mit farblich gekennzeichneter Syntax Befehle ausführen und Skripts schreiben, testen und debuggen können.|
|Windows PowerShell-Modul|Eine unabhängige, wiederverwendbare Einheit, mit deren Hilfe Sie Ihren Windows PowerShell-Code partitionieren, organisieren und abstrahieren können. Ein Modul kann Cmdlets, Anbieter, Funktionen, Variablen und andere Ressourcentypen enthalten, die als Einheit importiert werden können.|
|Windows PowerShell-Anbieter|Auf Microsoft .NET Framework basierendes Programm, das die Daten in einem speziellen Datenspeicher in Windows PowerShell verfügbar macht, damit Sie diese anzeigen und verwalten können.|
|Windows PowerShell-Skript|Skript, das in der Windows PowerShell-Sprache geschrieben ist.|
|Windows PowerShell-Skriptdatei|Datei mit der Erweiterung „.ps1“, die ein Skript enthält, das in der Windows PowerShell-Sprache geschrieben ist.|
|Windows PowerShell-Snap-In|Ressource, die einen Satz von Cmdlets, Anbietern und Microsoft .NET Framework-Typen definiert, die der Windows PowerShell-Umgebung hinzugefügt werden können.|
|Windows PowerShell-Workflow|Bei einem Workflow handelt es sich um eine Sequenz von programmierten, zusammenhängenden Schritten, mit denen zeitaufwendige Aufgaben ausgeführt werden oder für die mehrere Schritte auf mehreren Geräten oder verwalteten Knoten koordiniert werden müssen. Der Windows PowerShell-Workflow ermöglicht es IT-Spezialisten und Entwicklern, Sequenzen von Verwaltungsaktivitäten für mehrere Geräte oder einzelne Aufgaben innerhalb eines Workflows als Workflows zu erstellen. Mithilfe des Windows PowerShell-Workflows können Sie Windows PowerShell-Skripts und XAML-Dateien als Workflows anpassen und erstellen.|