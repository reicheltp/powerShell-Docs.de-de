---
title: Anmeldeinformationen Attributdeklaration | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 96a5dcad-faed-44d8-8c80-321f10499710
caps.latest.revision: 6
ms.openlocfilehash: 49a62ccb09f06f77862d4737199e58293e7fbe0a
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62068316"
---
# <a name="credential-attribute-declaration"></a>Attributdeklaration: Credential

Die Anmeldeinformationen-Attribut ist ein optionales Attribut, das mit dem Credential-Parameter des Typs verwendet werden kann [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) , damit eine Zeichenfolge auch als Argument an den Parameter übergeben werden kann. Wenn dieses Attribut auf eine Parameterdeklaration hinzugefügt wird, konvertiert Windows PowerShell die Zeichenfolgeneingabe in einem [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) Objekt. Z. B. die [Get-Credential](/powershell/module/Microsoft.PowerShell.Security/Get-Credential) Cmdlet verwendet dieses Attribut Generieren von Windows PowerShell werden der [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) -Objekt, das vom Cmdlet zurückgegeben wird.

## <a name="syntax"></a>Syntax

```csharp
[Credential]
```

## <a name="remarks"></a>Hinweise

- Dieses Attribut wird in der Regel verwendet, durch die Parameter des Typs [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) , damit eine Zeichenfolge auch als Argument an den Parameter übergeben werden kann. Wenn eine [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) Objekt wird an den Parameter übergeben, die Windows PowerShell führt keine Aktion aus.

- Beim Erstellen der [System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential) Objekt ist, wird Windows PowerShell den aktuellen Host verwendet, um die entsprechenden Anweisungen für den Benutzer anzuzeigen. Beispielsweise zeigt den Standardwert Host eine Eingabeaufforderung für einen Benutzernamen und ein Kennwort, wenn dieses Attribut verwendet wird. Jedoch wenn Sie ein benutzerdefinierter Host verwendet wird, die eine andere Eingabeaufforderung definiert, und Sie würden diese Aufforderung angezeigt werden.

- Dieses Attribut wird mit dem Parameter-Attribut verwendet. Weitere Informationen zu diesem Attribut finden Sie unter [Parameter Attributdeklaration](./parameter-attribute-declaration.md).

- Das Credential-Attribut definiert ist, durch die [System.Management.Automation.Credentialattribute](/dotnet/api/System.Management.Automation.CredentialAttribute) Klasse.

## <a name="see-also"></a>Weitere Informationen

[Parameteraliase](./parameter-aliases.md)

[Parameterdeklaration-Attribut](./parameter-attribute-declaration.md)

[Schreiben eines Windows PowerShell-Cmdlets](./writing-a-windows-powershell-cmdlet.md)
