---
title: Runspace02 (C#) Codebeispiel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 59a7b8b9-f72e-43fd-9a10-77404441a3f2
caps.latest.revision: 6
ms.openlocfilehash: 0a8fc05db74529e2bfb88820b9cfd988245e0aeb
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2019
ms.locfileid: "56854616"
---
# <a name="runspace02-c-code-sample"></a><span data-ttu-id="1f8c5-102">Runspace02-Codebeispiel (C#)</span><span class="sxs-lookup"><span data-stu-id="1f8c5-102">Runspace02 (C#) Code Sample</span></span>

<span data-ttu-id="1f8c5-103">Hier ist die C# Quellcode für das Beispiel Runspace02.</span><span class="sxs-lookup"><span data-stu-id="1f8c5-103">Here is the C# source code for the Runspace02 sample.</span></span> <span data-ttu-id="1f8c5-104">Dieses Beispiel verwendet die [System.Management.Automation.Runspaceinvoke](/dotnet/api/System.Management.Automation.RunspaceInvoke) Klasse zum Ausführen der `Get-Process` Cmdlet synchron.</span><span class="sxs-lookup"><span data-stu-id="1f8c5-104">This sample uses the [System.Management.Automation.Runspaceinvoke](/dotnet/api/System.Management.Automation.RunspaceInvoke) class to execute the `Get-Process` cmdlet synchronously.</span></span> <span data-ttu-id="1f8c5-105">Windows Forms und Datenbindung werden dann zum Anzeigen der Ergebnisse in einem DataGridView-Steuerelement</span><span class="sxs-lookup"><span data-stu-id="1f8c5-105">Windows Forms and data binding are then used to display the results in a DataGridView control</span></span>

## <a name="code-sample"></a><span data-ttu-id="1f8c5-106">Codebeispiel</span><span class="sxs-lookup"><span data-stu-id="1f8c5-106">Code Sample</span></span>

[!code-csharp[Runspace02.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace02/Runspace02.cs#L11-L82 "Runspace02.cs")]

## <a name="see-also"></a><span data-ttu-id="1f8c5-107">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="1f8c5-107">See Also</span></span>

[<span data-ttu-id="1f8c5-108">Windows PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="1f8c5-108">Windows PowerShell SDK</span></span>](../windows-powershell-reference.md)