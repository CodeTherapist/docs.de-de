---
title: ICorDebugMDA::GetDescription-Methode
ms.date: 03/30/2017
api_name:
- ICorDebugMDA.GetDescription
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMDA::GetDescription
helpviewer_keywords:
- GetDescription method [.NET Framework debugging]
- ICorDebugMDA::GetDescription method [.NET Framework debugging]
ms.assetid: 01d1b481-ca67-4712-8744-d342ec0df639
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2fdace527194228dd6004a991950a80d23275650
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33413295"
---
# <a name="icordebugmdagetdescription-method"></a>ICorDebugMDA::GetDescription-Methode
Ruft eine Zeichenfolge, enthält die Beschreibung der der Assistent für verwaltetes Debuggen (MDA) dargestellte [ICorDebugMDA](../../../../docs/framework/unmanaged-api/debugging/icordebugmda-interface.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
HRESULT GetDescription (  
    [in] ULONG32   cchName,  
    [out] ULONG32  *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
        WCHAR      szName[]  
);  
```  
  
#### <a name="parameters"></a>Parameter  
 `cchName`  
 [in] Die Größe des Zeichenfolgenpuffers, in denen die Beschreibung gespeichert wird.  
  
 `pcchName`  
 [out] Ein Zeiger auf die Anzahl der Bytes im Zeichenfolgenpuffer zurückgegeben.  
  
 `szName`  
 [out] Einen Zeichenfolgenpuffer, enthält die Beschreibung des MDA.  
  
## <a name="remarks"></a>Hinweise  
 Die Zeichenfolge kann 0 (null) lang sein.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorDebug.idl, CorDebug.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Siehe auch  
 [ICorDebugMDA-Schnittstelle](../../../../docs/framework/unmanaged-api/debugging/icordebugmda-interface.md)  
 [Diagnosing Errors with Managed Debugging Assistants (Diagnostizieren von Fehlern mit Assistenten für verwaltetes Debuggen)](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
