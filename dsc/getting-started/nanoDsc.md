---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: Verwenden von DSC auf Nano Server
ms.openlocfilehash: ac5eaf3885788f40e12e4f0a0f19025668280f7e
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62079726"
---
# <a name="using-dsc-on-nano-server"></a>Verwenden von DSC auf Nano Server

> Gilt für: Windows PowerShell 5.0

**DSC unter Nano Server** ist ein optionales Paket im Ordner `NanoServer\Packages` des Windows Server 2016-Mediums. Das Paket kann installiert werden, wenn Sie eine VHD (virtuelle Festplatte) für einen Nano Server erstellen, indem Sie **Microsoft-NanoServer-DSC-Package** als Wert des Parameters **Packages** der Funktion **New-NanoServerImage** angeben. Wenn Sie beispielsweise eine virtuelle Festplatte für einen virtuellen Computer erstellen, würde der Befehl wie folgt aussehen:

```powershell
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1 -Packages Microsoft-NanoServer-DSC-Package
```

Informationen zum Installieren und Verwenden von Nano Server sowie zum Verwalten von Nano Server mit PowerShell-Remoting finden Sie unter [Erste Schritte mit Nano Server](/windows-server/get-started/getting-started-with-nano-server).

## <a name="dsc-features-available-on-nano-server"></a>Unter Nano Server verfügbare DSC-Funktionen

Da Nano Server im Vergleich mit einer Vollversion von Windows Server nur einen eingeschränkten Satz von APIs unterstützt, besitzt DSC zurzeit unter Nano Server nicht denselben, vollständigen Funktionsumfang wie DSC, das auf vollständigen SKUs ausgeführt wird. DSC unter Nano Server befindet sich in der aktiven Entwicklung, und der Funktionsumfang ist noch nicht vollständig.

Die folgenden DSC-Funktionen sind zurzeit unter Nano Server verfügbar:

Sowohl Push- als auch Pull-Modus

- Alle DSC-Cmdlets, die eine Vollversion von Windows Server beinhaltet, einschließlich der folgenden:
- [Get-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager)
- [Set-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager)
- [Enable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Enable-DscDebug)
- [Disable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Disable-DscDebug)
- [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration)
- [Stop-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Stop-DscConfiguration)
- [Get-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Get-DscConfiguration)
- [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration)
- [Publish-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Publish-DscConfiguration)
- [Update-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Update-DscConfiguration)
- [Restore-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Restore-DscConfiguration)
- [Remove-DscConfigurationDocument](/powershell/module/PSDesiredStateConfiguration/Remove-DscConfigurationDocument)
- [Get-DscConfigurationStatus](/powershell/module/PSDesiredStateConfiguration/Get-DscConfigurationStatus)
- [Invoke-DscResource](/powershell/module/PSDesiredStateConfiguration/Invoke-DscResource)
- [Find-DscResource](https://technet.microsoft.com/en-us/library/mt517874.aspx)
- [Get-DscResource](/powershell/module/PSDesiredStateConfiguration/Get-DscResource)
- [New-DscChecksum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum)

- Kompilieren von Konfigurationen (siehe [DSC-Konfigurationen](../configurations/configurations.md))

  **Problem:** Die Kennwortverschlüsselung (siehe [Securing the MOF File (Schützen der MOF-Datei)](../pull-server/secureMOF.md)) während der Konfigurationskompilierung funktioniert nicht.

- Kompilieren von Metakonfigurationen (siehe [Konfigurieren des lokalen Konfigurations-Managers](../managing-nodes/metaConfig.md)

- Ausführen einer Ressource unter dem Benutzerkontext (siehe [Ausführen von DSC mit Benutzeranmeldeinformationen („Ausführen als“, RunAs)](../configurations/runAsUser.md))

- Klassenbasierte Ressourcen (siehe [Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen](../resources/authoringResourceClass.md))

- Debuggen von DSC-Ressourcen (siehe [Debuggen von DSC-Ressourcen](../troubleshooting/debugResource.md))

  **Problem:** Funktioniert nicht, wenn eine Ressource die PsDscRunAsCredential-Eigenschaft verwendet (siehe [Running DSC with user credentials (Ausführen von DSC-Ressourcen mit Anmeldeinformationen)](../configurations/runAsUser.md))

- [Angeben knotenübergreifender Abhängigkeiten](../configurations/crossNodeDependencies.md)

- [Ressourcenversionsverwaltung](../configurations/sxsResource.md)

- Pullclient (Konfigurationen und Ressourcen) (siehe [Einrichten eines Pullclients mithilfe von Konfigurationsnamen](../pull-server/pullClientConfigNames.md))

- [Teilkonfigurationen (Pull und Push)](../pull-server/partialConfigs.md)

- [Berichterstattung an Pullserver](../pull-server/reportServer.md)

- MOF-Verschlüsselung

- Protokollierung

- Azure Automation-DSC-Berichte

- Ressourcen, die voll funktionsfähig sind

- **Archive**
- **Environment**
- **File**
- **Log**
- **ProcessSet**
- **Registry**
- **Script**
- **WindowsPackageCab**
- **WindowsProcess**
- **WaitForAll** (siehe [Angeben knotenübergreifender Abhängigkeiten](../configurations/crossNodeDependencies.md))
- **WaitForAny** (siehe [Angeben knotenübergreifender Abhängigkeiten](../configurations/crossNodeDependencies.md))
- **WaitForSome** (siehe [Angeben knotenübergreifender Abhängigkeiten](../configurations/crossNodeDependencies.md))

- Ressourcen, die teilweise funktionsfähig sind
- **Gruppe**
- **GroupSet**

  **Problem:** Bei den oben genannten Ressourcen tritt ein Fehler auf, wenn eine bestimmte Instanz zweimal aufgerufen wird (bzw. wenn die gleiche Konfiguration zweimal ausgeführt wird)

- **Service**
- **ServiceSet**

  **Problem:** Funktioniert nur beim Starten/Beenden (Status) des Diensts. Schlägt fehl, wenn versucht wird, andere Attribute des Diensts zu ändern, z. B. Starttyp, Anmeldeinformationen, Beschreibung usw. Ein Fehler mit etwa folgendem Wortlaut wird ausgelöst:

  *Typ [management.managementobject] wurde nicht gefunden: Stellen Sie sicher, dass die Assembly, die diesen Typ enthält, geladen wird.*

- Ressourcen, die nicht funktionsfähig sind
- **User**

## <a name="dsc-features-not-available-on-nano-server"></a>Unter Nano Server nicht verfügbare DSC-Funktionen

Die folgenden DSC-Funktionen sind zurzeit unter Nano Server nicht verfügbar:

- Entschlüsseln von MOF-Dokumenten mit verschlüsselten Kennwörtern
- Pullserver: Sie können zurzeit keinen Pullserver unter Nano Server einrichten.
- Alle Funktionen, die nicht in der Liste der Funktionen aufgeführt sind, funktionieren.

## <a name="using-custom-dsc-resources-on-nano-server"></a>Verwenden benutzerdefinierter DSC-Ressourcen unter Nano Server

Aufgrund eingeschränkter Sätze von Windows-APIs und CLR-Bibliotheken, die unter Nano Server verfügbar sind, funktionieren DSC-Ressourcen, die in der vollständigen CLR-Version von Windows funktionieren, nicht notwendigerweise auch unter Nano Server.
Führen Sie zuerst End-to-End-Tests durch, bevor Sie benutzerdefinierte DSC-Ressourcen in einer Produktionsumgebung bereitstellen.

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Nano Server](/windows-server/get-started/getting-started-with-nano-server)
