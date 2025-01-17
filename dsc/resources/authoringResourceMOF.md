---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: Schreiben einer benutzerdefinierten DSC-Ressource mit MOF
ms.openlocfilehash: f243c3e3297711e6f6346a0f813a9c017fe227c3
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076734"
---
# <a name="writing-a-custom-dsc-resource-with-mof"></a>Schreiben einer benutzerdefinierten DSC-Ressource mit MOF

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

In diesem Thema wird das Schema für eine benutzerdefinierte Windows PowerShell DSC-Ressource in einer MOF-Datei definiert und die Ressource in einer Windows PowerShell-Skriptdatei implementiert. Diese benutzerdefinierte Ressource dient zum Erstellen und Verwalten einer Website.

## <a name="creating-the-mof-schema"></a>Schreiben des MOF-Schemas

Das Schema definiert die Eigenschaften der Ressource, die durch ein DSC-Konfigurationsskript konfiguriert werden können.

### <a name="folder-structure-for-a-mof-resource"></a>Ordnerstruktur für eine MOF-Ressource

Um eine benutzerdefinierte DSC-Ressource mit einem MOF-Schema zu implementieren, erstellen Sie die folgende Ordnerstruktur. Das MOF-Schema ist in der Datei „Demo_IISWebsite.schema.mof“ definiert und das Ressourcenskript in „Demo_IISWebsite.psm1“. Optional können Sie eine Modulmanifestdatei (psd1) erstellen.

```
$env:ProgramFiles\WindowsPowerShell\Modules (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- Demo_IISWebsite (folder)
                |- Demo_IISWebsite.psd1 (file, optional)
                |- Demo_IISWebsite.psm1 (file, required)
                |- Demo_IISWebsite.schema.mof (file, required)
```

Beachten Sie, dass im Ordner der obersten Ebene ein Ordner mit dem Namen „DSCResources“ erstellt werden muss, und dass die Ordnernamen für die einzelnen Ressourcen den Namen der jeweiligen Ressourcen entsprechen müssen.

### <a name="the-contents-of-the-mof-file"></a>Inhalt der MOF-Datei

Die folgende MOF-Beispieldatei kann für eine benutzerdefinierte Websiteressource verwendet werden. Um dieses Beispiel anzuwenden, speichern Sie das Schema in einer Datei, und rufen Sie die Datei *Demo_IISWebsite.schema.mof* auf.

```
[ClassVersion("1.0.0"), FriendlyName("Website")]
class Demo_IISWebsite : OMI_BaseResource
{
  [Key] string Name;
  [Required] string PhysicalPath;
  [write,ValueMap{"Present", "Absent"},Values{"Present", "Absent"}] string Ensure;
  [write,ValueMap{"Started","Stopped"},Values{"Started", "Stopped"}] string State;
  [write] string Protocol[];
  [write] string BindingInfo[];
  [write] string ApplicationPool;
  [read] string ID;
};
```

Beachten Sie Folgendes im Zusammenhang mit dem vorherigen Code:

* `FriendlyName` definiert den Namen, den Sie verwenden können, um auf diese benutzerdefinierte Ressource in DSC-Konfigurationsskripts zu verweisen. In diesem Beispiel entspricht `Website` dem Anzeigenamen `Archive` für die integrierten Ressource „Archive“.
* Die Klasse, die Sie für die benutzerdefinierte Ressource definieren, muss von `OMI_BaseResource` abgeleitet sein.
* Der Typqualifizierer `[Key]` für eine Eigenschaft gibt an, dass die Ressourceninstanz durch diese Eigenschaft eindeutig identifiziert wird. Mindestens eine `[Key]`-Eigenschaft ist erforderlich.
* Der Qualifizierer `[Required]` gibt an, dass die Eigenschaft erforderlich ist (der Wert muss in jedem Konfigurationsskript, in dem die Ressource verwendet wird, angegeben werden).
* Der Qualifizierer `[write]` gibt an, dass diese Eigenschaft bei Verwendung der benutzerdefinierten Ressource in einem Konfigurationsskript optional ist. Der Qualifizierer `[read]` gibt an, dass eine Eigenschaft nicht durch eine Konfiguration festgelegt werden kann und nur zu Berichtszwecken dient.
* `Values` schränkt die Werte, die der Eigenschaft zugewiesen werden können, auf die in `ValueMap` definierte Werteliste ein. Weitere Informationen finden Sie unter [Die Qualifizierer „ValueMap“ und „Value“](/windows/desktop/WmiSdk/value-map).
* Eine empfohlene Methode, einen konsistenten Stil mit integrierten DSC-Ressourcen beizubehalten, ist das Hinzufügen einer Eigenschaft namens `Ensure`, die die Werte `Present` und `Absent` aufweist, zu Ihrer Ressource.
* Benennen Sie die Schemadatei für die benutzerdefinierte Ressource wie folgt: `classname.schema.mof`, wobei `classname` der Bezeichner ist, der dem Schlüsselwort `class` in der Schemadefinition folgt.

### <a name="writing-the-resource-script"></a>Schreiben des Ressourcenskripts

Das Ressourcenskript implementiert die Logik der Ressource. In diesem Modul fügen Sie die drei Funktionen **Get-TargetResource**, **Set-TargetResource**, und **Test-TargetResource** hinzu. Alle drei Funktionen verwenden einen Parametersatz, der identisch mit dem Satz von Eigenschaften ist, die im MOF-Schema definiert wurden, das Sie für die Ressource erstellt haben. In diesem Dokument wird dieser Eigenschaftensatz als die „Ressourceneigenschaften“ bezeichnet. Speichern Sie die drei Funktionen in einer Datei namens „<ResourceName>.psm1“. Im folgenden Beispiel werden die Funktionen in einer Datei namens „Demo_IISWebsite.psm1“ gespeichert.

> [!NOTE]
> Wenn Sie das gleiche Konfigurationsskript mehrfach für Ihre Ressource ausführen, sollten keine Fehler ausgegeben werden, und die Ressource sollte sich im gleichen Zustand wie nach der einmaligen Ausführung des Skripts befinden. Stellen Sie dazu sicher, dass Ihre Funktionen **Get-TargetResource** und **Test-TargetResource** die Ressource unverändert verlassen, und dass das Ergebnis der Funktion **Set-TargetResource** mit denselben Parameterwerten immer identisch ist, egal, ob Sie die Funktion einmal oder mehrfach aufrufen.

Verwenden Sie in der Implementierung der Funktion **Get-TargetResource** die Schlüsselressourcen-Eigenschaftswerte, die als Parameter bereitgestellt werden, um den Status der angegebenen Ressourceninstanz zu überprüfen. Diese Funktion muss eine Hashtabelle zurückgeben, in der alle Ressourceneigenschaften als Schlüssel und die tatsächlichen Werte dieser Eigenschaften als die entsprechende Werte aufgeführt sind. Der folgende Code gibt ein Beispiel.

```powershell
# DSC uses the Get-TargetResource function to fetch the status of the resource instance specified in the parameters for the target machine
function Get-TargetResource
{
    param
    (
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

        $getTargetResourceResult = $null;

        <# Insert logic that uses the mandatory parameter values to get the website and assign it to a variable called $Website #>
        <# Set $ensureResult to "Present" if the requested website exists and to "Absent" otherwise #>

        # Add all Website properties to the hash table
        # This simple example assumes that $Website is not null
        $getTargetResourceResult = @{
                                      Name = $Website.Name;
                                        Ensure = $ensureResult;
                                        PhysicalPath = $Website.physicalPath;
                                        State = $Website.state;
                                        ID = $Website.id;
                                        ApplicationPool = $Website.applicationPool;
                                        Protocol = $Website.bindings.Collection.protocol;
                                        Binding = $Website.bindings.Collection.bindingInformation;
                                    }

        $getTargetResourceResult;
}
```

Abhängig von den Werten, die für die Ressourceneigenschaften im Konfigurationsskript angegeben sind, muss die Funktion **Set-TargetResource** eine der folgenden Aktionen ausführen:

* Erstellen einer neuen Website
* Aktualisieren einer vorhandenen Website
* Löschen einer vorhandenen Website

Das folgende Beispiel veranschaulicht dies.

```powershell
# The Set-TargetResource function is used to create, delete or configure a website on the target machine.
function Set-TargetResource
{
    [CmdletBinding(SupportsShouldProcess=$true)]
    param
    (
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

    <# If Ensure is set to "Present" and the website specified in the mandatory input parameters does not exist, then create it using the specified parameter values #>
    <# Else, if Ensure is set to "Present" and the website does exist, then update its properties to match the values provided in the non-mandatory parameter values #>
    <# Else, if Ensure is set to "Absent" and the website does not exist, then do nothing #>
    <# Else, if Ensure is set to "Absent" and the website does exist, then delete the website #>
}
```

Schließlich muss die Funktion **Test-TargetResource** den gleichen Parametersatz verwenden wie **Get-TargetResource** und **Set-TargetResource**. Überprüfen Sie in der Implementierung von **Test-TargetResource** den Status der in den Schlüsselparametern angegebenen Ressourceninstanz. Wenn der aktuelle Status der Ressourceninstanz nicht mit den im Parametersatz angegebenen Werten übereinstimmt, wird **$false** zurückgegeben. Andernfalls wird **$true** zurückgegeben.

Der folgende Code implementiert die Funktion **Test-TargetResource**.

```powershell
function Test-TargetResource
{
[CmdletBinding()]
[OutputType([System.Boolean])]
param
(
[ValidateSet("Present","Absent")]
[System.String]
$Ensure,

[parameter(Mandatory = $true)]
[System.String]
$Name,

[parameter(Mandatory = $true)]
[System.String]
$PhysicalPath,

[ValidateSet("Started","Stopped")]
[System.String]
$State,

[System.String[]]
$Protocol,

[System.String[]]
$BindingData,

[System.String]
$ApplicationPool
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."


#Include logic to
$result = [System.Boolean]
#Add logic to test whether the website is present and its status mathes the supplied parameter values. If it does, return true. If it does not, return false.
$result
}
```

**Hinweis**: Zum einfacheren Debuggen verwenden Sie in Ihrer Implementierung der vorherigen drei Funktionen das Cmdlet **Write-Verbose**.
>Dieses Cmdlet schreibt Text in den Stream für ausführliche Meldungen.
>Standardmäßig wird der Stream für ausführliche Meldungen nicht angezeigt. Sie können ihn jedoch anzeigen, indem Sie den Wert der Variablen **$VerbosePreference** ändern den Parameter **Verbose** in „DSC cmdlets = new“ verwenden.

### <a name="creating-the-module-manifest"></a>Erstellen das Modulmanifest

Verwenden Sie abschließend das Cmdlet **New-ModuleManifest**, um eine „<ResourceName>psd1“-Datei für das benutzerdefinierte Ressourcenmodul zu definieren. Wenn Sie dieses Cmdlet aufrufen, verweisen Sie auf die im vorherigen Abschnitt beschriebene Skriptmoduldatei (.psm1). Fügen Sie **Get-TargetResource**, **Set-TargetResource** und **Test-TargetResource** zur Liste der zu exportierenden Funktionen hinzu. Im Folgenden finden Sie eine Beispielmanifestdatei.

```powershell
# Module manifest for module 'Demo.IIS.Website'
#
# Generated on: 1/10/2013
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '6AB5ED33-E923-41d8-A3A4-5ADDA2B301DE'

# Author of this module
Author = 'Contoso'

# Company or vendor of this module
CompanyName = 'Contoso'

# Copyright statement for this module
Copyright = 'Contoso. All rights reserved.'

# Description of the functionality provided by this module
Description = 'This Module is used to support the creation and configuration of IIS Websites through Get, Set and Test API on the DSC managed nodes.'

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '4.0'

# Minimum version of the common language runtime (CLR) required by this module
CLRVersion = '4.0'

# Modules that must be imported into the global environment prior to importing this module
RequiredModules = @("WebAdministration")

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
NestedModules = @("Demo_IISWebsite.psm1")

# Functions to export from this module
FunctionsToExport = @("Get-TargetResource", "Set-TargetResource", "Test-TargetResource")

# Cmdlets to export from this module
#CmdletsToExport = '*'

# HelpInfo URI of this module
# HelpInfoURI = ''
}
```

## <a name="supporting-psdscrunascredential"></a>Unterstützung von PsDscRunAsCredential

>**Hinweis:** **PsDscRunAsCredential** wird in PowerShell 5.0 und höher unterstützt.

Mithilfe der Eigenschaft **PsDscRunAsCredential** kann im Ressourcenblock [DSC configurations](../configurations/configurations.md) angegeben werden, dass die Ressource mit einem festgelegten Satz an Anmeldeinformationen ausgeführt werden soll.
Weitere Informationen finden Sie unter [Ausführen von DSC mit Benutzeranmeldeinformationen](../configurations/runAsUser.md).

Um aus einer benutzerdefinierten Ressource auf den Benutzerkontext zuzugreifen, können Sie die automatische Variable `$PsDscContext` verwenden.

Beispielsweise wird mit dem folgenden Code der Benutzerkontext für die Ressourcenausführung in den ausführlichen Ausgabestream geschrieben:

```powershell
if (PsDscContext.RunAsUser) {
    Write-Verbose "User: $PsDscContext.RunAsUser";
}
```

## <a name="rebooting-the-node"></a>Neustarten des Knotens

Wenn die Aktionen in Ihrer `Set-TargetResource`-Funktion einen Neustart erfordern, können Sie ein globales Flag verwenden, um den lokalen Konfigurations-Manager zum Neustart des Knotens anzuweisen. Dieser Neustart tritt unmittelbar nach Abschluss der `Set-TargetResource`-Funktion auf.

Fügen Sie die folgende Codezeile in Ihre `Set-TargetResource`-Funktion ein.

```powershell
# Include this line if the resource requires a system reboot.
$global:DSCMachineStatus = 1
```

Damit der lokale Konfigurations-Manager den Knoten neu startet, muss das Flag **RebootNodeIfNeeded** auf `$true` festgelegt werden. Außerdem sollte die Einstellung **ActionAfterReboot** auf **ContinueConfiguration** festgelegt sein. Dabei handelt es sich um die Standardeinstellung. Weitere Informationen zum Konfigurieren des lokalen Konfigurations-Managers finden Sie unter [Configuring the Local Configuration Manager (Konfigurieren des lokalen Konfigurations-Managers)](../managing-nodes/metaConfig.md) und [Configuring the Local Configuration Manager (v4) (Konfigurieren des lokalen Konfigurations-Managers (PowerShell 4.0))](../managing-nodes/metaConfig4.md).
