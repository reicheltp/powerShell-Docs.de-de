---
title: Runspace01 (C#) Codebeispiel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: d59f8b7c-e800-4633-aa5b-74d4c57e2706
caps.latest.revision: 6
ms.openlocfilehash: 59320365c4a35c3d71af10273eb21b1ce01e5c0c
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62081460"
---
# <a name="runspace01-c-code-sample"></a>Runspace01-Codebeispiel (C#)

Hier sind die Codebeispiele, für der Runspace beschrieben [erstellen eine Konsole Einzelanwendung eines angegebenen Befehls](http://msdn.microsoft.com/en-us/793a6570-a072-4799-840b-172f28ce620e). Zu diesem Zweck die Anwendung einen Runspace aufruft, und ruft dann einen Befehl auf. (Beachten Sie, dass diese Anwendung gibt keinen Runspace-Konfigurationsinformationen oder wird explizit eine Pipeline erstellt.) Der Befehl, der aufgerufen wird, wird die `Get-Process` Cmdlet.

> [!NOTE]
> Sie können die C# Quelldatei (runspace01.cs) für diesen Runspace, mit dem Microsoft Windows Software Development Kit für Windows Vista und Microsoft .NET Framework 3.0-Laufzeitkomponenten. Anweisungen zum Herunterladen, finden Sie unter [das Installieren von Windows PowerShell und das Windows PowerShell-SDK-Download](/powershell/developer/installing-the-windows-powershell-sdk).
>
> Die heruntergeladene Quelldateien stehen in der  **\<PowerShell-Beispiele >** Verzeichnis.

## <a name="code-sample"></a>Codebeispiel

[!code-csharp[Runspace01.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace01/Runspace01.cs#L11-L62 "Runspace01.cs")]

## <a name="see-also"></a>Weitere Informationen

[Windows PowerShell Handbuch für Programmierer](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)