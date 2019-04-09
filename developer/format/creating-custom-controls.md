---
title: Erstellen von benutzerdefinierten Steuerelementen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: c3baa406-cd33-4420-be5a-07ef09d93480
caps.latest.revision: 8
ms.openlocfilehash: 3504ab1d974c55e9279172d0e851961474ccb926
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2019
ms.locfileid: "56853196"
---
# <a name="creating-custom-controls"></a><span data-ttu-id="ad8ac-102">Erstellen von benutzerdefinierten Steuerelementen</span><span class="sxs-lookup"><span data-stu-id="ad8ac-102">Creating Custom Controls</span></span>

<span data-ttu-id="ad8ac-103">Benutzerdefinierte Steuerelemente sind der flexibelsten Komponenten eine Formatierungsdatei.</span><span class="sxs-lookup"><span data-stu-id="ad8ac-103">Custom controls are the most flexible components of a formatting file.</span></span> <span data-ttu-id="ad8ac-104">Im Gegensatz zu der Tabelle, Liste und Breiten Ansichten, die definieren, eine formale Struktur der Daten, z. B. eine Tabelle mit Daten, können benutzerdefinierte Steuerelemente definieren, wie eine einzelne Dateneinheit angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ad8ac-104">Unlike table, list, and wide views that define a formal structure of data, such as a table of data, custom controls allow you to define how an individual piece of data is displayed.</span></span> <span data-ttu-id="ad8ac-105">Sie können definieren, einen gemeinsamen Satz von benutzerdefinierten Steuerelementen, die auf die Formatierungsdatei für alle Ansichten verfügbar sind, können Sie definieren, benutzerdefinierte Steuerelemente, die mit einer bestimmten Ansicht verfügbar sind oder Sie können definieren, einen Satz von Steuerelementen, die eine Gruppe von Objekten zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="ad8ac-105">You can define a common set of custom controls that are available to all the views of the formatting file, you can define custom controls that are available to a specific view, or you can define a set of controls that are available to a group of objects.</span></span>

## <a name="custom-control-example"></a><span data-ttu-id="ad8ac-106">Beispiel für ein benutzerdefiniertes Steuerelement</span><span class="sxs-lookup"><span data-stu-id="ad8ac-106">Custom Control Example</span></span>

<span data-ttu-id="ad8ac-107">Das folgende Beispiel zeigt ein benutzerdefiniertes Steuerelement, das in der Datei Certificates.Format.ps1xml definiert ist.</span><span class="sxs-lookup"><span data-stu-id="ad8ac-107">The following example shows a custom control that is defined in the Certificates.Format.ps1xml file.</span></span> <span data-ttu-id="ad8ac-108">Dieses benutzerdefinierte Steuerelement dient zum Trennen der [System.Management.Automation.Signature](/dotnet/api/System.Management.Automation.Signature) Objekte, die in einer Tabellenansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ad8ac-108">This custom control is used to separate the [System.Management.Automation.Signature](/dotnet/api/System.Management.Automation.Signature) objects displayed in a table view.</span></span>

```xml
<Controls>
  <Control>
    <Name>SignatureTypes-GroupingFormat</Name>
    <CustomControl>
      <CustomEntries>
        <CustomEntry>
          <CustomItem>
            <Frame>
              <LeftIndent>4</LeftIndent>
              <CustomItem>
                <Text AssemblyName="System.Management.Automation" BaseName="FileSystemProviderStrings"
                  ResourceId="DirectoryDisplayGrouping"/>
                <ExpressionBinding>
                  <ScriptBlock>split-path $_.Path</ScriptBlock>
                </ExpressionBinding>
                <NewLine/>
              </CustomItem>
            </Frame>
          </CustomItem>
        </CustomEntry>
      </CustomEntries>
    </CustomControl>
  </Control>
</Controls>

```

## <a name="see-also"></a><span data-ttu-id="ad8ac-109">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="ad8ac-109">See Also</span></span>

[<span data-ttu-id="ad8ac-110">Schreiben Sie eine Formatierungsdatei PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad8ac-110">Writing a PowerShell Formatting File</span></span>](./writing-a-powershell-formatting-file.md)