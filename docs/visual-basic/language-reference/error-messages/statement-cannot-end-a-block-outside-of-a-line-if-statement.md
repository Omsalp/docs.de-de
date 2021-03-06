---
title: "Statement cannot end a block outside of a line &#39;If&#39; statement | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "vbc32005"
  - "bc32005"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "BC32005"
ms.assetid: 4039f51b-e0ee-4789-a89b-45d06de06b5d
caps.latest.revision: 8
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 8
---
# Statement cannot end a block outside of a line &#39;If&#39; statement
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Eine einzeilige `If`\-Anweisung enthält mehrere Anweisungen, die durch Doppelpunkte \(:\) voneinander getrennt sind; eine dieser Anweisungen ist eine `End`\-Anweisung für einen Kontrollblock außerhalb der einzeiligen `If`\-Anweisung.  Einzeilige `If`\-Anweisungen verwenden keine `End If`\-Anweisung.  
  
 **Fehler\-ID:** BC32005  
  
### So beheben Sie diesen Fehler  
  
-   Verschieben Sie die einzeilige `If`\-Anweisung aus dem Kontrollblock mit der `End If`\-Anweisung.  
  
## Siehe auch  
 [If...Then...Else Statement](../../../visual-basic/language-reference/statements/if-then-else-statement.md)