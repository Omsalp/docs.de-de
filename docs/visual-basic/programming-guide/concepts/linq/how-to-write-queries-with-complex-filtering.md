---
title: 'Gewusst wie: Schreiben von Abfragen mit komplexer Filterung (Visual Basic) | Microsoft-Dokumentation'
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: bf286ffc-7990-4b00-a4eb-ee3d70129950
caps.latest.revision: 3
author: dotnet-bot
ms.author: dotnetcontent
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: e46d07674d901aef77db04d63314080a4ca68801
ms.lasthandoff: 03/13/2017


---
# <a name="how-to-write-queries-with-complex-filtering-visual-basic"></a>Gewusst wie: Schreiben von Abfragen mit komplexer Filterung (Visual Basic)
Es kann vorkommen, dass Sie LINQ to XML-Abfragen mit komplexen Filtern schreiben möchten. Vielleicht möchten Sie z. B. auf diese Weise nach allen Elementen suchen, die ein untergeordnetes Element mit einem bestimmten Namen und einem bestimmten Wert besitzen. In diesem Thema finden Sie ein Beispiel für das Schreiben von Fragen mit komplexer Filterung.  
  
## <a name="example"></a>Beispiel  
 Dieses Beispiel zeigt, wie Sie nach allen `PurchaseOrder`-Elementen suchen können, die ein untergeordnetes `Address`-Element mit einem `Type`-Attribut "Shipping" und einem untergeordneten `State`-Element "NY" besitzen. Das Beispiel verwendet eine geschachtelte Abfrage in der `Where`-Klausel, und der `Any`-Operator gibt `True` zurück, sofern die Auflistung überhaupt Elemente enthält.  
  
 Dieses Beispiel verwendet die folgende XML-Dokument: [XML-Beispieldatei: mehrere Bestellungen (LINQ to XML)](../../../../visual-basic/programming-guide/concepts/linq/sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
 Weitere Informationen zu den `Any` Operator finden Sie unter [Quantifizierer-Vorgänge (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/quantifier-operations.md).  
  
```vb  
Dim root As XElement = XElement.Load("PurchaseOrders.xml")  
Dim purchaseOrders As IEnumerable(Of XElement) = _  
    From el In root.<PurchaseOrder> _  
    Where _  
        (From add In el.<Address> _  
        Where _  
             add.@Type = "Shipping" And _  
             add.<State>.Value = "NY" _  
        Select add) _  
    .Any() _  
    Select el  
For Each el As XElement In purchaseOrders  
    Console.WriteLine(el.@PurchaseOrderNumber)  
Next  
```  
  
 Dieser Code erzeugt die folgende Ausgabe:  
  
```  
99505  
```  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird dieselbe Abfrage für XML in einem Namespace gezeigt. Weitere Informationen finden Sie unter [arbeiten mit XML-Namespaces (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/working-with-xml-namespaces.md).  
  
 Dieses Beispiel verwendet die folgende XML-Dokument: [XML-Beispieldatei: mehrere Bestellungen in einem Namespace](../../../../visual-basic/programming-guide/concepts/linq/sample-xml-file-multiple-purchase-orders-in-a-namespace.md).  
  
```vb  
Imports <xmlns:aw='http://www.adventure-works.com'>  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = XElement.Load("PurchaseOrdersInNamespace.xml")  
        Dim purchaseOrders As IEnumerable(Of XElement) = _  
            From el In root.<aw:PurchaseOrder> _  
            Where _  
                (From add In el.<aw:Address> _  
                Where _  
                     add.@aw:Type = "Shipping" And _  
                     add.<aw:State>.Value = "NY" _  
                Select add) _  
            .Any() _  
            Select el  
        For Each el As XElement In purchaseOrders  
            Console.WriteLine(el.@aw:PurchaseOrderNumber)  
        Next  
    End Sub  
End Module  
```  
  
 Dieser Code erzeugt die folgende Ausgabe:  
  
```  
99505  
```  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.Xml.Linq.XElement.Attribute%2A></xref:System.Xml.Linq.XElement.Attribute%2A>   
 <xref:System.Xml.Linq.XContainer.Elements%2A></xref:System.Xml.Linq.XContainer.Elements%2A>   
 [Standardabfragen (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/basic-queries-linq-to-xml.md)   
 [Untergeordnete XML-Achseneigenschaft](../../../../visual-basic/language-reference/xml-axis/xml-child-axis-property.md)   
 [XML-Attributachseneigenschaft](../../../../visual-basic/language-reference/xml-axis/xml-attribute-axis-property.md)   
 [XML-Value-Eigenschaft](../../../../visual-basic/language-reference/xml-axis/xml-value-property.md)   
 [Projektionsvorgänge (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/projection-operations.md)   
 [Quantifizierer-Vorgänge (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/quantifier-operations.md)
