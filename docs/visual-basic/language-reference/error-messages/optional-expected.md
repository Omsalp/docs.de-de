---
title: "&#39;Optional&#39; expected | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "bc30202"
  - "vbc30202"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "BC30202"
ms.assetid: 6f75060c-2db4-4a79-b5d1-5780c09a74cd
caps.latest.revision: 9
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 9
---
# &#39;Optional&#39; expected
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Nach einem optionalen Argument in einer Prozedurdeklaration steht ein erforderliches Argument.  Jedes Argument, dass nach einem optionalen Argument steht, muss ebenfalls optional sein.  
  
 **Fehler\-ID:** BC30202  
  
### So beheben Sie diesen Fehler  
  
1.  Wenn das Argument erforderlich sein soll, verschieben Sie es, sodass es vor dem ersten optionalen Argument in der Argumentliste steht.  
  
2.  Wenn das Argument optional sein soll, verwenden Sie das `Optional`\-Schlüsselwort.  
  
## Siehe auch  
 [Optional Parameters](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)