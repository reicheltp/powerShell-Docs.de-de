---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen
ms.openlocfilehash: 34356f65bcb83153e7395a16d2a4a5cf2e507332
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076717"
---
# <a name="writing-a-custom-dsc-resource-with-powershell-classes"></a>Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen

> Gilt für: Windows PowerShell 5.0

Mit der Einführung der PowerShell-Klassen in Windows PowerShell 5.0 können Sie jetzt eine DSC-Ressourcen durch Erstellen einer Klasse definieren. Die Klasse definiert das Schema und die Implementierung der Ressource, daher besteht keine Notwendigkeit, eine separate MOF-Datei zu erstellen. Die Ordnerstruktur für eine klassenbasierte Ressource ist auch einfacher, da kein **DSCResources**-Ordner erforderlich ist.

In einer klassenbasierten DSC-Ressource wird das Schema als Eigenschaften der Klasse definiert, die mit Attributen für den Eigenschaftstyp geändert werden können. Die Ressource wird mit den Methoden **Get()**, **Set()** und **Test()** implementiert (entspricht den Funktionen **Get-TargetResource**, **Set-TargetResource** und **Test-TargetResource** in einer Skriptressource.

In diesem Thema wird eine einfache Ressource mit dem Namen **FileResource** erstellt, die eine Datei unter einem angegebenen Pfad verwaltet.

Weitere Informationen zu DSC-Ressourcen finden Sie unter [Erstellen von benutzerdefinierten Windows PowerShell DSC-Ressourcen](authoringResource.md).

>**Hinweis:** Generische Sammlungen werden nicht in auf Klassen basierenden Ressourcen unterstützt.

## <a name="folder-structure-for-a-class-resource"></a>Ordnerstruktur für eine Klassenressource

Um eine benutzerdefinierte DSC-Ressource mit einer PowerShell-Klasse zu implementieren, erstellen Sie die folgende Ordnerstruktur. Die Klasse wird in **MyDscResource.psm1** und das Modulmanifest in **MyDscResource.psd1** definiert.

```
$env:ProgramFiles\WindowsPowerShell\Modules (folder)
    |- MyDscResource (folder)
        MyDscResource.psm1
        MyDscResource.psd1
```

## <a name="create-the-class"></a>Erstellen einer Klasse

Sie verwenden das Schlüsselwort „class“ zum Erstellen einer PowerShell-Klasse. Um anzugeben, dass eine Klasse eine DSC-Ressource ist, verwenden Sie das Attribut **DscResource()**. Der Name der Klasse ist der Name der DSC-Ressource.

```powershell
[DscResource()]
class FileResource {
}
```

### <a name="declare-properties"></a>Deklarieren von Eigenschaften

Das DSC-Ressourcenschema wird als Eigenschaften der Klasse definiert. Es wurden die folgenden drei Eigenschaften deklariert.

```powershell
[DscProperty(Key)]
[string]$Path

[DscProperty(Mandatory)]
[Ensure] $Ensure

[DscProperty(Mandatory)]
[string] $SourcePath

[DscProperty(NotConfigurable)]
[Nullable[datetime]] $CreationTime
```

Beachten Sie, dass die Eigenschaften durch Attribute geändert werden. Die Attribute haben folgende Bedeutungen:

- **DscProperty(Key)**: Die Eigenschaft ist erforderlich. Die Eigenschaft ist ein Schlüssel. Die Werte aller als Schlüssel gekennzeichneten Eigenschaften in Kombination müssen eine Ressourceninstanz in einer Konfiguration eindeutig identifizieren.
- **DscProperty(Mandatory)**: Die Eigenschaft ist erforderlich.
- **DscProperty(NotConfigurable)**: Die Eigenschaft ist schreibgeschützt. Eigenschaften, die mit diesem Attribut gekennzeichnet sind, können nicht von einer Konfiguration festgelegt werden, sondern werden durch die Methode **Get()**, wenn vorhanden, aufgefüllt.
- **DscProperty()**: Die Eigenschaft ist konfigurierbar, aber nicht erforderlich.

Die Eigenschaften **$Path** und **$SourcePath** sind Zeichenfolgen. **$CreationTime** ist eine [DateTime](/dotnet/api/system.datetime)-Eigenschaft. Die Eigenschaft **$Ensure** ist ein Enumerationstyp, der wie folgt definiert ist:

```powershell
enum Ensure
{
    Absent
    Present
}
```

### <a name="implementing-the-methods"></a>Implementieren der Methoden

Die Methoden **Get()**, **Set()** und **Test()** werden analog zu den Funktionen **Get-TargetResource**, **Set-TargetResource** und **Test-TargetResource** in einer Skriptressource implementiert.

Dieser Code umfasst auch die Funktion „CopyFile()“, eine Hilfsfunktion, die die Datei von **$SourcePath** nach **$Path** kopiert.

```powershell
    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)

        if ($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ErrorAction Ignore

        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = New-Object -TypeName System.IO.FileInfo($this.Path)

        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
```

### <a name="the-complete-file"></a>Die vollständige Datei

Die vollständige Klassendatei folgt.

```powershell
enum Ensure
{
    Absent
    Present
}

<#
   This resource manages the file in a specific path.
   [DscResource()] indicates the class is a DSC resource
#>

[DscResource()]
class FileResource
{
    <#
       This property is the fully qualified path to the file that is
       expected to be present or absent.

       The [DscProperty(Key)] attribute indicates the property is a
       key and its value uniquely identifies a resource instance.
       Defining this attribute also means the property is required
       and DSC will ensure a value is set before calling the resource.

       A DSC resource must define at least one key property.
    #>
    [DscProperty(Key)]
    [string]$Path

    <#
        This property indicates if the settings should be present or absent
        on the system. For present, the resource ensures the file pointed
        to by $Path exists. For absent, it ensures the file point to by
        $Path does not exist.

        The [DscProperty(Mandatory)] attribute indicates the property is
        required and DSC will guarantee it is set.

        If Mandatory is not specified or if it is defined as
        Mandatory=$false, the value is not guaranteed to be set when DSC
        calls the resource.  This is appropriate for optional properties.
    #>
    [DscProperty(Mandatory)]
    [Ensure] $Ensure

    <#
       This property defines the fully qualified path to a file that will
       be placed on the system if $Ensure = Present and $Path does not
        exist.

       NOTE: This property is required because [DscProperty(Mandatory)] is
        set.
    #>
    [DscProperty(Mandatory)]
    [string] $SourcePath

    <#
       This property reports the file's create timestamp.

       [DscProperty(NotConfigurable)] attribute indicates the property is
       not configurable in DSC configuration.  Properties marked this way
       are populated by the Get() method to report additional details
       about the resource when it is present.

    #>
    [DscProperty(NotConfigurable)]
    [Nullable[datetime]] $CreationTime

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)
        if ($this.ensure -eq [Ensure]::Present)
        {
            if (-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                 to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
} # This module defines a class for a DSC "FileResource" provider.
```

## <a name="create-a-manifest"></a>Erstellen eines Manifests

Um eine klassenbasierte Ressource für die DSC-Engine verfügbar zu machen, müssen Sie eine **DscResourcesToExport**-Anweisung zur Manifestdatei hinzufügen, die die Engine anweist, die Ressource zu exportieren. Unser Manifest sieht folgendermaßen aus:

```powershell
@{

# Script module or binary module file associated with this manifest.
RootModule = 'MyDscResource.psm1'

DscResourcesToExport = 'FileResource'

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''
}
```

## <a name="test-the-resource"></a>Testen der Ressource

Nachdem Sie die Klasse und die Manifestdateien, wie zuvor beschrieben, in der Ordnerstruktur gespeichert haben, können Sie eine Konfiguration erstellen, die die neue Ressource verwendet. Informationen dazu, wie Sie eine DSC-Konfiguration ausführen, finden Sie unter [Inkraftsetzung von Konfigurationen](../pull-server/enactingConfigurations.md). Die folgende Konfiguration überprüft, ob die Datei unter `c:\test\test.txt` vorhanden ist, und kopiert sie bei Bedarf aus `c:\test.txt` (Sie sollten `c:\test.txt` vor dem Ausführen der Konfiguration erstellen).

```powershell
Configuration Test
{
    Import-DSCResource -module MyDscResource
    FileResource file
    {
        Path = "C:\test\test.txt"
        SourcePath = "c:\test.txt"
        Ensure = "Present"
    }
}
Test
Start-DscConfiguration -Wait -Force Test
```

## <a name="supporting-psdscrunascredential"></a>Unterstützung von PsDscRunAsCredential

>**Hinweis:** **PsDscRunAsCredential** wird in PowerShell 5.0 und höher unterstützt.

Mithilfe der Eigenschaft **PsDscRunAsCredential** kann im Ressourcenblock [DSC configurations](../configurations/configurations.md) angegeben werden, dass die Ressource mit einem festgelegten Satz an Anmeldeinformationen ausgeführt werden soll.
Weitere Informationen finden Sie unter [Ausführen von DSC mit Benutzeranmeldeinformationen](../configurations/runAsUser.md).

### <a name="require-or-disallow-psdscrunascredential-for-your-resource"></a>Anfordern oder Untersagen von PsDscRunAsCredential für Ihre Ressource

Das Attribut **DscResource()** verwendet einen optionalen Parameter **RunAsCredential**.
Dieser Parameter kann einen von drei Werten annehmen:

- `Optional` **PsDscRunAsCredential** ist optional für Ressourcen, die diese Ressource aufrufen. Dies ist der Standardwert.
- `Mandatory` **PsDscRunAsCredential** muss für alle Konfigurationen verwendet werden, die diese Ressource aufrufen.
- `NotSupported`Konfigurationen, die diese Ressource aufrufen, können **PsDscRunAsCredential** nicht verwenden.
- `Default` Identisch mit `Optional`.

Verwenden Sie beispielsweise das folgende Attribut, um anzugeben, dass Ihre benutzerdefinierte Ressource die Verwendung von **PsDscRunAsCredential** nicht unterstützt:

```powershell
[DscResource(RunAsCredential=NotSupported)]
class FileResource {
}
```

### <a name="declaring-multiple-class-resources-in-a-module"></a>Deklarieren von Ressourcen mit mehreren Klassen in einem Modul

Ein Modul kann mehrere klassenbasierte DSC-Ressourcen definieren. Sie können die Ordnerstruktur auf folgende Weise erstellen:

1. Definieren Sie die erste Ressource in der Datei „<ModuleName>.psm1“ und die nachfolgenden Ressourcen unter dem Ordner **DSCResources**.

   ```
   $env:ProgramFiles\WindowsPowerShell\Modules (folder)
        |- MyDscResource (folder)
           |- MyDscResource.psm1
              MyDscResource.psd1
        |- DSCResources
           |- SecondResource.psm1
   ```

2. Definieren Sie alle Ressourcen unter dem Ordner **DSCResources**.

   ```
   $env:ProgramFiles\WindowsPowerShell\Modules (folder)
        |- MyDscResource (folder)
           |- MyDscResource.psm1
              MyDscResource.psd1
        |- DSCResources
           |- FirstResource.psm1
              SecondResource.psm1
   ```

> [!NOTE]
> Fügen Sie in den Beispielen oben alle PSM1-Dateien unter **DSCResources** dem Schlüssel **NestedModules** in Ihrer PSD1-Datei hinzu.

### <a name="access-the-user-context"></a>Zugriff auf den Benutzerkontext

Um aus einer benutzerdefinierten Ressource auf den Benutzerkontext zuzugreifen, können Sie die automatische Variable `$global:PsDscContext` verwenden.

Beispielsweise wird mit dem folgenden Code der Benutzerkontext für die Ressourcenausführung in den ausführlichen Ausgabestream geschrieben:

```powershell
if (PsDscContext.RunAsUser) {
    Write-Verbose "User: $global:PsDscContext.RunAsUser";
}
```

## <a name="see-also"></a>Weitere Informationen

[Erstellen von benutzerdefinierten Windows PowerShell DSC-Ressourcen](authoringResource.md)
