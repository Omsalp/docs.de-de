---
title: "Zuordnungstyp | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 26c409f6-06e8-4441-ac78-1b1076a3c005
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# Zuordnungstyp
Ein *Zuordnungstyp* \(auch als Zuordnung bezeichnet\) ist der wesentliche Baustein zum Beschreiben von Beziehungen im Entity Data Model \(EDM\).  In einem konzeptionellen Modell stellt eine Zuordnung eine Beziehung zwischen zwei [Entitätstypen](../../../../docs/framework/data/adonet/entity-type.md) dar \(z. B. `Customer` und `Order`\).  In einer Anwendung stellt eine Instanz einer Zuordnung eine bestimmte Zuordnung dar \(z. B. eine Zuordnung zwischen einer Instanz von `Customer` und einer Instanz von `Order`\).  Zuordnungsinstanzen werden in einem [Zuordnungssatz](../../../../docs/framework/data/adonet/association-set.md) logisch gruppiert.  
  
 Eine Zuordnungsdefinition enthält die folgenden Informationen:  
  
-   Eine eindeutige Bezeichnung.  \(erforderlich\)  
  
-   Zwei [Zuordnungsenden](../../../../docs/framework/data/adonet/association-end.md), je ein Ende für jeden Entitätstyp in der Beziehung.  \(erforderlich\)  
  
    > [!NOTE]
    >  Eine Zuordnung kann keine Beziehung zwischen mehr als zwei Entitätstypen darstellen.  Eine Zuordnung kann jedoch eine Selbstbeziehung definieren, indem der gleichen Entitätstyp für beide Zuordnungsenden angegeben wird.  
  
-   Eine [Einschränkung der referenziellen Integrität](../../../../docs/framework/data/adonet/referential-integrity-constraint.md).  \(Optional\)  
  
 Jedes Zuordnungsende muss eine [Multiplizität des Zuordnungsendes](../../../../docs/framework/data/adonet/association-end-multiplicity.md) angeben, die die Anzahl möglicher Entitätstypinstanzen an einem Ende der Zuordnung angibt.  Die Multiplizität eines Zuordnungsendes kann über einen Wert von eins \(1\), null oder eins \(0..1\) oder n \(\*\) verfügen.  Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über [Navigationseigenschaften](../../../../docs/framework/data/adonet/navigation-property.md) oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.  Weitere Informationen finden Sie unter [Entity Data Model: Foreign Keys](../../../../docs/framework/data/adonet/foreign-key-property.md).  
  
## Beispiel  
 Die unten stehende Abbildung zeigt ein konzeptionelles Modell mit zwei Zuordnungen: `PublishedBy` und `WrittenBy`.  Die Zuordnungsenden für die `PublishedBy`\-Zuordnung sind die Entitätstypen `Book` und `Publisher`.  Die Multiplizität des `Publisher`\-Endes ist eins \(1\), und die Multiplizität des `Book`\-Endes ist n \(\*\), wodurch angegeben wird, dass ein Verleger viele Bücher veröffentlicht und ein Buch von einem Verleger veröffentlicht wird.  
  
 ![Beispielmodell](../../../../docs/framework/data/adonet/media/examplemodel.gif "ExampleModel")  
  
 Das [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md) verwendet eine domänenspezifische Sprache \(DSL\) mit der Bezeichnung konzeptionelle Schemadefinitionssprache \([CSDL](../../../../docs/framework/data/adonet/ef/language-reference/csdl-specification.md)\), um konzeptionelle Modelle zu definieren.  Die folgende CSDL definiert die in der Abbildung oben gezeigte `PublishedBy`\-Zuordnung:  
  
 [!code-xml[EDM_Example_Model#AssociationExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#associationexample)]  
  
## Siehe auch  
 [Schlüsselkonzepte im Entity Data Model](../../../../docs/framework/data/adonet/entity-data-model-key-concepts.md)   
 [Entity Data Model](../../../../docs/framework/data/adonet/entity-data-model.md)