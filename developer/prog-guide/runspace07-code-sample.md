---
title: Codebeispiel RunSpace07 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 8ad306d9-45c2-4d55-8e64-fdcba43402c5
caps.latest.revision: 6
ms.openlocfilehash: 064e7d7ea2ee173bbcdd75a9f3a6c12582afe17b
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62081290"
---
# <a name="runspace07-code-sample"></a>RunSpace07-Codebeispiel

Hier ist der Quellcode, für das Beispiel Runspace07 in beschrieben [erstellen eine Anwendung, fügt Konsolenbefehle zu einer Pipeline](http://msdn.microsoft.com/en-us/01eb7808-e97b-4905-80be-9e2fa38c262e). Diese beispielanwendung einen Runspace erstellt, wird eine Pipeline erstellt, fügt zwei Befehle an die Pipeline und führt dann die Pipeline. Die Befehle zur Pipeline hinzugefügte sind die `Get-Process` und `Measure-Object` Cmdlets.

> [!NOTE]
> Sie können die C# Quelldatei (runspace07.cs) mit dem Microsoft Windows Software Development Kit für Windows Vista und Microsoft .NET Framework 3.0-Laufzeitkomponenten. Anweisungen zum Herunterladen, finden Sie unter [das Installieren von Windows PowerShell und das Windows PowerShell-SDK-Download](/powershell/developer/installing-the-windows-powershell-sdk).
>
> Die heruntergeladene Quelldateien stehen in der  **\<PowerShell-Beispiele >** Verzeichnis.

## <a name="code-sample"></a>Codebeispiel

[!code-csharp[Runspace07.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace07/Runspace07.cs#L11-L108 "Runspace07.cs")]

## <a name="see-also"></a>Weitere Informationen

[Windows PowerShell Handbuch für Programmierer](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)