---
title: Insert-, Update- und Delete-Operationen
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 26a43a4f-83c9-4732-806d-bb23aad0ff6b
ms.openlocfilehash: 29d87ed22f987a09348cde446d2fd6b11a3c1528
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33360503"
---
# <a name="insert-update-and-delete-operations"></a>Insert-, Update- und Delete-Operationen
Sie führen die Operationen `Insert`, `Update` und `Delete` in [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] aus, indem Sie Objekte dem Objektmodell hinzufügen, diese ändern oder entfernen. Standardmäßig übersetzt [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] Ihre Aktionen in SQL und übergibt die Änderungen an die Datenbank.  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] bietet maximale Flexibilität beim Bearbeiten von und an Ihren Objekten vorgenommenen Änderungen beibehalten. Sobald Entitätsobjekte zur Verfügung stehen (entweder durch Abrufen in einer Abfrage oder durch Neuzusammenstellung), können Sie diese wie typische Objekte in Ihrer Anwendung ändern. D. h. Sie deren Werte ändern, Auflistungen hinzufügen und können sie aus Ihren Sammlungen entfernt werden. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] verfolgt Ihre Änderungen und kann diese zurück in die Datenbank übertragen, wenn Sie <xref:System.Data.Linq.DataContext.SubmitChanges%2A> aufrufen.  
  
> [!NOTE]
>  Kaskadierte Löschvorgänge werden von [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] nicht unterstützt bzw. erkannt. Wenn eine Zeile in einer Tabelle zu löschen, die Einschränkungen gelten sollen, müssen Sie entweder die `ON DELETE CASCADE` -Regel in der foreign Key-Einschränkung in der Datenbank, oder verwenden Sie eigenen Code zunächst die untergeordneten Objekte löschen, die verhindern, dass das übergeordnete Objekt gelöscht werden. Andernfalls wird eine Ausnahme ausgelöst. Weitere Informationen finden Sie unter [Vorgehensweise: Löschen von Zeilen aus der Datenbank](../../../../../../docs/framework/data/adonet/sql/linq/how-to-delete-rows-from-the-database.md).  
  
 Die folgenden Auszüge verwenden die `Customer`-Klasse und die `Order`-Klasse aus der Beispieldatenbank Northwind. Klassendefinitionen werden zur besseren Übersicht nicht angezeigt.  
  
 [!code-csharp[DLinqCRUDOps#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCRUDOps/cs/Program.cs#1)]
 [!code-vb[DLinqCRUDOps#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCRUDOps/vb/Module1.vb#1)]  
  
 Wenn Sie <xref:System.Data.Linq.DataContext.SubmitChanges%2A> aufrufen, übernimmt [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] automatisch die Erzeugung und Ausführung der SQL-Befehle, die zur Übertragung Ihrer Änderungen in die Datenbank erforderlich sind.  
  
> [!NOTE]
>  Sie können dieses Verhalten überschreiben, indem Sie Ihre eigene Logik verwenden (typischerweise in Form einer gespeicherten Prozedur). Weitere Informationen finden Sie unter [Aufgaben der Entwickler beim Überschreiben von Standardverhalten](../../../../../../docs/framework/data/adonet/sql/linq/responsibilities-of-the-developer-in-overriding-default-behavior.md).  
>   
>  Entwickler, die mit Visual Studio können mithilfe der [!INCLUDE[vs_ordesigner_long](../../../../../../includes/vs-ordesigner-long-md.md)] gespeicherte Prozeduren zu diesem Zweck entwickeln.  
  
## <a name="see-also"></a>Siehe auch  
 [Downloading Sample Databases (Herunterladen von Beispieldatenbanken)](../../../../../../docs/framework/data/adonet/sql/linq/downloading-sample-databases.md)  
 [Anpassen von Insert-, Update- und Delete-Operationen](../../../../../../docs/framework/data/adonet/sql/linq/customizing-insert-update-and-delete-operations.md)
