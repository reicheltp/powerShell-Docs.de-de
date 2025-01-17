---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: DSC für Linux-Ressource „nxSshAuthorizedKeys“
ms.openlocfilehash: d4cdb727a94a5e89e8401769f24977d49bcf4929
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62077703"
---
# <a name="dsc-for-linux-nxsshauthorizedkeys-resource"></a>DSC für Linux-Ressource „nxSshAuthorizedKeys“

Die Ressource **nxAuthorizedKeys** in PowerShell DSC bietet ein Mechanismus zum Verwalten autorisierter SSH-Schlüssel für einen angegebenen Benutzer.

## <a name="syntax"></a>Syntax

```
nxAuthorizedKeys <string> #ResourceName
{
    KeyComment = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Username = <string> ]
    [ Key = <string> ]
    [ DependsOn = <string[]> ]

}
```

## <a name="properties"></a>Eigenschaften

|  Eigenschaft |  Beschreibung |
|---|---|
| KeyComment| Ein eindeutiger Kommentar für den Schlüssel. Dieser wird verwendet, um Schlüssel eindeutig zu identifizieren.|
| Ensure| Gibt an, ob der Schlüssel definiert ist. Legen Sie diese Eigenschaft auf „Absent“ fest, um sicherzustellen, dass der Schlüssel nicht in der Datei mit den autorisierten Schlüsseln des Benutzers vorhanden ist. Legen Sie sie auf „Present“ fest, um sicherzustellen, dass der Schlüssel in der autorisierten Schlüsseldatei des Benutzers definiert ist.|
| Username| Der Benutzername, für den die autorisierten SSH-Schlüssel verwaltet werden sollen. Falls nicht definiert, ist der Standardbenutzer „root“.|
| Key| Der Inhalt des Schlüssels. Ist erforderlich, wenn **Ensure** auf „Present“ festgelegt ist.|
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die **ID** des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.|

## <a name="example"></a>Beispiel

Das folgende Beispiel definiert einen öffentlichen autorisierten SSH-Schlüssel für den Benutzer „monuser“.

```
Import-DSCResource -Module nx

Node $node {

nxSshAuthorizedKeys myKey{
   KeyComment = "myKey"
   Ensure = "Present"
   Key = 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEA0b+0xSd07QXRifm3FXj7Pn/DblA6QI5VAkDm6OivFzj3U6qGD1VJ6AAxWPCyMl/qhtpRtxZJDu/TxD8AyZNgc8aN2CljN1hOMbBRvH2q5QPf/nCnnJRaGsrxIqZjyZdYo9ZEEzjZUuMDM5HI1LA9B99k/K6PK2Bc1NLivpu7nbtVG2tLOQs+GefsnHuetsRMwo/+c3LtwYm9M0XfkGjYVCLO4CoFuSQpvX6AB3TedUy6NZ0iuxC0kRGg1rIQTwSRcw+McLhslF0drs33fw6tYdzlLBnnzimShMuiDWiT37WqCRovRGYrGCaEFGTG2e0CN8Co8nryXkyWc6NSDNpMzw== rsa-key-20150401'
   UserName = "monuser"
}
}
```