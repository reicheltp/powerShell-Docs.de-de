---
title: HelpInfo-XML-Beispieldatei | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/12/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 6544070f-5549-407f-8603-5df60fe9e013
caps.latest.revision: 7
ms.openlocfilehash: 11804db56ec47554e82f04fe6954920ad9577370
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62082429"
---
# <a name="helpinfo-xml-sample-file"></a>XML-Beispieldatei HelpInfo

In diesem Thema zeigt ein Beispiel für eine wohlgeformte aktualisierbare Hilfe-Informationen-Datei, die so genannte "HelpInfo XML-Datei". In dieser Beispieldatei werden die Benutzeroberflächenelemente für die Kultur durch den Namen der Benutzeroberflächenkultur in alphabetischer Reihenfolge angeordnet. Alphabetische Sortierung hat sich bewährt, aber es ist nicht erforderlich.

## <a name="helpinfo-xml-sample-file"></a>XML-Beispieldatei HelpInfo

```xml

<?xml version="1.0" encoding="utf-8"?>
<HelpInfo xmlns="http://schemas.microsoft.com/powershell/help/2010/05">
   <HelpContentURI>http://go.microsoft.com/fwlink/?LinkID=141553</HelpContentURI>
   <SupportedUICultures>
    <UICulture>
      <UICultureName>de-DE</UICultureName>
      <UICultureVersion>2.15.0.10</UICultureVersion>
    </UICulture>
    <UICulture>
      <UICultureName>en-US</UICultureName>
      <UICultureVersion>3.2.0.7</UICultureVersion>
    </UICulture>
    <UICulture>
      <UICultureName>it-IT</UICultureName>
      <UICultureVersion>1.1.0.5</UICultureVersion>
    </UICulture>
    <UICulture>
      <UICultureName>ja-JP</UICultureName>
      <UICultureVersion>3.2.0.4</UICultureVersion>
    </UICulture>
   </SupportedUICultures>
</HelpInfo>

```