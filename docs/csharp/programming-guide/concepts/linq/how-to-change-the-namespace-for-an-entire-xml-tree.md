---
title: "Vorgehensweise: Ändern des Namespaces für einen gesamten XML-Baum (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 1584ff3b-c77d-4241-ab62-80adfb7bfc1b
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: ffebf1bda275eb815ff3e15538fd69de6d3b0c1a
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-change-the-namespace-for-an-entire-xml-tree-c"></a>Vorgehensweise: Ändern des Namespaces für einen gesamten XML-Baum (C#)
Es kann passieren, dass Sie den Namespace für ein Element oder Attribut programmgesteuert ändern müssen. Mit LINQ to XML ist dies ganz einfach. Sie können die <xref:System.Xml.Linq.XElement.Name%2A?displayProperty=fullName>-Eigenschaft festlegen. Die <xref:System.Xml.Linq.XAttribute.Name%2A?displayProperty=fullName>-Eigenschaft kann nicht festgelegt werden, Sie können aber problemlos die Attribute in eine <xref:System.Collections.Generic.List%601?displayProperty=fullName> kopieren, die vorhandenen Attribute entfernen und dann neue Attribute hinzufügen, die sich im neuen gewünschten Namespace befinden.  
  
 Weitere Informationen finden Sie unter [Working with XML Namespaces (C#) (Arbeiten mit XML-Namespaces (C#))](../../../../csharp/programming-guide/concepts/linq/working-with-xml-namespaces.md).  
  
## <a name="example"></a>Beispiel  
 Der folgende Code erstellt zwei XML-Strukturen, die sich in keinem Namespace befinden. Er ändert dann den Namespace beider Strukturen und fasst die Strukturen in einer gemeinsamen Struktur zusammen.  
  
```csharp  
XElement tree1 = new XElement("Data",  
    new XElement("Child", "content",  
        new XAttribute("MyAttr", "content")  
    )  
);  
XElement tree2 = new XElement("Data",  
    new XElement("Child", "content",  
        new XAttribute("MyAttr", "content")  
    )  
);  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace ad = "http://www.adatum.com";  
// change the namespace of every element and attribute in the first tree  
foreach (XElement el in tree1.DescendantsAndSelf())  
{  
    el.Name = aw.GetName(el.Name.LocalName);  
    List<XAttribute> atList = el.Attributes().ToList();  
    el.Attributes().Remove();  
    foreach (XAttribute at in atList)  
        el.Add(new XAttribute(aw.GetName(at.Name.LocalName), at.Value));  
}  
// change the namespace of every element and attribute in the second tree  
foreach (XElement el in tree2.DescendantsAndSelf())  
{  
    el.Name = ad.GetName(el.Name.LocalName);  
    List<XAttribute> atList = el.Attributes().ToList();  
    el.Attributes().Remove();  
    foreach (XAttribute at in atList)  
        el.Add(new XAttribute(ad.GetName(at.Name.LocalName), at.Value));  
}  
// add attribute namespaces so that the tree will be serialized with  
// the aw and ad namespace prefixes  
tree1.Add(  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com")  
);  
tree2.Add(  
    new XAttribute(XNamespace.Xmlns + "ad", "http://www.adatum.com")  
);  
// create a new composite tree  
XElement root = new XElement("Root",  
    tree1,  
    tree2  
);  
Console.WriteLine(root);  
```  
  
 Dieses Beispiel erzeugt die folgende Ausgabe:  
  
```xml  
<Root>  
  <aw:Data xmlns:aw="http://www.adventure-works.com">  
    <aw:Child aw:MyAttr="content">content</aw:Child>  
  </aw:Data>  
  <ad:Data xmlns:ad="http://www.adatum.com">  
    <ad:Child ad:MyAttr="content">content</ad:Child>  
  </ad:Data>  
</Root>  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Modifying XML Trees (LINQ to XML) (C#) (Ändern von XML-Strukturen (LINQ to XML) (C#))](../../../../csharp/programming-guide/concepts/linq/modifying-xml-trees-linq-to-xml.md)

