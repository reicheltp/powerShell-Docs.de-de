---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: DSC-Ressource „Service“
ms.openlocfilehash: 09571bd0eaa428e7d0bb7a533d6ad1c0c936e2cf
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076893"
---
# <a name="dsc-service-resource"></a>DSC-Ressource „Service“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0


Die Ressource **Service** in Windows PowerShell DSC bietet einen Mechanismus zum Verwalten von Diensten auf dem Zielknoten.

## <a name="syntax"></a>Syntax

```
Service [string] #ResourceName
{
    Name = [string]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ State = [string] { Running | Stopped }  ]
    [ Description = [string] ]
    [ DisplayName = [string] ]
    [ Ensure = [string] { Absent | Present } ]
    [ Path = [string] ]
}
```

## <a name="properties"></a>Eigenschaften

|  Eigenschaft  |  Beschreibung   |
|---|---|
| Name| Gibt den Namen des Diensts an. Beachten Sie, dass sich dieser mitunter vom Anzeigenamen unterscheidet. Mit dem Cmdlet „Get-Service“ können Sie eine Liste der Dienste und ihren aktuellen Status abrufen.|
| BuiltInAccount| Gibt das zu verwendende Anmeldekonto für den Dienst an. Die für diese Eigenschaft zulässigen Werte sind: **LocalService**, **LocalSystem** und **NetworkService**.|
| Credential| Gibt die Anmeldeinformationen für das Konto an, unter dem der Dienst ausgeführt wird. Diese Eigenschaft und die __BuiltinAccount__-Eigenschaft können nicht zusammen verwendet werden.|
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.|
| StartupType| Gibt den Starttyp für den Dienst an. Die für diese Eigenschaft zulässigen Werte sind: **Automatic**, **Disabled** und **Manual**.|
| Status| Gibt den Status an, den Sie für den Dienst sicherstellen möchten.|
| Beschreibung | Gibt die Beschreibung des Zieldiensts an.|
| DisplayName | Gibt den Anzeigenamen des Zieldiensts an.|
| Ensure | Gibt an, ob der Zieldienst auf dem System vorhanden ist. Legen Sie diese Eigenschaft auf **Absent** fest, um sicherzustellen, dass der Zieldienst nicht vorhanden ist. Das Festlegen auf **Present** (den Standardwert) stellt sicher, dass der Zieldienst vorhanden ist.|
| Pfad | Gibt den Pfad zur Binärdatei eines neuen Diensts an.|

## <a name="example"></a>Beispiel

```powershell
configuration ServiceTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        Service ServiceExample
        {
            Name        = "TermService"
            StartupType = "Manual"
            State       = "Running"
        }
    }
}
```