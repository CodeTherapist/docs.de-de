---
title: end of-Anweisung erwartet.
ms.date: 07/20/2015
f1_keywords:
- bc30205
- vbc30205
helpviewer_keywords:
- BC30205
ms.assetid: 53c7f825-a737-4b76-a1fa-f67745b8bd40
ms.openlocfilehash: 8df756009ebe3a0613ec47018d938151829214df
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2018
ms.locfileid: "50198262"
---
# <a name="end-of-statement-expected"></a>end of-Anweisung erwartet.
Die Anweisung syntaktisch abgeschlossen ist, aber eine zusätzliche Programmierelement folgt auf das Element, das die Anweisung abgeschlossen ist. Ein Zeilenabschlusszeichen ist am Ende jeder Anweisung erforderlich.
  
 Ein Zeilenabschlusszeichen teilt die Zeichen einer Visual Basic-Quelldatei in Zeilen. Beispiele für Zeilenabschlusszeichen sind, die Unicode-Carriage return Zeichen (& HD), die Unicode-Zeilenvorschub-Zeichen (& HA), und Unicode-Wagenrücklaufzeichen gefolgt von dem Unicode-Zeichen für Zeilenvorschub. Weitere Informationen zu Zeilenabschlusszeichen, finden Sie unter den [Visual Basic-Sprachspezifikation](~/_vblang/spec/lexical-grammar.md#line-terminators).
  
 **Fehler-ID:** BC30205
  
## <a name="to-correct-this-error"></a>So beheben Sie diesen Fehler
  
1.  Überprüfen Sie, wenn zwei verschiedene Anweisungen in der gleichen Zeile versehentlich gestellt wurden.
  
2.  Fügen Sie einem Zeilenabschluss, nach das Element, das die Anweisung abgeschlossen ist.
  
## <a name="see-also"></a>Siehe auch  
 [Gewusst wie: Umbrechen und Zusammenfassen von Anweisungen in Code](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)  
 [Anweisungen](../../../visual-basic/programming-guide/language-features/statements.md)
