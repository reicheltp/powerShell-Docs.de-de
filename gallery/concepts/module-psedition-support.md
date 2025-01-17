---
ms.date: 03/28/2019
contributor: manikb
keywords: gallery,powershell,cmdlet,psget
title: Module mit kompatiblen PowerShell-Editionen
ms.openlocfilehash: 425588c168a4f864fdc0c52aa53cfd748b80dc98
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62084724"
---
# <a name="modules-with-compatible-powershell-editions"></a>Module mit kompatiblen PowerShell-Editionen

Ab Version 5.1 steht PowerShell in verschiedenen Editionen zur Verfügung, die unterschiedliche Featuregruppen und Plattformkompatibilität bieten.

- **Desktop-Edition:** Basiert auf .NET Framework, gilt für Windows PowerShell v4.0 und niedriger sowie Windows PowerShell 5.1 für Windows Desktop, Windows Server, Windows Server Core und die meisten anderen Editionen von Windows.
- **Core-Edition:** Basiert auf .NET Core, gilt für PowerShell Core 6.0 und höher sowie Windows PowerShell 5.1 auf reduzierten Editionen von Windows wie z.B. Windows IoT und Windows Nanoserver.

Weitere Informationen zu den PowerShell-Editionen finden Sie unter [about_PowerShell_Editions][].

## <a name="declaring-compatible-editions"></a>Deklarieren von kompatiblen Editionen

Modulautoren können ihre Module als kompatibel mit einer oder mehreren PowerShell-Editionen deklarieren, indem Sie den Modulmanifestschlüssel CompatiblePSEditions verwenden. Dieser Schlüssel wird nur auf PowerShell 5.1 oder höher unterstützt.

> [!NOTE]
> Sobald mit dem Schlüssel CompatiblePSEditions ein Modulmanifest angegeben wurde, kann es nicht in ältere Versionen als PowerShell 4 importiert werden.

```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$ModuleInfo = Test-ModuleManifest -Path .\TestModuleWithEdition.psd1
$ModuleInfo.CompatiblePSEditions
```

```Output
Desktop
Core
```

```powershell
$ModuleInfo | Get-Member CompatiblePSEditions
```

```Output
   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}
```

Beim Abrufen einer Liste der verfügbaren Module können Sie sie nach PowerShell-Editionen filtern.

```powershell
Get-Module -ListAvailable -PSEdition Desktop
```

```Output
    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions
```

```powershell
Get-Module -ListAvailable -PSEdition Core | % CompatiblePSEditions
```

```Output
Desktop
Core
```

## <a name="targeting-multiple-editions"></a>Festlegen von mehreren Editionen als Ziel

Modulautoren können ein einzelnes Modul veröffentlichen, bei dem eine oder beide PowerShell-Editionen (Desktop und Core) als Ziel festgelegt sind.

Ein einzelnes Modul kann sowohl in der Desktop- als auch in der Core-Edition funktionieren, wenn der Modulautor die dafür benötigte Logik mithilfe der Variable $PSEdition zu RootModule oder dem Modulmanifest hinzufügt. Module können über zwei Sätze kompilierter DLLs verfügen, die jeweils auf CoreCLR und FullCLR ausgerichtet sind. Nachstehend finden Sie die verschiedenen Optionen, mit denen Sie Ihren Modulen Logik hinzufügen können, damit die jeweils korrekten DLLs geladen werden können.

### <a name="option-1-packaging-a-module-for-targeting-multiple-versions-and-multiple-editions-of-powershell"></a>Option 1: Packen eines Moduls für die Adressierung mehrerer Versionen und Editionen von PowerShell

Inhalt des Modulordners

- Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- PSScriptAnalyzer.psd1
- PSScriptAnalyzer.psm1
- ScriptAnalyzer.format.ps1xml
- ScriptAnalyzer.types.ps1xml
- coreclr\Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- coreclr\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- en-US\about_PSScriptAnalyzer.help.txt
- en-US\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll-Help.xml
- PSv3\Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- PSv3\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- Settings\CmdletDesign.psd1
- Settings\DSC.psd1
- Settings\ScriptFunctions.psd1
- Settings\ScriptingStyle.psd1
- Settings\ScriptSecurity.psd1

Inhalt der Datei PSScriptAnalyzer.psd1

```powershell
@{

# Author of this module
Author = 'Microsoft Corporation'

# Script module or binary module file associated with this manifest.
RootModule = 'PSScriptAnalyzer.psm1'

# Version number of this module.
ModuleVersion = '1.6.1'

# ---
}
```

Die nachstehende Logik lädt die erforderlichen Assemblys abhängig von der aktuellen Edition oder Version.

Inhalte der Datei „PSScriptAnalyzer.psm1“:

```powershell
#
# Script module for module 'PSScriptAnalyzer'
#
Set-StrictMode -Version Latest

# Set up some helper variables to make it easier to work with the module
$PSModule = $ExecutionContext.SessionState.Module
$PSModuleRoot = $PSModule.ModuleBase

# Import the appropriate nested binary module based on the current PowerShell version
$binaryModuleRoot = $PSModuleRoot


if (($PSVersionTable.Keys -contains "PSEdition") -and ($PSVersionTable.PSEdition -ne 'Desktop')) {
    $binaryModuleRoot = Join-Path -Path $PSModuleRoot -ChildPath 'coreclr'
}
else
{
    if ($PSVersionTable.PSVersion -lt [Version]'5.0')
    {
        $binaryModuleRoot = Join-Path -Path $PSModuleRoot -ChildPath 'PSv3'
    }
}

$binaryModulePath = Join-Path -Path $binaryModuleRoot -ChildPath 'Microsoft.Windows.PowerShell.ScriptAnalyzer.dll'
$binaryModule = Import-Module -Name $binaryModulePath -PassThru

# When the module is unloaded, remove the nested binary module that was loaded with it
$PSModule.OnRemove = {
    Remove-Module -ModuleInfo $binaryModule
}
```

### <a name="option-2-use-psedition-variable-in-the-psd1-file-to-load-the-proper-dlls-and-nestedrequired-modules"></a>Option 2: Verwenden der Variablen $PSEdition in der PSD1-Datei, um die korrekten DLLs und die geschachtelten/erforderlichen Module zu laden

In PS 5.1 oder höher ist die Variable „$PSEdition global“ in der Modulmanifestdatei zulässig. Durch die Verwendung dieser Variable kann der Modulautor die bedingten Werte in der Modulmanifestdatei angeben. Auf die Variable $PSEdition kann im eingeschränkten Sprachmodus oder in einem „Data“-Abschnitt verwiesen werden.

> [!NOTE]
> Sobald mit dem Schlüssel „CompatiblePSEditions“ ein Modulmanifest angegeben wurde oder dieses die Variable `$PSEdition` verwendet, kann es nicht in ältere Versionen von PowerShell importiert werden.

Beispiel einer Modulmanifestdatei mit dem Schlüssel CompatiblePSEditions

```powershell
@{
    # Script module or binary module file associated with this manifest.
    RootModule = if($PSEdition -eq 'Core')
    {
        'coreclr\MyCoreClrRM.dll'
    }
    else # Desktop
    {
        'clr\MyFullClrRM.dll'
    }

    # Supported PSEditions
    CompatiblePSEditions = 'Desktop', 'Core'

    # Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
    NestedModules = if($PSEdition -eq 'Core')
    {
        'coreclr\MyCoreClrNM1.dll',
        'coreclr\MyCoreClrNM2.dll'
    }
    else # Desktop
    {
        'clr\MyFullClrNM1.dll',
        'clr\MyFullClrNM2.dll'
    }
}
```

### <a name="module-contents"></a>Modulinhalte

```powershell
dir -Recurse
```

```Output
    Directory: C:\Users\manikb\Documents\WindowsPowerShell\Modules\ModuleWithEditions

Mode           LastWriteTime   Length Name
----           -------------   ------ ----
d-----    7/5/2016   1:37 PM          clr
d-----    7/5/2016   1:36 PM          coreclr
-a----    7/5/2016   1:34 PM     4906 ModuleWithEditions.psd1

    Directory: C:\Users\manikb\Documents\WindowsPowerShell\Modules\ModuleWithEditions\clr

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
-a----    7/5/2016   1:35 PM         0 MyFullClrNM1.dll
-a----    7/5/2016   1:35 PM         0 MyFullClrNM2.dll
-a----    7/5/2016   1:35 PM         0 MyFullClrRM.dl

    Directory: C:\Users\manikb\Documents\WindowsPowerShell\Modules\ModuleWithEditions\coreclr

Mode           LastWriteTime   Length Name
----           -------------   ------ ----
-a----    7/5/2016   1:35 PM        0 MyCoreClrNM1.dll
-a----    7/5/2016   1:35 PM        0 MyCoreClrNM2.dll
-a----    7/5/2016   1:35 PM        0 MyCoreClrRM.dl
```

Benutzer des PowerShell-Katalogs finden mithilfe der Tags PSEdition_Desktop und PSEdition_Core eine Liste der in einer bestimmten PowerShell-Edition unterstützten Module.

Module ohne die Tags PSEdition_Desktop und PSEditon_Core sollten in PowerShell Desktop-Editionen keine Probleme bereiten.

```powershell
# Find modules supported on PowerShell Desktop edition
Find-Module -Tag PSEdition_Desktop

# Find modules supported on PowerShell Core editions
Find-Module -Tag PSEdition_Core
```

## <a name="more-details"></a>Weitere Details

[Skripts mit PowerShell-Editionen](script-psedition-support.md)

[Unterstützung von PowerShell-Editionen im PowerShell-Katalog](../how-to/finding-packages/searching-by-compatibility.md)

[Modulmanifest aktualisieren](/powershell/module/powershellget/update-modulemanifest)

[about_PowerShell_Editions][]

[about_PowerShell_Editions]: /powershell/module/Microsoft.PowerShell.Core/About/about_PowerShell_Editions
