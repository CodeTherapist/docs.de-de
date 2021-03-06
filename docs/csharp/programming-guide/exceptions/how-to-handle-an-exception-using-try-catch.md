---
title: 'Vorgehensweise: Behandeln einer Ausnahme mit try/catch (C#-Programmierhandbuch)'
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], try/catch blocks
- exceptions [C#], try/catch blocks
- try/catch blocks [C#]
ms.assetid: ca8e3773-980e-4767-8633-7408540e9818
ms.openlocfilehash: 74503c510007b132a7bbb14da7eade4c379b2179
ms.sourcegitcommit: 3c1c3ba79895335ff3737934e39372555ca7d6d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43856577"
---
# <a name="how-to-handle-an-exception-using-trycatch-c-programming-guide"></a>Gewusst wie: Behandeln einer Ausnahme mit try/catch (C#-Programmierhandbuch)
Ein [try/catch](../../../csharp/language-reference/keywords/try-catch.md)-Block soll von Arbeitscode generierte Ausnahmen abfangen und verarbeiten. Einige Ausnahmen können in einem `catch`-Block verarbeitet werden, und das Problem kann behoben werden, ohne dass die Ausnahme erneut ausgelöst wird. Allerdings können Sie meistens nur sicherstellen, dass die passende Ausnahme ausgelöst wird.  
  
## <a name="example"></a>Beispiel  
 In diesem Beispiel ist <xref:System.IndexOutOfRangeException> nicht die passendste Ausnahme: Für diese Methode ist <xref:System.ArgumentOutOfRangeException> sinnvoller, da der Fehler durch die Übergabe des Arguments `index` des Aufrufers verursacht wird.  
  
 [!code-csharp[csProgGuideExceptions#5](../../../csharp/programming-guide/exceptions/codesnippet/CSharp/how-to-handle-an-exception-using-try-catch_1.cs)]  
  
## <a name="comments"></a>Kommentare  
 Der Code, der eine Ausnahme auslöst, ist im `try`-Block eingeschlossen. Eine `catch`-Anweisung wird direkt danach hinzugefügt, um `IndexOutOfRangeException` zu verarbeiten, falls dies auftritt. Der `catch`-Block verarbeitet `IndexOutOfRangeException` und löst stattdessen die passendere Ausnahme `ArgumentOutOfRangeException` aus. Um dem Aufrufer so viele Informationen wie möglich bereitzustellen, können Sie die ursprüngliche Ausnahme auf <xref:System.Exception.InnerException%2A> der neuen Ausnahme festlegen. Da die Eigenschaft <xref:System.Exception.InnerException%2A> [schreibgeschützt](../../../csharp/language-reference/keywords/readonly.md) ist, müssen Sie sie im Konstruktor der neuen Ausnahme zuweisen.  
  
## <a name="see-also"></a>Siehe auch

- [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)  
- [Ausnahmen und Ausnahmebehandlung](../../../csharp/programming-guide/exceptions/index.md)  
- [Ausnahmebehandlung](../../../csharp/programming-guide/exceptions/exception-handling.md)
