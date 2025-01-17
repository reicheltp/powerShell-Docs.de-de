---
ms.date: 06/05/2017
keywords: powershell,cmdlet
title: Windows PowerShell Integrated Scripting Environment (ISE)
ms.assetid: f156b92d-0203-46d2-89c7-b4989d32e3d2
ms.openlocfilehash: 3002c2cb7213b1c2d7201dddf5ff3c0651fad767
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62057484"
---
# <a name="windows-powershell-integrated-scripting-environment-ise"></a>Windows PowerShell Integrated Scripting Environment (ISE)

Die Windows PowerShell Integrated Scripting Environment (ISE) ist einer von zwei Hosts der Engine und der Sprache von Windows PowerShell. Sie ermöglicht Ihnen, Skripts auf eine Weise zu schreiben, auszuführen und zu testen, die in der Windows PowerShell-Konsole nicht verfügbar ist. Diese ISE bietet zusätzlich Syntaxfarben, Vervollständigung mit der TAB-TASTE, IntelliSense, visuelles Debuggen und kontextbezogene Hilfe.

Die ISE ermöglicht das Ausführen von Befehlen in einem Konsolenbereich, unterstützt aber auch Bereiche, mit deren Hilfe Sie den Quellcode Ihres Skripts und andere Tools gleichzeitig anzeigen können, die in die ISE eingebunden werden können. Sie können sogar mehrere Skriptfenster gleichzeitig öffnen, was besonders hilfreich ist, wenn Sie ein Skript debuggen, das Funktionen nutzt, die in anderen Skripts oder Modulen definiert sind.

## <a name="whats-new"></a>Neu

Es folgen einige der Features, die der ISE in den neuesten Versionen von PowerShell hinzugefügt wurden.

### <a name="added-in-powershell-30-windows-server-2012-windows-8"></a>In PowerShell 3.0 hinzugefügt (Windows Server 2012, Windows 8)

**IntelliSense** vervollständigt automatisch Ihre Befehle, indem während der Eingabe Menüs übereinstimmender Cmdlets, Parameter, Parameterwerte, Dateien oder Ordner angezeigt werden.

**Codeausschnitte** sind kurze Abschnitte des Codes, die Sie in von Ihnen geschriebene Skripts einfügen können. Eine Sammlung nützlicher Codeausschnitte ist standardmäßig enthalten. Über das Cmdlet **New-Snippet** können Sie weitere hinzufügen.

Sie können **Add-On-Tools** zum Hinzufügen von Features zur ISE erstellen, indem Sie Code schreiben, der mit dem [Windows PowerShell ISE-Skriptobjektmodell](../../core-powershell/ise/The-ISE-Object-Model-Hierarchy.md) interagiert.

Diese Tools können Steuerelemente in einem Bereich mit Registerkarten anzeigen oder unsichtbar im Hintergrund arbeiten. Das Add-On **Befehle** ist ein gutes Beispiel und in Version 3.0 und höher enthalten. Es zeigt eine Liste der verfügbaren Befehle und ihre Hilfe an.

**Neustart-Manager und automatisches Speichern**. Dieses Features speichern Ihre Skripts automatisch alle zwei Minuten, um den Verlust Ihrer Arbeit im Fall eines Absturzes oder unerwarteten Neustarts zu verhindern.

Die **Liste „Zuletzt verwendet“** ist jetzt Teil des Menüs „Datei öffnen“, damit sie leichter auf die Dateien zugreifen können, die Sie am häufigsten verwenden.

**Zusammengeführter Konsolenbereich**. In vorherigen Versionen der ISE gab es separate Bereiche für Befehle und Ausgabe. Diese wurden zu einem einzelnen Bereich kombiniert, der die Windows Powershell-Konsole direkter nachbildet.

**Befehlszeilenschalter**. Mehrere neue Befehlszeilenschalter bieten Ihnen mehr Kontrolle über die Funktionsweise der ISE. „-NoProfile“ startet die ISE ohne Ausführung eines Profilskripts. „-Help“ öffnet ein Hilfefenster mit der ISE. „-mta“ startet die ISE im „Multithread-Apartment-Modus“. Der Standardwert ist „Singlethread“.

**Neue Features im Editor** erleichtern das Erstellen und Lesen Ihres Codes:

- **XML-Syntaxfarben**. Im ISE-Editor wird die XML-Syntax ebenso wie die Windows PowerShell-Codesyntax mit Farben versehen.

- **Zugehörige Klammer**. Die Windows PowerShell ISE markiert zueinanderpassende Klammern, um sicherzustellen, dass Sie über die richtige Anzahl schließender Klammern entsprechend den geöffneten verfügen. Drücken Sie STRG-\[, um die schließende Klammer zu finden, die zur öffnenden Klammer gehört, bei der sich der Cursor befindet.

- **Gliederungsansicht**. Sie können Abschnitte in Ihrem Code auf- und zuklappen, indem Sie am linken Rand auf das Plus- oder Minuszeichen klicken. Dies erleichtert das Auffinden des gesuchten Codes in einem langen Skript.

- **Textbearbeitung per Drag & Drop**. Sie können einen Textblock auswählen und ihn an eine andere Stelle ziehen, um ihn zu verschieben. Wenn Sie die STRG-Taste gedrückt halten, während Sie den markierten Text ziehen, wird der Text kopiert und nicht verschoben.

- **Anzeige von Analysefehlern**. Windows PowerShell untersucht Ihr Skript während der Eingabe. Wenn einen Fehler erkannt wird, wird eine rote Wellenlinie unter den fehlerhaften Code angezeigt. Wenn Sie auf den angezeigten Fehler zeigen, informiert eine QuickInfo über das gefundene Problem.

- **Zoom**. Sie können Text vergrößern, um ihn leichter lesen zu können, oder verkleinern, um sich ein besseres Bild zu verschaffen, indem Sie rechts unten im ISE-Fenster den Schieberegler bewegen.

- **Umfassende Funktionen zum Kopieren und Einfügen von Text**. Wenn Sie Code aus der ISE in die Zwischenablage kopieren, werden die Schriftart-, Größen und Farbinformationen des markierten Texts eingeschlossen.

- **Blockauswahl**. Sie können einen Textblock auswählen, indem Sie die ALT-TASTE gedrückt halten, während Sie den Text im Skriptbereich mit der Maus auswählen, oder **ALT+UMSCHALT+NACH-OBEN/NACH-UNTEN** verwenden.

### <a name="added-in-powershell-20-windows-server-2008-r2-windows-7"></a>In PowerShell 2.0 (Windows Server 2008 R2, Windows 7) hinzugefügt

Die ISE wurde mit PowerShell 2.0 eingeführt.

## <a name="requirements-for-running-the-windows-powershell-ise"></a>Anforderungen für die Ausführung der Windows PowerShell ISE

Die ISE ist auf allen Windows-Computern verfügbar, auf denen Windows PowerShell 2.0 oder höher ausgeführt werden kann. Alle Versionen von Windows und Windows Server enthalten eine Version von Windows PowerShell und der ISE. Durch die Installation von Windows Management Framework (WMF) können Sie jedoch ein Upgrade auf die neueste verfügbare Version vornehmen. Weitere Informationen finden Sie in der [WMF](/powershell/wmf)-Dokumentation.

> [!NOTE]
> Da die Windows PowerShell ISE eine grafische Benutzeroberfläche erfordert, können Sie sie nicht mit der Server Core-Option von Windows Server ausführen.

## <a name="see-also"></a>Siehe auch

[Zweck des Windows PowerShell ISE-Skriptobjektmodells](../../core-powershell/ise/Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)