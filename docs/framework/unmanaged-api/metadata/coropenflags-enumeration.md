---
title: CorOpenFlags-Enumeration
ms.date: 03/30/2017
api_name:
- CorOpenFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorOpenFlags
helpviewer_keywords:
- CorOpenFlags enumeration [.NET Framework metadata]
ms.assetid: e27a83b5-2698-4996-9032-1e0fed8b91ca
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: c08b1f6be41de63886115e5aed6bcad901658bb5
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43509219"
---
# <a name="coropenflags-enumeration"></a>CorOpenFlags-Enumeration
Enthält Flagwerte, die das Verhalten von Metadaten beim Öffnen von Manifestdateien steuern.  
  
## <a name="syntax"></a>Syntax  
  
```  
typedef enum CorOpenFlags  
{  
    ofRead              =   0x00000000,  
    ofWrite             =   0x00000001,  
    ofReadWriteMask     =   0x00000001,  
    ofCopyMemory        =   0x00000002,  
    ofCacheImage        =   0x00000004,  
    ofManifestMetadata  =   0x00000008,  
    ofReadOnly          =   0x00000010,  
    ofTakeOwnership     =   0x00000020,  
    ofCacheImage        =   0x00000004,  
    ofNoTypeLib         =   0x00000080,  
    ofNoTransform       =   0x00001000,  
    ofReserved1         =   0x00000100,  
    ofReserved2         =   0x00000200,  
    ofReserved          =   0xffffff40  
} CorOpenFlags;  
```  
  
## <a name="members"></a>Member  
  
|Member|Beschreibung|  
|------------|-----------------|  
|`ofRead`|Gibt an, dass die Datei nur zum Lesen geöffnet werden darf.|  
|`ofWrite`|Gibt an, dass die Datei nur zum Schreiben geöffnet werden darf.<br /><br /> Wenn Sie beim Öffnen einer .winmd-Datei das `ofWrite`-Flag verwenden, sollten Sie auch das `ofNoTransform`-Flag übergeben.|  
|`ofReadWriteMask`|Eine Maske zum Lesen und Schreiben.|  
|`ofCopyMemory`|Gibt an, dass die Datei in den Arbeitsspeicher gelesen werden muss. Metadaten müssen eine eigene Kopie verwalten.|  
|`ofCacheImage`|Veraltet. Dieses Flag wird ignoriert.|  
|`ofManifestMetadata`|Veraltet. Dieses Flag wird ignoriert.|  
|`ofReadOnly`|Gibt an, dass die Datei zum Lesen und, die geöffnet werden darf einen Aufruf von `QueryInterface` für eine [IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md) kann nicht hergestellt werden.|  
|`ofTakeOwnership`|Gibt an, dass der Speicher belegt wurde über einen Aufruf an [CoTaskMemAlloc](/windows/desktop/api/combaseapi/nf-combaseapi-cotaskmemalloc) und von den Metadaten freigegeben wird.|  
|`ofNoTypeLib`|Veraltet. Dieses Flag wird ignoriert.|  
|`ofNoTransform`|Gibt an, dass automatische Umwandlungen von .winmd-Dateien deaktiviert sein sollten. Anders ausgedrückt: Die Projektion eines Windows Runtime-Typs auf einen .NET Framework-Typ sollte deaktiviert sein. Weitere Informationen finden Sie unter [darunter das System mit .NET und die Windows-Runtime](https://msdn.microsoft.com/magazine/jj651569.aspx).|  
|`ofReserved1`|Für die interne Verwendung vorgesehen.|  
|`ofReserved2`|Für die interne Verwendung vorgesehen.|  
|`ofReserved`|Für die interne Verwendung vorgesehen.|  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorHdr.h  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Siehe auch  
 [Metadatenenumerationen](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
