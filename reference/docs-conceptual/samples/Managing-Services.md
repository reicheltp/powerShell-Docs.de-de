---
ms.date: 06/05/2017
keywords: powershell,cmdlet
title: Verwalten von Diensten
ms.assetid: 7a410e4d-514b-4813-ba0c-0d8cef88df31
ms.openlocfilehash: 81fd8802215da80ce22fa3fd4750b1df6efe8206
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62086186"
---
# <a name="managing-services"></a>Verwalten von Diensten

Es gibt acht dienstbezogene Kern-Cmdlets („Service“-Cmdlets), die für eine Vielzahl von Dienstaufgaben konzipiert sind. Hier geht es nur um das Auflisten von Diensten und das Ändern des Ausführungsstatus für Dienste. Sie können aber eine Liste der Service-Cmdlets abrufen, indem Sie `Get-Help \*-Service` verwenden, und Sie finden Informationen zu jedem Service-Cmdlet, indem Sie `Get-Help <Cmdlet-Name>` verwenden (z.B. `Get-Help New-Service`).

## <a name="getting-services"></a>Abrufen von Diensten

Sie können die Dienste auf einem lokalen oder Remotecomputer abrufen, indem Sie das Cmdlet `Get-Service` verwenden. Wie bei `Get-Process` werden bei der Verwendung des `Get-Service`-Befehls ohne Parameter alle Dienste zurückgegeben. Sie können nach Name filtern, und Sie können sogar ein Sternchen als Platzhalterzeichen verwenden:

```powershell
PS> Get-Service -Name se*

Status   Name               DisplayName
------   ----               -----------
Running  seclogon           Secondary Logon
Running  SENS               System Event Notification
Stopped  ServiceLayer       ServiceLayer
```

Weil nicht immer offensichtlich ist, welcher Name der echte Name für einen Dienst ist, möchten Sie möglicherweise über den Anzeigenamen nach Diensten suchen. Dazu können Sie nach einem bestimmten Namen, mit Platzhaltern oder mit einer Liste von Anzeigenamen suchen:

```powershell
PS> Get-Service -DisplayName se*

Status   Name               DisplayName
------   ----               -----------
Running  lanmanserver       Server
Running  SamSs              Security Accounts Manager
Running  seclogon           Secondary Logon
Stopped  ServiceLayer       ServiceLayer
Running  wscsvc             Security Center

PS> Get-Service -DisplayName ServiceLayer,Server

Status   Name               DisplayName
------   ----               -----------
Running  lanmanserver       Server
Stopped  ServiceLayer       ServiceLayer
```

Sie können den „ComputerName“-Parameter des „Get-Service“-Cmdlets verwenden, um die Dienste auf Remotecomputern abzurufen. Der „ComputerName“-Parameter akzeptiert mehrere Werte sowie Platzhalterzeichen, sodass Sie die Dienste auf mehreren Computern mit einem einzigen Befehl abrufen können. Beispielsweise ruft der folgende Befehl die Dienste auf dem Remotecomputer „Server01“ ab.

```powershell
Get-Service -ComputerName Server01
```

## <a name="getting-required-and-dependent-services"></a>Abrufen von erforderlichen und abhängigen Diensten

Das „Get-Service“-Cmdlet hat zwei Parameter, die bei der Verwaltung von Diensten sehr hilfreich sind. Der „DependentServices“-Parameter bewirkt, dass Dienste abgerufen werden, die vom angegebenen Dienst abhängen. Der „RequiredServices“-Parameter bewirkt, dass Dienste abgerufen, von denen der angegebene Dienst abhängt.

Diese Parameter bewirken lediglich, dass die Werte der Eigenschaften „DependentServices und „ServicesDependedOn“ (Alias = „RequiredServices“) des von „Get-Service“ zurückgegebenen „System.ServiceProcess.ServiceController“-Objekts angezeigt werden, aber sie vereinfachen Befehle und machen das Abrufen dieser Informationen viel einfacher.

Der folgende Befehl ruft die Dienste ab, die für den „LanmanWorkstation“-Dienst erforderlich sind.

```powershell
PS> Get-Service -Name LanmanWorkstation -RequiredServices

Status   Name               DisplayName
------   ----               -----------
Running  MRxSmb20           SMB 2.0 MiniRedirector
Running  bowser             Bowser
Running  MRxSmb10           SMB 1.x MiniRedirector
Running  NSI                Network Store Interface Service
```

Der folgende Befehl ruft die Dienste ab, für die der „LanmanWorkstation“-Dienst erforderlich ist.

```powershell
PS> Get-Service -Name LanmanWorkstation -DependentServices

Status   Name               DisplayName
------   ----               -----------
Running  SessionEnv         Terminal Services Configuration
Running  Netlogon           Netlogon
Stopped  Browser            Computer Browser
Running  BITS               Background Intelligent Transfer Ser...
```

Sie können sogar alle Dienste abrufen, für die es Abhängigkeiten gibt. Im folgenden Befehl wird genau das getan und anschließend das Cmdlet „Format-Table“ verwendet, um für die Dienste auf dem Computer die Eigenschaften „Status“, „Name“, „RequiredServices“ und „DependentServices“ anzuzeigen.

```powershell
Get-Service -Name * | Where-Object {$_.RequiredServices -or $_.DependentServices} | Format-Table -Property Status, Name, RequiredServices, DependentServices -auto
```

## <a name="stopping-starting-suspending-and-restarting-services"></a>Beenden, Starten, Anhalten und Neustarten von Diensten

Die „Service“-Cmdlets die alle haben dieselbe allgemeine Form. Dienste können als allgemeine Namen oder Anzeigenamen angegeben werden, und es können Listen sowie Platzhalter als Werte angegeben werden. Um den Druckspooler zu beenden, verwenden Sie folgenden Befehl:

```powershell
Stop-Service -Name spooler
```

Um den Druckspooler zu starten, nachdem er beendet wurde, verwenden Sie folgenden Befehl:

```powershell
Start-Service -Name spooler
```

Um den Druckspooler anzuhalten, verwenden Sie folgenden Befehl:

```powershell
Suspend-Service -Name spooler
```

Das `Restart-Service`-Cmdlet funktioniert in gleicher Weise wie die anderen Service-Cmdlets, aber es werden einige komplexere Beispiele für dieses Cmdlet erläutert. Bei der einfachsten Verwendung geben Sie den Namen des Diensts an:

```powershell
PS> Restart-Service -Name spooler

WARNING: Waiting for service 'Print Spooler (Spooler)' to finish starting...
WARNING: Waiting for service 'Print Spooler (Spooler)' to finish starting...
PS>
```

Sie werden feststellen, dass eine wiederholte Warnmeldung zum Startvorgang des Druckspoolers angezeigt wird. Wenn Sie einen Dienstvorgang ausführen, der einige Zeit dauert, werden Sie von Windows PowerShell benachrichtigt, dass weiterhin versucht wird, die Aufgabe auszuführen.

Wenn Sie mehrere Dienste neu starten möchten, können Sie eine Liste der Dienste abrufen, diese Liste filtern und dann den Neustart ausführen:

```powershell
PS> Get-Service | Where-Object -FilterScript {$_.CanStop} | Restart-Service

WARNING: Waiting for service 'Computer Browser (Browser)' to finish stopping...
WARNING: Waiting for service 'Computer Browser (Browser)' to finish stopping...
Restart-Service : Cannot stop service 'Logical Disk Manager (dmserver)' because
 it has dependent services. It can only be stopped if the Force flag is set.
At line:1 char:57
+ Get-Service | Where-Object -FilterScript {$_.CanStop} | Restart-Service <<<<
WARNING: Waiting for service 'Print Spooler (Spooler)' to finish starting...
WARNING: Waiting for service 'Print Spooler (Spooler)' to finish starting...
```

Diese „Service“-Cmdlets haben keinen „ComputerName“-Parameter, Sie können sie aber auf einem Remotecomputer ausführen, indem Sie das „Invoke-Command“-Cmdlet verwenden. Beispielsweise startet der folgende Befehl den „Spooler“-Dienst auf dem Remotecomputer „Server01“ neu.

```powershell
Invoke-Command -ComputerName Server01 {Restart-Service Spooler}
```

## <a name="setting-service-properties"></a>Festlegen von Diensteigenschaften

Das Cmdlet „`Set-Service`“ ändert die Eigenschaften eines Diensts auf einem lokalen Computer oder Remotecomputer. Weil der Dienststatus eine Eigenschaft ist, können Sie dieses Cmdlet verwenden, um einen Dienst zu starten, zu beenden und anzuhalten.
Das Cmdlet „Set-Service“ hat außerdem einen „StartupType“-Parameter, über den Sie den Starttyp des Diensts ändern können.

Wenn Sie „`Set-Service`“ unter Windows Vista und höheren Versionen von Windows verwenden möchten, öffnen Sie Windows PowerShell mit der Option „Als Administrator ausführen“.

Weitere Informationen hierzu finden Sie unter [Set-Service [m2]](https://technet.microsoft.com/library/b71e29ed-372b-4e32-a4b7-5eb6216e56c3).

## <a name="see-also"></a>Weitere Informationen

- [Get-Service [m2]](https://technet.microsoft.com/en-us/library/0a09cb22-0a1c-4a79-9851-4e53075f9cf6)
- [Set-Service [m2]](https://technet.microsoft.com/library/b71e29ed-372b-4e32-a4b7-5eb6216e56c3)
- [Restart-Service [m2]](https://technet.microsoft.com/en-us/library/45acf50d-2277-4523-baf7-ce7ced977d0f)
- [Suspend-Service [m2]](https://technet.microsoft.com/en-us/library/c8492b87-0e21-4faf-8054-3c83c2ec2826)