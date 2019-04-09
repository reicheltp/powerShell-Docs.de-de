---
title: Beispiel für Runspace10 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 7c265084-e072-46ca-9844-c3c0e275d6b0
caps.latest.revision: 7
ms.openlocfilehash: fdf0036c68b608d254ed928ae9ac58616a856200
ms.sourcegitcommit: caac7d098a448232304c9d6728e7340ec7517a71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2019
ms.locfileid: "58059506"
---
# <a name="runspace10-sample"></a><span data-ttu-id="7921e-102">Runspace10-Beispiel</span><span class="sxs-lookup"><span data-stu-id="7921e-102">Runspace10 Sample</span></span>

<span data-ttu-id="7921e-103">In diesem Beispiel wird gezeigt, wie Sie einen anfänglichen Standardsitzungsstatus erstellen wie ein Cmdlet zum Hinzufügen der [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState), wie Sie einen Runspace erstellen, die den anfänglichen Sitzungsstatus verwendet und ausführen der Befehl mit einem [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-103">This sample shows how to create a default initial session state, how to add a cmdlet to the [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState), how to create a runspace that uses the initial session state, and how to run the command by using a [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) object.</span></span>

## <a name="requirements"></a><span data-ttu-id="7921e-104">Anforderungen</span><span class="sxs-lookup"><span data-stu-id="7921e-104">Requirements</span></span>

<span data-ttu-id="7921e-105">Dieses Beispiel ist die Windows PowerShell 2.0 erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7921e-105">This sample requires Windows PowerShell 2.0.</span></span>

## <a name="demonstrates"></a><span data-ttu-id="7921e-106">Veranschaulicht</span><span class="sxs-lookup"><span data-stu-id="7921e-106">Demonstrates</span></span>

<span data-ttu-id="7921e-107">Dieses Beispiel veranschaulicht die folgenden.</span><span class="sxs-lookup"><span data-stu-id="7921e-107">This sample demonstrates the following.</span></span>

- <span data-ttu-id="7921e-108">Erstellen einer [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-108">Creating an [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) object.</span></span>

- <span data-ttu-id="7921e-109">Hinzufügen eines Cmdlets (von der hostanwendung definiert) die [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-109">Adding a cmdlet (defined by the Host application) to the [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) object.</span></span>

- <span data-ttu-id="7921e-110">Erstellen einer [System.Management.Automation.Runspaces.Runspace](/dotnet/api/System.Management.Automation.Runspaces.Runspace) -Objekt, das das Objekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="7921e-110">Creating a [System.Management.Automation.Runspaces.Runspace](/dotnet/api/System.Management.Automation.Runspaces.Runspace) object that uses the object.</span></span>

- <span data-ttu-id="7921e-111">Erstellen einer [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) , verwendet der [System.Management.Automation.Runspaces.Runspace](/dotnet/api/System.Management.Automation.Runspaces.Runspace) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-111">Creating a [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) object that uses the [System.Management.Automation.Runspaces.Runspace](/dotnet/api/System.Management.Automation.Runspaces.Runspace) object.</span></span>

- <span data-ttu-id="7921e-112">Der Befehl hinzufügen, der Pipeline die [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-112">Adding the command to the pipeline of the [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) object.</span></span>

- <span data-ttu-id="7921e-113">Extrahieren von Eigenschaften aus der [System.Management.Automation.PSObject](/dotnet/api/System.Management.Automation.PSObject) Objekte, die vom Befehl zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="7921e-113">Extracting properties from the [System.Management.Automation.PSObject](/dotnet/api/System.Management.Automation.PSObject) objects returned by the command.</span></span>

## <a name="example"></a><span data-ttu-id="7921e-114">Beispiel</span><span class="sxs-lookup"><span data-stu-id="7921e-114">Example</span></span>

<span data-ttu-id="7921e-115">Dieses Beispiel erstellt einen Runspace, die verwendet eine [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) Objekt, das die Elemente zu definieren, die verfügbar sind, wenn der Runspace geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="7921e-115">This sample creates a runspace that uses an [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) object to define the elements that are available when the runspace is opened.</span></span> <span data-ttu-id="7921e-116">In diesem Beispiel wird das Get-Proc-Cmdlet (definiert durch die hostanwendung) den anfänglichen Sitzungsstatus hinzugefügt, und das Cmdlet wird synchron ausgeführt, mit einem [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) Objekt.</span><span class="sxs-lookup"><span data-stu-id="7921e-116">In this sample, the Get-Proc cmdlet (defined by the Host application) is added to the initial session state, and the cmdlet is run synchronously by using a [System.Management.Automation.Powershell](/dotnet/api/system.management.automation.powershell) object.</span></span>

```csharp
namespace Microsoft.Samples.PowerShell.Runspaces
{
  using System;
  using System.Collections.Generic;
  using System.Collections.ObjectModel;
  using System.Diagnostics;
  using System.Management.Automation;
  using System.Management.Automation.Runspaces;
  using PowerShell = System.Management.Automation.PowerShell;

  #region GetProcCommand

  /// <summary>
  /// Class that implements the GetProcCommand.
  /// </summary>
  [Cmdlet(VerbsCommon.Get, "Proc")]
  public class GetProcCommand : Cmdlet
  {
    #region Cmdlet Overrides

    /// <summary>
    /// For each of the requested process names, retrieve and write
    /// the associated processes.
    /// </summary>
    protected override void ProcessRecord()
    {
      // Get the current processes.
      Process[] processes = Process.GetProcesses();

      // Write the processes to the pipeline making them available
      // to the next cmdlet. The second argument (true) tells the
      // system to enumerate the array, and send one process object
      // at a time to the pipeline.
      WriteObject(processes, true);
    }

    #endregion Overrides
  } // End GetProcCommand class.

  #endregion GetProcCommand

  /// <summary>
  /// This class contains the Main entry point for this host application.
  /// </summary>
  internal class Runspace10
  {
    /// <summary>
    /// This sample shows how to create a default initial session state, how to add
    /// add a cmdlet to the InitialSessionState object, and then how to create
    /// a Runspace object.
    /// </summary>
    /// <param name="args">Parameter is not used.</param>
    /// This sample demonstrates:
    /// 1. Creating an InitialSessionState object.
    /// 2. Adding a cmdlet to the InitialSessionState object.
    /// 3. Creating a runspace that uses the InitialSessionState object.
    /// 4. Creating a PowerShell object that uses the Runspace object.
    /// 5. Running the added command synchronously.
    /// 6. Working with PSObject objects to extract properties
    ///    from the objects returned by the pipeline.
    private static void Main(string[] args)
    {
      // Create a default InitialSessionState object. The default
      // InitialSessionState object contains all the elements provided
      // by Windows PowerShell.
      InitialSessionState iss = InitialSessionState.CreateDefault();

      // Add the get-proc cmdlet to the InitialSessionState object.
      SessionStateCmdletEntry ssce = new SessionStateCmdletEntry("get-proc", typeof(GetProcCommand), null);
      iss.Commands.Add(ssce);

      // Create a Runspace object that uses the InitialSessionState object.
      // Notice that no PSHost object is specified, so the default host is used.
      // See the Hosting samples for information on creating your own custom host.
      using (Runspace myRunSpace = RunspaceFactory.CreateRunspace(iss))
      {
        myRunSpace.Open();

        using (PowerShell powershell = PowerShell.Create())
        {
          powershell.Runspace = myRunSpace;

          // Add the get-proc cmdlet to the pipeline of the PowerShell object.
          powershell.AddCommand("get-proc");

          Collection<PSObject> results = powershell.Invoke();

          Console.WriteLine("Process              HandleCount");
          Console.WriteLine("--------------------------------");

          // Display the output of the pipeline.
          foreach (PSObject result in results)
          {
             Console.WriteLine(
                               "{0,-20} {1}",
                               result.Members["ProcessName"].Value,
                               result.Members["HandleCount"].Value);
          }
        }

        // Close the runspace to release resources.
        myRunSpace.Close();
      }

      System.Console.WriteLine("Hit any key to exit...");
      System.Console.ReadKey();
    }
  }
}
```

## <a name="see-also"></a><span data-ttu-id="7921e-117">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="7921e-117">See Also</span></span>

[<span data-ttu-id="7921e-118">Schreiben Sie eine Windows PowerShell-Hostanwendung</span><span class="sxs-lookup"><span data-stu-id="7921e-118">Writing a Windows PowerShell Host Application</span></span>](./writing-a-windows-powershell-host-application.md)