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
ms.sourcegitcommit: 69abc5ad16e5dd29ddfb1853e266a4bfd1d59d59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57429634"
---
# <a name="runspace07-code-sample"></a><span data-ttu-id="9af60-102">RunSpace07-Codebeispiel</span><span class="sxs-lookup"><span data-stu-id="9af60-102">RunSpace07 Code Sample</span></span>

<span data-ttu-id="9af60-103">Hier ist der Quellcode, für das Beispiel Runspace07 in beschrieben [erstellen eine Anwendung, fügt Konsolenbefehle zu einer Pipeline](http://msdn.microsoft.com/en-us/01eb7808-e97b-4905-80be-9e2fa38c262e).</span><span class="sxs-lookup"><span data-stu-id="9af60-103">Here is the source code for the Runspace07 sample described in [Creating a Console Application That Adds Commands to a Pipeline](http://msdn.microsoft.com/en-us/01eb7808-e97b-4905-80be-9e2fa38c262e).</span></span> <span data-ttu-id="9af60-104">Diese beispielanwendung einen Runspace erstellt, wird eine Pipeline erstellt, fügt zwei Befehle an die Pipeline und führt dann die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="9af60-104">This sample application creates a runspace, creates a pipeline, adds two commands to the pipeline, and then executes the pipeline.</span></span> <span data-ttu-id="9af60-105">Die Befehle zur Pipeline hinzugefügte sind die `Get-Process` und `Measure-Object` Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="9af60-105">The commands added to the pipeline are the `Get-Process` and `Measure-Object` cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="9af60-106">Sie können die C# Quelldatei (runspace07.cs) mit dem Microsoft Windows Software Development Kit für Windows Vista und Microsoft .NET Framework 3.0-Laufzeitkomponenten.</span><span class="sxs-lookup"><span data-stu-id="9af60-106">You can download the C# source file (runspace07.cs) using the Microsoft Windows Software Development Kit for Windows Vista and Microsoft .NET Framework 3.0 Runtime Components.</span></span> <span data-ttu-id="9af60-107">Anweisungen zum Herunterladen, finden Sie unter [das Installieren von Windows PowerShell und das Windows PowerShell-SDK-Download](/powershell/developer/installing-the-windows-powershell-sdk).</span><span class="sxs-lookup"><span data-stu-id="9af60-107">For download instructions, see [How to Install Windows PowerShell and Download the Windows PowerShell SDK](/powershell/developer/installing-the-windows-powershell-sdk).</span></span>
>
> <span data-ttu-id="9af60-108">Die heruntergeladene Quelldateien stehen in der  **\<PowerShell-Beispiele >** Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="9af60-108">The downloaded source files are available in the **\<PowerShell Samples>** directory.</span></span>

## <a name="code-sample"></a><span data-ttu-id="9af60-109">Codebeispiel</span><span class="sxs-lookup"><span data-stu-id="9af60-109">Code Sample</span></span>

[!code-csharp[Runspace07.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace07/Runspace07.cs#L11-L108 "Runspace07.cs")]

## <a name="see-also"></a><span data-ttu-id="9af60-110">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="9af60-110">See Also</span></span>

[<span data-ttu-id="9af60-111">Windows PowerShell Handbuch für Programmierer</span><span class="sxs-lookup"><span data-stu-id="9af60-111">Windows PowerShell Programmer's Guide</span></span>](./windows-powershell-programmer-s-guide.md)

[<span data-ttu-id="9af60-112">Windows PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="9af60-112">Windows PowerShell SDK</span></span>](../windows-powershell-reference.md)