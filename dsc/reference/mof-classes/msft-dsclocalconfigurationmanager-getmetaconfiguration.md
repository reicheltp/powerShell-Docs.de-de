---
ms.date: 06/12/2017
keywords: dsc,powershell,configuration,setup
title: GetMetaConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse
ms.openlocfilehash: e14237ef68b95d68e2f14071aa1fa6ba0717f39f
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62078519"
---
# <a name="getmetaconfiguration-method-of-the-msftdsclocalconfigurationmanager-class"></a>GetMetaConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse

Ruft die Einstellungen des lokalen Konfigurations-Managers ab, die zur Steuerung des Konfigurations-Agents verwendet werden.

## <a name="syntax"></a>Syntax

```mof
uint32 GetMetaConfiguration(
  [out] MSFT_DSCMetaConfiguration MetaConfiguration
);
```

## <a name="parameters"></a>Parameter

*MetaConfiguration* \[out\] Enthält bei der Rückgabe eine eingebettete Instanz der **MSFT_DSCMetaConfiguration**-Klasse, die die Einstellungen definiert.

## <a name="return-value"></a>Rückgabewert

Gibt bei Erfolg null zurück, andernfalls einen Fehlercode.

## <a name="remarks"></a>Hinweise

Dies ist eine statische Methode.

## <a name="requirements"></a>Anforderungen

**MOF:** DscCore.mof

**Namespace**: Root\Microsoft\Windows\DesiredStateConfiguration

## <a name="see-also"></a>Siehe auch

[**MSFT_DSCLocalConfigurationManager-Klasse**](msft-dsclocalconfigurationmanager.md)