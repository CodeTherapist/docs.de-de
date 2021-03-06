---
title: 'Gewusst wie: Abrufen der Adresse einer Variablen (C#-Programmierhandbuch)'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [C#], address of
- pointers [C#], & operator
- pointer expressions [C#], address-of operator
ms.assetid: 44fe2cd9-a64f-4ef5-be2a-09ce807c0182
ms.openlocfilehash: 40a7ac34a4e68df7aa316adc3cbd1999d975eabe
ms.sourcegitcommit: 3c1c3ba79895335ff3737934e39372555ca7d6d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43741873"
---
# <a name="how-to-obtain-the-address-of-a-variable-c-programming-guide"></a>Gewusst wie: Abrufen der Adresse einer Variablen (C#-Programmierhandbuch)
Verwenden Sie den address-of-Operator `&` zum Abrufen der Adresse eines unären Ausdrucks, der eine feste Variable ergibt:  
  
```csharp  
int number;  
int* p = &number; //address-of operator &  
```  
  
 Der address-of-Operator kann nur auf eine Variable angewendet werden. Wenn die Variable beweglich ist, können Sie eine [feste Anweisung](../../../csharp/language-reference/keywords/fixed-statement.md) verwenden, um die Variable vorübergehen zu befestigen, um ihre Adresse abzurufen.  
  
 Sie müssen sicherstellen, dass die Variable initialisiert wird. Der Compiler gibt keine Fehlermeldung aus, wenn die Variable nicht initialisiert ist.  
  
 Sie können nicht die Adresse einer Konstanten oder eines Werts abrufen.  
  
## <a name="example"></a>Beispiel  
 In diesem Beispiel wird ein Zeiger auf `int`, `p`, deklariert. Die Adresse einer ganzzahligen Variable, `number`, wird ihm zugewiesen. Die Variable `number` wird als Ergebnis der Zuweisung zu `*p` initialisiert. Wenn Sie diese Zuweisungsanweisung auskommentieren, wird die Initialisierung der Variable `number` entfernt, jedoch wird kein Kompilierzeitfehler ausgegeben.  

> [!NOTE]
> Kompilieren Sie dieses Beispiel mit der Compileroption [`-unsafe`](../../language-reference/compiler-options/unsafe-compiler-option.md).
  
 [!code-csharp[address-of-a-variable](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers.cs#8)]  
  
## <a name="see-also"></a>Siehe auch

- [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)  
- [Zeigerausdrücke](../../../csharp/programming-guide/unsafe-code-pointers/pointer-expressions.md)  
- [Zeigertypen](../../../csharp/programming-guide/unsafe-code-pointers/pointer-types.md)  
- [Typen](../../../csharp/language-reference/keywords/types.md)  
- [unsafe](../../../csharp/language-reference/keywords/unsafe.md)  
- [fixed-Anweisung](../../../csharp/language-reference/keywords/fixed-statement.md)  
- [stackalloc](../../../csharp/language-reference/keywords/stackalloc.md)
