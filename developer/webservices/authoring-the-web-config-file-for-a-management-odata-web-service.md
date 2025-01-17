---
title: Erstellen die Datei "Web.config" für einen Management OData-Webdienst | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: d569f5d5-9746-40c0-be5e-f218bc4560f7
caps.latest.revision: 4
ms.openlocfilehash: f52953ee091f05df5f355719ecba788d3d5ee055
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62080712"
---
# <a name="authoring-the-webconfig-file-for-a-management-odata-web-service"></a>Erstellen der Web.config-Schemadatei für einen Management OData-Webdienst

Bevor Sie Ihre Management OData-Webdienst bereitstellen können, müssen Sie konfigurieren, dass die Datei "Web.config", zeigen Sie auf die XML-Schema-Dateien und DLLs, auf die Implementierung der [Microsoft.Management.Odata.Customauthorization](/dotnet/api/Microsoft.Management.Odata.CustomAuthorization) und [ System.Management.Automation.Remoting.Pssessionconfiguration](/dotnet/api/System.Management.Automation.Remoting.PSSessionConfiguration) Schnittstellen.

## <a name="sample-config-file"></a>Beispielkonfigurationsdatei

Folgendes ist ein Beispiel, wie die Datei "Web.config" für den Webdienst aussieht.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <configSections>
      <section name="managementOdata" type="Microsoft.Management.Odata.Core.DSConfiguration, Microsoft.Management.OData, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35, processorArchitecture=MSIL" />
   </configSections>
   <managementOdata schemaFileName="Schema.mof" resourceMappingFileName="Schema.xml">
      <customAuthorization type="Microsoft.Samples.Management.OData.RoleBasedPlugins.CustomAuthorization" assembly=".\Microsoft.Samples.Management.OData.RoleBasedPlugins.dll" />
      <powershell>
         <sessionConfiguration type="Microsoft.Samples.Management.OData.RoleBasedPlugins.SessionConfiguration" assembly=".\Microsoft.Samples.Management.OData.RoleBasedPlugins.dll" />
      </powershell>
      <commandInvocation enabled="true" />
      <wcfDataServicesConfig>
      </wcfDataServicesConfig>
   </managementOdata>
   <system.serviceModel>
      <behaviors>
         <serviceBehaviors>
            <behavior>
                  <serviceAuthorization serviceAuthorizationManagerType="Microsoft.Management.Odata.Core.CustomAuthorizationManager, Microsoft.Management.OData, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
            </behavior>
         </serviceBehaviors>
      </behaviors>
      <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
   </system.serviceModel>
   <system.webServer>
      <security>
         <authentication>
            <anonymousAuthentication enabled="false" />
            <basicAuthentication enabled="true" />
            <windowsAuthentication enabled="false" />
         </authentication>
      </security>
  </system.webServer>
</configuration>

```

## <a name="see-also"></a>Weitere Informationen

[Implementieren von benutzerdefinierten Autorisierung für einen Management OData-Webdienst](./implementing-custom-authorization-for-a-management-odata-web-service.md)

[Implementieren von SessionConfiguration für einen Management OData-Webdienst](./implementing-sessionconfiguration-for-a-management-odata-web-service.md)

[Erstellen der MOF-Schema-Datei für einen Management OData-Webdienst](./authoring-the-mof-schema-file-for-a-management-odata-web-service.md)

[Erstellung der XML-Schema-Datei für einen Management OData-Webdienst](./authoring-the-xml-schema-file-for-a-management-odata-web-service.md)

[Erstellen einen OData-Web-Management-Dienst](./creating-a-management-odata-web-service.md)