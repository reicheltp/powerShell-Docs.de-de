---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: Erste Schritte mit DSC für Linux
ms.openlocfilehash: a18b226d4b2d8b8e1ba8b4168ec6ad8f73c73c42
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62079692"
---
# <a name="get-started-with-desired-state-configuration-dsc-for-linux"></a>Erste Schritte mit DSC für Linux

In diesem Thema werden die ersten Schritte mit PowerShell DSC für Linux erläutert. Allgemeine Informationen zu DSC finden Sie unter [Erste Schritte mit Windows PowerShell DSC](../overview/overview.md).

## <a name="supported-linux-operation-system-versions"></a>Unterstützte Linux-Betriebssystemversionen

Die folgenden Linux-Betriebssystemversionen werden für DSC unterstützt.

- CentOS 5, 6 und 7 (x86/x64)
- Debian GNU/Linux 6, 7 und 8 (x86/x64)
- Oracle Linux 5, 6 und 7 (x86/x64)
- Red Hat Enterprise Linux Server 5, 6 und 7 (x86/x64)
- SUSE Linux Enterprise Server 10, 11 und 12 (x86/x64)
- Ubuntu Server 12.04 LTS, 14.04 LTS und 16.04 LTS (x86/x64)

In der folgenden Tabelle werden die erforderlichen Paketabhängigkeiten für DSC für Linux beschrieben.

|  Erforderliches Paket |  Beschreibung |  Mindestversion |
|---|---|---|
| glibc| GNU-Bibliothek| 2…4 – 31.30|
| python| Python| 2.4 – 3.4|
| omiserver| Open Management Infrastructure| 1.0.8.1|
| openssl| OpenSSL-Bibliotheken| 0.9.8 oder 1.0|
| ctypes| Python CTypes-Bibliothek| Muss mit Python-Version übereinstimmen|
| libcurl| cURL http-Clientbibliothek| 7.15.1|

## <a name="installing-dsc-for-linux"></a>Installieren von DSC für Linux

Sie müssen vor der Installation von DSC für Linux die [Open Management Infrastructure (OMI)](https://github.com/Microsoft/omi) installieren.

### <a name="installing-omi"></a>Installieren von OMI

DSC für Linux erfordert den Open Management Infrastructure (OMI) CIM-Server, Version 1.0.8.1 oder höher. OMI kann von The Open Group heruntergeladen werden: [Open Management Infrastructure (OMI)](https://github.com/Microsoft/omi).

Zum Installieren von OMI installieren Sie das Ihrem Linux-System entsprechende Paket (RPM oder DEB), die OpenSSL-Version (ssl_098 oder ssl_100) und die Architektur (x64/x86). RPM-Pakete eignen sich für CentOS, Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Oracle Linux. DEB-Pakete sind für Debian GNU/Linux und Ubuntu Server geeignet. Die ssl_098-Pakete eignen sich für Computer mit installiertem OpenSSL 0.9.8, während die ssl_100 Pakete für Computer mit installiertem OpenSSL 1.0 geeignet sind.

> [!NOTE]
> Um die installierte OpenSSL-Version zu bestimmen, führen Sie den Befehl `openssl version` aus.

Führen Sie den folgenden Befehl aus, um OMI auf einem CentOS 7 x64-System zu installieren.

`# sudo rpm -Uvh omiserver-1.0.8.ssl_100.rpm`

### <a name="installing-dsc"></a>Installieren von DSC

DSC für Linux kann [hier](https://github.com/Microsoft/PowerShell-DSC-for-Linux/releases/tag/v1.1.1-294) heruntergeladen werden.

Zum Installieren von DSC installieren Sie das Ihrem Linux-System entsprechende Paket (RPM oder DEB), die OpenSSL-Version (ssl_098 oder ssl_100) und die Architektur (x64/x86). RPM-Pakete eignen sich für CentOS, Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Oracle Linux. DEB-Pakete sind für Debian GNU/Linux und Ubuntu Server geeignet. Die ssl_098-Pakete eignen sich für Computer mit installiertem OpenSSL 0.9.8, während die ssl_100 Pakete für Computer mit installiertem OpenSSL 1.0 geeignet sind.

> [!NOTE]
> Um die installierte OpenSSL-Version zu bestimmen, führen Sie den Befehl „openssl version“ aus.

Führen Sie den folgenden Befehl aus, um DSC auf einem CentOS 7 x64-System zu installieren.

`# sudo rpm -Uvh dsc-1.0.0-254.ssl_100.x64.rpm`

## <a name="using-dsc-for-linux"></a>Verwenden von DSC für Linux

In den folgenden Abschnitten wird erläutert, wie DSC-Konfigurationen erstellt und auf Linux-Computern ausgeführt werden.

### <a name="creating-a-configuration-mof-document"></a>Erstellen eines MOF-Konfigurationsdokuments

Das Windows PowerShell-Schlüsselwort „Configuration“ wird wie für Windows-Computer verwendet, um eine Konfiguration für Linux-Computer zu erstellen. Es folgen die Schritte zum Erstellen eines Konfigurationsdokuments für einen Linux-Computer mithilfe von Windows PowerShell.

1. Importieren Sie das Modul „nx“. Das Windows PowerShell-Modul „nx“ enthält das Schema für integrierte Ressourcen für DSC für Linux und muss auf dem lokalen Computer installiert und in die Konfiguration importiert werden.

   - Zum Installieren des Moduls „nx“ kopieren Sie das Verzeichnis dieses Moduls entweder in `$env:USERPROFILE\Documents\WindowsPowerShell\Modules\` oder in `$PSHOME\Modules`. Das Modul „nx“ ist im Installationspaket von DSC für Linux enthalten. Verwenden Sie zum Importieren des Moduls „nx“ in Ihre Konfiguration den Befehl `Import-DSCResource`:

   ```powershell
   Configuration ExampleConfiguration{

    Import-DSCResource -Module nx

   }
   ```

2. Definieren Sie eine Konfiguration, und generieren Sie das Konfigurationsdokument:

   ```powershell
   Configuration ExampleConfiguration
   {
        Import-DscResource -Module nx

        Node  "linuxhost.contoso.com"
        {
            nxFile ExampleFile 
            {
                DestinationPath = "/tmp/example"
                Contents = "hello world `n"
                Ensure = "Present"
                Type = "File"
            }
        }
   }

   ExampleConfiguration -OutputPath:"C:\temp"
   ```

### <a name="push-the-configuration-to-the-linux-computer"></a>Übertragen der Konfiguration per Push auf den Linux-Computer

Konfigurationsdokumente (MOF-Dateien) können mit dem Cmdlet [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) per Push auf den Linux-Computer übertragen werden. Verwenden Sie eine CIMSession, um dieses Cmdlet zusammen mit den Cmdlets [Get-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Get-DscConfiguration) und [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration) remote auf einem Linux-Computer zu verwenden. Das Cmdlet [New-CimSession](/powershell/module/CimCmdlets/New-CimSession) dient zum Starten einer CIMSession mit dem Linux-Computer.

Der folgende Code zeigt, wie Sie eine CIMSession für DSC für Linux starten.

```powershell
$Node = "ostc-dsc-01"
$Credential = Get-Credential -UserName "root" -Message "Enter Password:"

#Ignore SSL certificate validation
#$opt = New-CimSessionOption -UseSsl $true -SkipCACheck $true -SkipCNCheck $true -SkipRevocationCheck $true

#Options for a trusted SSL certificate
$opt = New-CimSessionOption -UseSsl $true
$Sess=New-CimSession -Credential $credential -ComputerName $Node -Port 5986 -Authentication basic -SessionOption $opt -OperationTimeoutSec 90
```

> [!NOTE]
> Im Pushmodus müssen die Anmeldeinformationen des Benutzers denen des Benutzers „root“ auf dem Linux-Computer entsprechen.
> Für DSC für Linux werden nur SSL/TLS-Verbindungen unterstützt. Der Befehl `New-CimSession` muss mit auf „$true“ festgelegtem „–UseSSL“-Parameter aufgerufen werden.
> Das von OMI (für DSC) verwendete SSL-Zertifikat wird in der Datei `/opt/omi/etc/omiserver.conf` mit den Eigenschaften „pemfile“ und „keyfile“ angegeben.
> Wenn diesem Zertifikat vom Windows-Computer nicht vertraut wird, auf dem Sie das Cmdlet [New-CimSession](/powershell/module/CimCmdlets/New-CimSession) ausführen, können Sie in den Optionen für „CIMSession“ die Überprüfung des Zertifikats ignorieren: `-SkipCACheck $true -SkipCNCheck $true -SkipRevocationCheck $true`

Führen Sie den folgenden Befehl aus, um die DSC-Konfiguration per Push auf den Linux-Knoten zu übertragen.

`Start-DscConfiguration -Path:"C:\temp" -CimSession $Sess -Wait -Verbose`

### <a name="distribute-the-configuration-with-a-pull-server"></a>Verteilen der Konfiguration mit einem Pullserver

Konfigurationen können mithilfe eines Pullservers wie bei Windows-Computern auch an Linux-Computer verteilt werden. Eine Anleitung zur Verwendung eines Pullservers finden Sie unter [Windows PowerShell DSC – Pullserver](../pull-server/pullServer.md). Weitere Informationen und Einschränkungen im Zusammenhang mit der Verwendung von Linux-Computern mit einem Pullserver finden Sie die Versionshinweisen zu DSC für Linux.

### <a name="working-with-configurations-locally"></a>Arbeiten mit lokalen Konfigurationen

DSC für Linux bietet Skripts für das Ausführen von Konfigurationsaufgaben auf einem lokalen Linux-Computer. Diese Skripts befinden sich in `/opt/microsoft/dsc/Scripts` und umfassen Folgendes:

- GetDscConfiguration.py

Gibt die aktuell auf dem Computer installierte Konfiguration zurück. Dies ist vergleichbar mit dem Windows PowerShell-Cmdlet `Get-DscConfiguration`.

`# sudo ./GetDscConfiguration.py`

- GetDscLocalConfigurationManager.py

Gibt die aktuell auf dem Computer installierte Metakonfiguration zurück. Vergleichbar mit dem Cmdlet [Get-DSCLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager).

`# sudo ./GetDscLocalConfigurationManager.py`

- InstallModule.py

Installiert ein benutzerdefiniertes DSC-Ressourcenmodul. Erfordert den Pfad zu einer ZIP-Datei, die die freigegebene Objektbibliothek des Moduls und die MOF-Dateien mit dem Schema enthält.

`# sudo ./InstallModule.py /tmp/cnx_Resource.zip`

- RemoveModule.py

Entfernt ein benutzerdefiniertes DSC-Ressourcenmodul. Erfordert die Angabe des Namens des Moduls, das entfernt werden soll.

`# sudo ./RemoveModule.py cnx_Resource`

- StartDscLocalConfigurationManager.py

Wendet eine MOF-Konfigurationsdatei auf den Computer an. Vergleichbar mit dem Cmdlet [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration). Erfordert die Angabe des Pfads zur anzuwendenden MOF-Konfigurationsdatei.

`# sudo ./StartDscLocalConfigurationManager.py –configurationmof /tmp/localhost.mof`

- SetDscLocalConfigurationManager.py

Wendet eine MOF-Metakonfigurationsdatei auf den Computer an. Vergleichbar mit dem Cmdlet [Set-DSCLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager). Erfordert die Angabe des Pfads zur anzuwendenden MOF-Metakonfigurationsdatei.

`# sudo ./SetDscLocalConfigurationManager.py –configurationmof /tmp/localhost.meta.mof`

## <a name="powershell-desired-state-configuration-for-linux-log-files"></a>PowerShell – DSC für Linux – Protokolldateien

Die folgenden Protokolldateien werden für DSC-für-Linux-Nachrichten generiert.

|Protokolldatei|Verzeichnis|Beschreibung|
|---|---|---|
|**omiserver.log**|`/var/opt/omi/log`|Meldungen im Zusammenhang mit dem Betrieb des OMI CIM-Servers.|
|**dsc.log**|`/var/opt/omi/log`|Meldungen im Zusammenhang mit dem Betrieb des lokalen Konfigurations-Managers (LCM) und DSC-Ressourcenvorgängen.|
