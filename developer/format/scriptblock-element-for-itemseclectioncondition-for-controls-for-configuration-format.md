---
title: ScriptBlock-Element für ItemSeclectionCondition für Steuerelemente für die Konfiguration (Format) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 51f7aec9-7b01-4370-84f4-1e58508a851f
caps.latest.revision: 6
ms.openlocfilehash: e92b2dfff07358132c480c47c34279e5365fe400
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62064440"
---
# <a name="scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format"></a>Element „ScriptBlock“ für ItemSeclectionCondition für Controls für Configuration (Format)

Gibt das Skript an, das die Bedingung auslöst. Wenn dieses Skript ergab `true`, die Bedingung ist erfüllt, und das Steuerelement wird verwendet. Dieses Element wird verwendet, wenn ein allgemeines Steuerelement zu definieren, das von allen Ansichten im die Formatierungsdatei verwendet werden kann.

Konfigurationselement (Format)-Element-Steuerelemente (Format)-Steuerelement Konfigurationselements für Steuerelemente für (Format) CustomControl Konfigurationselement für Steuerelement (Format) CustomEntries Konfigurationselement für CustomControl für Konfiguration ( CustomEntry-Element für CustomControl für Steuerelemente für (Format) CustomItem Konfigurationselement für CustomEntry für Steuerelemente für die ExpressionBinding-Konfigurationselement für CustomItem für Steuerelemente für die Konfiguration (Format)-Format) ItemSelectionCondition-Element für die ExpressionBinding für Steuerelemente für Konfiguration (Format)-ScriptBlock-Element für ItemSelectionCondition für Steuerelemente für die Konfiguration (Format)

## <a name="syntax"></a>Syntax

```xml
<ScriptBlock>ScriptToEvaluate</ScriptBlock>
```

## <a name="attributes-and-elements"></a>Attribute und Elemente

Den folgenden Abschnitten werden Attribute, untergeordnete Elemente und das übergeordnete Element des der `ScriptBlock` Element.

### <a name="attributes"></a>Attributes

Keine.

### <a name="child-elements"></a>Untergeordnete Elemente

Keine.

### <a name="parent-elements"></a>Übergeordnete Elemente

|Element|Beschreibung|
|-------------|-----------------|
|[ItemSelectionCondition-Element für die ExpressionBinding für Steuerelemente für die Konfiguration (Format)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)|Definiert die Bedingung, die für dieses Steuerelement verwendet werden, vorhanden sein muss.|

## <a name="text-value"></a>Textwert

Geben Sie das Skript aus, das ausgewertet wird.

## <a name="remarks"></a>Hinweise

Wenn dieses Element verwendet wird, Sie können nicht angeben, die [PropertyName](./propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format.md) -Element, wenn die auswahlbedingung zu definieren.

## <a name="see-also"></a>Weitere Informationen

[PropertyName-Element für ItemSeclectionCondition für Steuerelemente für die Konfiguration (Format)](./propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)

[ItemSelectionCondition-Element für die ExpressionBinding für Steuerelemente für die Konfiguration (Format)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)

[Schreiben Sie eine Formatierungsdatei PowerShell](./writing-a-powershell-formatting-file.md)
