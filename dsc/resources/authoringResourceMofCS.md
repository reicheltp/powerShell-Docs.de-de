---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: Erstellen einer DSC-Ressource in C#
ms.openlocfilehash: 6f2bb4d411237f13e2735c2e5f630b4f40dc6842
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076649"
---
# <a name="authoring-a-dsc-resource-in-c"></a>Erstellen einer DSC-Ressource in C\#

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

In der Regel wird eine benutzerdefinierte Windows PowerShell DSC-Ressource in einem PowerShell-Skript implementiert. Allerdings können Sie die Funktionalität einer benutzerdefinierten DSC-Ressource auch durch Schreiben von C#-Cmdlets implementieren. Eine Einführung zum Schreiben von Cmdlets in C# finden Sie unter [Schreiben eines Windows PowerShell-Cmdlets](/powershell/developer/windows-powershell).

Abgesehen von der Implementierung der Ressource als Cmdlets in C# entsprechen die Schritte zum Erstellen des MOF-Schemas, zum Erstellen der Ordnerstruktur sowie zum Importieren und Verwenden Ihrer benutzerdefinierten DSC-Ressource dem unter [Schreiben einer benutzerdefinierten DSC-Ressource mit MOF](authoringResourceMOF.md) beschriebenen Verfahren.

## <a name="writing-a-cmdlet-based-resource"></a>Schreiben einer Cmdlet-basierten Ressource
In diesem Beispiel wird eine einfache Ressource implementiert, die eine Textdatei und deren Inhalt verwaltet.

### <a name="writing-the-mof-schema"></a>Schreiben des MOF-Schemas

Im folgenden sehen Sie die Definition der MOF-Ressource.

```
[ClassVersion("1.0.0"), FriendlyName("xDemoFile")]
class MSFT_XDemoFile : OMI_BaseResource
{
                [Key, Description("path")] String Path;
                [Write, Description("Should the file be present"), ValueMap{"Present","Absent"}, Values{"Present","Absent"}] String Ensure;
                [Write, Description("Contentof file.")] String Content;
};
```

### <a name="setting-up-the-visual-studio-project"></a>Einrichten des Visual Studio-Projekts
#### <a name="setting-up-a-cmdlet-project"></a>Einrichten eines Cmdlet-Projekts

1. Öffnen Sie Visual Studio.
1. Erstellen Sie ein C#-Projekt, und geben Sie diesem einen Namen.
1. Wählen Sie **-Klassenbibliothek** aus den verfügbaren Projektvorlagen aus.
1. Klicken Sie auf **OK**.
1. Fügen Sie „System.Automation.Management.dll“ einen Assemblyverweis auf Ihr Projekt hinzu.
1. Ändern Sie den Assemblynamen so, dass er dem Namen der Ressource entspricht. In diesem Fall sollte die Assembly **MSFT_XDemoFile** benannt werden.

### <a name="writing-the-cmdlet-code"></a>Schreiben des Codes des Cmdlets

Der folgende C#-Code implementiert die Cmdlets **Get-TargetResource**, **Set-TargetResource** und **Test-TargetResource**.

```C#


namespace cSharpDSCResourceExample
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Management.Automation;  // Windows PowerShell assembly.

    #region Get-TargetResource

    [OutputType(typeof(System.Collections.Hashtable))]
    [Cmdlet(VerbsCommon.Get, "TargetResource")]
    public class GetTargetResource : PSCmdlet
    {
        [Parameter(Mandatory = true)]
        public string Path { get; set; }

        /// <summary>
        /// Implement the logic to return the current state of the resource as a hashtable with keys being the resource properties
        /// and the values are the corresponding current value on the machine.
        /// </summary>
        protected override void ProcessRecord()
        {
            var currentResourceState = new Dictionary<string, string>();
            if (File.Exists(Path))
            {
                currentResourceState.Add("Ensure", "Present");

                // read current content
                string CurrentContent = "";
                using (var reader = new StreamReader(Path))
                {
                    CurrentContent = reader.ReadToEnd();
                }
                currentResourceState.Add("Content", CurrentContent);
            }
            else
            {
                currentResourceState.Add("Ensure", "Absent");
                currentResourceState.Add("Content", "");
            }
            // write the hashtable in the PS console.
            WriteObject(currentResourceState);
        }
    }

    # endregion

    #region Set-TargetResource
    [OutputType(typeof(void))]
    [Cmdlet(VerbsCommon.Set, "TargetResource")]
    public class SetTargetResource : PSCmdlet
    {
        [Parameter(Mandatory = true)]
        public string Path { get; set; }

        [Parameter(Mandatory = false)]

        [ValidateSet("Present", "Absent", IgnoreCase = true)]
        public string Ensure {
            get
            {
                // set the default to present.
               return (this._ensure ?? "Present") ;
            }
            set
            {
                this._ensure = value;
            }
        }

        [Parameter(Mandatory = false)]
        public string Content {
            get { return (string.IsNullOrEmpty(this._content) ? "" : this._content); }
            set { this._content = value; }
        }

        private string _ensure;
        private string _content;

        /// <summary>
        /// Implement the logic to set the state of the machine to the desired state.
        /// </summary>
        protected override void ProcessRecord()
        {
            WriteVerbose(string.Format("Running set with parameters {0}{1}{2}", Path, Ensure, Content));
            if (File.Exists(Path))
            {
                if (Ensure.Equals("absent", StringComparison.InvariantCultureIgnoreCase))
                {
                    File.Delete(Path);
                }
                else
                {
                    // file already exist and ensure "present" is specified. start writing the content to a file
                    if (!string.IsNullOrEmpty(Content))
                    {
                        string existingContent = null;
                        using (var reader = new StreamReader(Path))
                        {
                            existingContent = reader.ReadToEnd();
                        }
                        // check if the content of the file mathes the content passed
                        if (!existingContent.Equals(Content, StringComparison.InvariantCultureIgnoreCase))
                        {
                            WriteVerbose("Existing content did not match with desired content updating the content of the file");
                            using (var writer = new StreamWriter(Path))
                            {
                                writer.Write(Content);
                                writer.Flush();
                            }
                        }
                    }
                }

            }
            else
            {
                if (Ensure.Equals("present", StringComparison.InvariantCultureIgnoreCase))
                {
                    // if nothing is passed for content just write "" otherwise write the content passed.
                    using (var writer = new StreamWriter(Path))
                    {
                        WriteVerbose(string.Format("Creating a file under path {0} with content {1}", Path, Content));
                        writer.Write(Content);
                    }
                }

            }

            /* if you need to reboot the VM. please add the following two line of code.
            PSVariable DscMachineStatus = new PSVariable("DSCMachineStatus", 1, ScopedItemOptions.AllScope);
            this.SessionState.PSVariable.Set(DscMachineStatus);
             */

        }

    }

    # endregion

    #region Test-TargetResource

    [Cmdlet("Test", "TargetResource")]
    [OutputType(typeof(Boolean))]
    public class TestTargetResource : PSCmdlet
    {
        [Parameter(Mandatory = true)]
        public string Path { get; set; }

        [Parameter(Mandatory = false)]

        [ValidateSet("Present", "Absent", IgnoreCase = true)]
        public string Ensure
        {
            get
            {
                // set the default to present.
                return (this._ensure ?? "Present");
            }
            set
            {
                this._ensure = value;
            }
        }

        [Parameter(Mandatory = false)]
        public string Content
        {
            get { return (string.IsNullOrEmpty(this._content) ? "" : this._content); }
            set { this._content = value; }
        }

        private string _ensure;
        private string _content;

        /// <summary>
        /// Return a boolean value which indicates wheather the current machine is in desired state or not.
        /// </summary>
        protected override void ProcessRecord()
        {
            if (File.Exists(Path))
            {
                if( Ensure.Equals("absent", StringComparison.InvariantCultureIgnoreCase))
                {
                    WriteObject(false);
                }
                else
                {
                    // check if the content matches

                    string existingContent = null;
                    using (var stream = new StreamReader(Path))
                    {
                        existingContent = stream.ReadToEnd();
                    }

                    WriteObject(Content.Equals(existingContent, StringComparison.InvariantCultureIgnoreCase));
                }
            }
            else
            {
                WriteObject(Ensure.Equals("Absent", StringComparison.InvariantCultureIgnoreCase));
            }
        }
    }

    # endregion

}
```

### <a name="deploying-the-resource"></a>Bereitstellen der Ressource

Die kompilierte DLL-Datei sollte in einer Dateistruktur gespeichert werden, die einer skriptbasierten Ressource ähnelt. Im folgenden finden die Ordnerstruktur für diese Ressource.

```
$env: psmodulepath (folder)
    |- MyDscResources (folder)
        |- MyDscResources.psd1 (file, required)
        |- DSCResources (folder)
            |- MSFT_XDemoFile (folder)
                |- MSFT_XDemoFile.psd1 (file, optional)
                |- MSFT_XDemoFile.dll (file, required)
                |- MSFT_XDemoFile.schema.mof (file, required)
```

### <a name="see-also"></a>Weitere Informationen
#### <a name="concepts"></a>Konzepte
[Schreiben einer benutzerdefinierten DSC-Ressource mit MOF](authoringResourceMOF.md)
#### <a name="other-resources"></a>Weitere Ressourcen
[Schreiben eines Windows PowerShell-Cmdlets](/powershell/developer/windows-powershell)