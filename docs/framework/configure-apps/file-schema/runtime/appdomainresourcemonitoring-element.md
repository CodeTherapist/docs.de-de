---
title: '&lt;AppDomainResourceMonitoring&gt; Element'
ms.date: 03/30/2017
helpviewer_keywords:
- appDomainResourceMonitoring element
- <appDomainResourceMonitoring> element
ms.assetid: 02119ab6-1e91-448e-97ad-e7b2e5c4bbbd
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 11e892d8ab9001d3670c801b43ba444aa24b2e41
ms.sourcegitcommit: 11f11ca6cefe555972b3a5c99729d1a7523d8f50
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="ltappdomainresourcemonitoringgt-element"></a>&lt;AppDomainResourceMonitoring&gt; Element
Weist die Runtime zum Sammeln von Statistiken für alle Anwendungsdomänen im Prozess für die Lebensdauer des Prozesses an.  
  
 \<configuration>  
\<Common Language Runtime >  
\<AppDomainResourceMonitoring >  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<appDomainResourceMonitoring    
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|  
|---------------|-----------------|  
|`enabled`|Erforderliches Attribut.<br /><br /> Gibt an, ob die Common Language Runtime Statistiken für die ressourcenüberwachung der Anwendungsdomäne erfasst.|  
  
## <a name="enabled-attribute"></a>Enabled-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|`true`|Statistiken für die ressourcenüberwachung der Anwendungsdomäne werden gesammelt.|  
|`false`|Statistiken für die ressourcenüberwachung der Anwendungsdomäne werden nicht erfasst.|  
  
### <a name="child-elements"></a>Untergeordnete Elemente  
 Keine  
  
### <a name="parent-elements"></a>Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|-----------------|  
|`configuration`|Das Stammelement in jeder von den Common Language Runtime- und .NET Framework-Anwendungen verwendeten Konfigurationsdatei.|  
|`runtime`|Enthält Informationen über die Assemblybindung und die Garbage Collection.|  
  
## <a name="remarks"></a>Hinweise  
 Ressourcenüberwachung der Anwendungsdomäne ist verfügbar, über die verwaltete Anwendung Domänenklasse, das hosting [ICLRAppDomainResourceMonitor](../../../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md) Schnittstelle und ereignisablaufverfolgung für Windows (ETW). Wenn die Überwachung aktiviert ist, werden Statistiken für alle Anwendungsdomänen im Prozess für die Lebensdauer des Prozesses gesammelt.  
  
 Verwenden Sie zum Aktivieren der Überwachung von verwaltetem Code die <xref:System.AppDomain.MonitoringIsEnabled%2A> Eigenschaft.  
  
 Dieses Konfigurationselement steht nur in der [!INCLUDE[net_v40_long](../../../../../includes/net-v40-long-md.md)] und höher.  
  
## <a name="example"></a>Beispiel  
 Im folgende Beispiel wird gezeigt, wie die ressourcenüberwachung der Anwendungsdomäne aktiviert.  
  
```xml  
<configuration>  
   <runtime>  
      <appDomainResourceMonitoring enabled="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>  
 [Schema für Laufzeiteinstellungen](../../../../../docs/framework/configure-apps/file-schema/runtime/index.md)  
 [Konfigurationsdateischema](../../../../../docs/framework/configure-apps/file-schema/index.md)
