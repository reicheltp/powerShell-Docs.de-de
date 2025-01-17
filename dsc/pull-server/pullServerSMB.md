---
ms.date: 04/11/2018
keywords: dsc,powershell,configuration,setup
title: Einrichten eines DSC-SMB-Pullservers
ms.openlocfilehash: 9d087a08861b2f4683e81efd1e25f857b8b75e07
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62079284"
---
# <a name="setting-up-a-dsc-smb-pull-server"></a>Einrichten eines DSC-SMB-Pullservers

Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

> [!IMPORTANT]
> Der Pull-Server (Windows-Feature *DSC-Dienst*) ist eine von Windows Server unterstützte Komponente, jedoch sollen keine neuen Features oder Funktionen angeboten werden. Es wird empfohlen, verwaltete Clients auf [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) umzustellen (enthält Features zusätzlich zum Pull-Server unter Windows Server) oder auf eine der [hier](pullserver.md#community-solutions-for-pull-service) aufgeführten Communitylösungen.

Ein DSC-[SMB](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11))-Pullserver ist ein Computer, der als Host einer SMB-Dateifreigabe fungiert, mit der DSC-Konfigurationsdateien und DSC-Ressourcen für Zielknoten zur Verfügung gestellt werden, wenn sie von diesen Knoten angefordert werden.

Zum Verwenden eines SMB-Pullservers für DSC müssen Sie folgende Schritte ausführen:

- Einrichten einer SMB-Dateifreigabe auf einem Server mit PowerShell 4.0 oder höher
- Konfigurieren eines Clients mit PowerShell 4.0 oder höher zum Abrufen von dieser SMB-Freigabe

## <a name="using-the-xsmbshare-resource-to-create-an-smb-file-share"></a>Verwenden der xSmbShare-Ressource zum Erstellen einer SMB-Dateifreigabe

Es gibt zahlreiche Methoden zum Einrichten einer SMB-Dateifreigabe. Im Folgenden wird die Methode unter Verwendung von DSC beschrieben.

### <a name="install-the-xsmbshare-resource"></a>Installieren der Ressource „xSmbShare“

Rufen Sie das Cmdlet [Install-Module](/powershell/module/PowershellGet/Install-Module) auf, um das Modul **xSmbShare** zu installieren.

> [!NOTE]
> `Install-Module` ist im Modul **PowerShellGet** enthalten, das Bestandteil von PowerShell 5.0 ist. Das Modul **PowerShellGet** für PowerShell 3.0 und 4.0 können Sie unter [PowerShell-Module „PackageManagement“ – Vorschau](https://www.microsoft.com/en-us/download/details.aspx?id=49186) herunterladen.
> **xSmbShare** enthält die DSC-Ressource **xSmbShare**, mit der Sie eine SMB-Dateifreigabe erstellen können.

### <a name="create-the-directory-and-file-share"></a>Erstellen des Verzeichnisses und der Dateifreigabe

Die folgende Konfiguration verwendet die **File**-Ressource zum Erstellen des Verzeichnisses für die Freigabe und die **xSmbShare**-Ressource zum Einrichten der SMB-Freigabe:

```powershell
Configuration SmbShare
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Import-DscResource -ModuleName xSmbShare

    Node localhost
    {

        File CreateFolder
        {
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
        }

        xSMBShare CreateShare
        {
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
        }
    }
}
```

Mit der Konfiguration wird das Verzeichnis `C:\DscSmbShare` erstellt, wenn es noch nicht vorhanden ist. Das Verzeichnis wird dann als SMB-Dateifreigabe verwendet. Die Berechtigung **FullAccess** sollte jedem Konto gewährt werden, das in die Dateifreigabe schreiben oder aus dieser löschen muss. Die Berechtigung **ReadAccess** muss allen Clientknoten gewährt werden, die Konfigurationen und bzw. oder DSC-Ressourcen von der Freigabe abrufen.

> [!NOTE]
> DSC wird standardmäßig als das Systemkonto ausgeführt, der Computer selbst muss also über Zugriff auf die Freigabe verfügen.

### <a name="give-file-system-access-to-the-pull-client"></a>Gewähren von Dateisystemzugriff für den Pullclient

Indem Sie einem Clientknoten **ReadAccess** gewähren, kann dieser Knoten auf die SMB-Freigabe zugreifen, aber nicht auf Dateien oder Ordner innerhalb der Freigabe. Sie müssen Clientknoten explizit Zugriff auf den SMB-Freigabeordner und dessen Unterordner gewähren. In DSC erfolgt dies mithilfe der Ressource **cNtfsPermissionEntry**, die im Modul [CNtfsAccessControl](https://www.powershellgallery.com/packages/cNtfsAccessControl/1.2.0) enthalten ist. Die folgende Konfiguration fügt einen **cNtfsPermissionEntry**-Block hinzu, der dem Pullclient „ReadAndExecute“-Zugriff gewährt:

```powershell
Configuration DSCSMB
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Import-DscResource -ModuleName xSmbShare
    Import-DscResource -ModuleName cNtfsAccessControl

    Node localhost
    {

        File CreateFolder
        {
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
        }

        xSMBShare CreateShare
        {
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
        }

        cNtfsPermissionEntry PermissionSet1
        {
            Ensure = 'Present'
            Path = 'C:\DscSmbShare'
            Principal = 'myDomain\Contoso-Server$'
            AccessControlInformation = @(
                cNtfsAccessControlInformation
                {
                    AccessControlType = 'Allow'
                    FileSystemRights = 'ReadAndExecute'
                    Inheritance = 'ThisFolderSubfoldersAndFiles'
                    NoPropagateInherit = $false
                }
            )
            DependsOn = '[File]CreateFolder'
        }
    }
}
```

## <a name="placing-configurations-and-resources"></a>Platzieren von Konfigurationen und Ressourcen

Speichern Sie alle MOF-Konfigurationsdateien und/oder DSC-Ressourcen, die von Clientknoten per Pull abgerufen werden sollen, im SMB-Freigabeordner.

Alle MOF-Konfigurationsdateien müssen den Namen *ConfigurationID*.mof haben, wobei *ConfigurationID* der Wert der **ConfigurationID**-Eigenschaft des LCM des Zielknotens ist. Weitere Informationen zum Einrichten von Pullclients finden Sie unter [Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md).

> [!NOTE]
> Sie müssen Konfigurations-IDs verwenden, wenn Sie einen SMB-Pullserver verwenden. Konfigurationsnamen werden für SMB nicht unterstützt.

Jedes Ressourcenmodul muss komprimiert und entsprechend dem folgenden Muster benannt werden: `{Module Name}_{Module Version}.zip`. Ein Modul namens „xWebAdminstration“ mit einer Modulversion 3.1.2.0 würde beispielsweise „xWebAdministration_3.2.1.0.zip“ heißen. Jede Version eines Moduls muss in einer eigenen ZIP-Datei enthalten sein. Separate Versionen eines Moduls in einer ZIP-Datei werden nicht unterstützt. Bevor Sie die DSC-Ressourcenmodule für die Verwendung mit einem Pullserver verpacken, müssen Sie eine kleine Änderung an der Verzeichnisstruktur vornehmen.

Das Standardformat für Module mit DSC-Ressourcen in WMF 5.0 ist `{Module Folder}\{Module Version}\DscResources\{DSC Resource Folder}\`.

Entfernen Sie vor dem Packvorgang für den Pullserver einfach den Ordner `{Module version}`, damit der Pfad zu `{Module Folder}\DscResources\{DSC Resource Folder}\` wird. Komprimieren Sie den Ordner nach dieser Änderung wie oben beschrieben, und speichern Sie die ZIP-Dateien im SMB-Freigabeordner.

## <a name="creating-the-mof-checksum"></a>Erstellen der MOF-Prüfsumme

Eine MOF-Konfigurationsdatei muss einer Prüfsummendatei zugeordnet werden, damit ein LCM auf einem Zielknoten die Konfiguration überprüfen kann.
Um eine Prüfsumme zu erstellen, rufen Sie das Cmdlet [New-DSCCheckSum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum) auf. Das Cmdlet verwendet einen `Path`-Parameter, der den Ordner angibt, in dem sich die MOF-Konfigurationsdatei befindet. Das Cmdlet erstellt eine Prüfsummendatei mit dem Namen `ConfigurationMOFName.mof.checksum`, wobei `ConfigurationMOFName` der Name der MOF-Konfigurationsdatei ist.
Wenn in dem angegebenen Ordner mehrere MOF-Konfigurationsdateien vorhanden sind, wird für jede Konfiguration im Ordner eine Prüfsumme erstellt.

Die Prüfsummendatei muss sich im gleichen Verzeichnis wie die MOF-Konfigurationsdatei befinden (standardmäßig `$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration`). Sie muss den gleichen Namen mit der Erweiterung `.checksum` haben.

> [!NOTE]
> Wenn Sie die MOF-Konfigurationsdatei in irgendeiner Weise ändern, müssen Sie auch die Prüfsummendatei neu erstellen.

## <a name="setting-up-a-pull-client-for-smb"></a>Einrichten eines Pullclients für SMB

Zum Einrichten eines Clients, der Konfigurationen und/oder der Ressourcen von einer SMB-Freigabe abruft, konfigurieren Sie den lokalen Konfigurations-Manager (Local Configuration Manager, LCM) des Clients mit den Codeblöcken **ConfigurationRepositoryShare** und **ResourceRepositoryShare**, die die Freigabe angeben, aus der Pullkonfigurationen und DSC-Ressourcen abgerufen werden sollen.

Weitere Informationen zum Konfigurieren des LCM finden Sie unter [Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md).

> [!NOTE]
> Der Einfachheit halber wird in diesem Beispiel **PSDscAllowPlainTextPassword** verwendet, um die Übergabe eines Nur-Text-Kennworts an den Parameter **Credential** zu ermöglichen. Informationen zur sicheren Weitergabe von Anmeldeinformationen finden Sie unter [Optionen für Anmeldeinformationen in den Konfigurationsdaten](../configurations/configDataCredentials.md).
>
> Sie **MÜSSEN** eine **ConfigurationID** im Codeblock **Settings** einer Metakonfiguration für einen SMB-Pullserver eingeben, selbst wenn Sie nur Ressourcen abrufen.

```powershell
$secpasswd = ConvertTo-SecureString “Pass1Word” -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential (“TestUser”, $secpasswd)

[DSCLocalConfigurationManager()]
configuration SmbCredTest
{
    Node $AllNodes.NodeName
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
            ConfigurationID    = '16db7357-9083-4806-a80c-ebbaf4acd6c1'
        }

         ConfigurationRepositoryShare SmbConfigShare
        {
            SourcePath = '\\WIN-E0TRU6U11B1\DscSmbShare'
            Credential = $mycreds
        }

        ResourceRepositoryShare SmbResourceShare
        {
            SourcePath = '\\WIN-E0TRU6U11B1\DscSmbShare'
            Credential = $mycreds

        }
    }
}

$ConfigurationData = @{
    AllNodes = @(
        @{
            #the "*" means "all nodes named in ConfigData" so we don't have to repeat ourselves
            NodeName="localhost"
            PSDscAllowPlainTextPassword = $true
        })
}
```

## <a name="acknowledgements"></a>Bestätigungen

Besonderer Dank gilt den folgenden Personen:

- Mike F. Robbins, dessen Beiträge zur Verwendung von SMB für DSC beim Zusammenstellen des Inhalts in diesem Thema geholfen haben. Seinen Blog finden Sie unter [Mike F Robbins](http://mikefrobbins.com/).
- Serge Nikalaichyk, der das Modul **cNtfsAccessControl** erstellt hat. Die Quelle für dieses Modul finden Sie unter [cNtfsAccessControl](https://github.com/SNikalaichyk/cNtfsAccessControl).

## <a name="see-also"></a>Siehe auch

[Windows PowerShell DSC – Übersicht](../overview/overview.md)

[Inkraftsetzung von Konfigurationen](enactingConfigurations.md)

[Einrichten eines Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md)
