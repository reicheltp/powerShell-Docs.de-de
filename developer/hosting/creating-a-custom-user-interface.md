---
title: Erstellen einer benutzerdefinierten Benutzeroberfläche | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: d7286443-eed4-43d5-b809-50cdcdcba088
caps.latest.revision: 4
ms.openlocfilehash: 23518c625fe1138e1bd2bcc895274cb21d7daf8a
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62083041"
---
# <a name="creating-a-custom-user-interface"></a>Erstellen einer benutzerdefinierten Benutzeroberfläche

Windows PowerShell bietet abstrakter Klassen und Schnittstellen, mit denen Sie eine benutzerdefinierte interaktive Benutzeroberfläche zu erstellen, die Windows PowerShell-Engine hostet. Um eine benutzerdefinierte Benutzeroberfläche erstellen möchten, müssen Sie implementieren die [System.Management.Automation.Host.PSHost](/dotnet/api/System.Management.Automation.Host.PSHost) Klasse. Optional können Sie auch implementieren die [System.Management.Automation.Host.Pshostrawuserinterface](/dotnet/api/System.Management.Automation.Host.PSHostRawUserInterface)und [System.Management.Automation.Host.Pshostuserinterface](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface) Klassen und die [System.Management.Automation.Host.Ihostsupportsinteractivesession](/dotnet/api/System.Management.Automation.Host.IHostSupportsInteractiveSession) und [System.Management.Automation.Host.Ihostuisupportsmultiplechoiceselection](/dotnet/api/System.Management.Automation.Host.IHostUISupportsMultipleChoiceSelection) Schnittstellen.
